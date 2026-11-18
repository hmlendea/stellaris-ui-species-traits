# Flow: Trait selection

This document details trait selection logic, validation, and persistence during species creation and empire generation.

## Selection contexts

| Context | Actor | Trait pool | Constraints |
|---------|-------|------------|-------------|
| Player empire creation | Player | Traits allowed for the archetype | Points, count, opposites |
| AI empire generation | AI | Traits allowed for the archetype | Points, count, opposites, `ai_weight` |
| Robot assembly | Player or AI | Traits allowed for `MACHINE` or `ROBOT` | Points, count, opposites |
| Genetic modification | Player | Current traits plus eligible additions | Points, count, opposites, eligibility blocks |
| Scripted assignment | Event or anomaly script | Named identifiers | Script-defined |

## Player selection

### Budget tracking

```
available_points = archetype.trait_points
selected = []
count = 0

on_select(trait):
    if count >= archetype.max_traits: refuse("maximum traits reached")
    if available_points - trait.cost < 0: refuse("insufficient trait points")
    if any(selected.opposites contains trait): refuse("conflicts with selection")

    selected.append(trait)
    count += 1
    available_points -= trait.cost
    disable(trait.opposites)
    accept()

on_deselect(trait):
    selected.remove(trait)
    count -= 1
    available_points += trait.cost
    enable(trait.opposites)
```

Selection offers affordability feedback, and every trait disabled by an `opposites` entry becomes unselectable until the opposing trait is removed.

## AI selection

The AI uses `ai_weight` where a trait declares it. Ten entries in `ui-traits_species_newtiers.txt` declare a weight block, and the negative tiers mostly declare `weight = 0`, so the AI avoids them deliberately. The 38 entries in `ui-traits_species.txt` declare no weight, leaving the engine's default.

### Selection outline

```
candidates = traits allowed for the archetype
budget = archetype.trait_points
count = 0

for trait in candidates by weight:
    if count >= archetype.max_traits: stop
    if budget - trait.cost < 0: continue
    if conflicts(trait, selected): continue

    select(trait)
    budget -= trait.cost
    count += 1
    disable(trait.opposites)
```

Because the mod traits are limited to `BIOLOGICAL` and `LITHOID`, AI machine and robot empires never select them, and AI biological empires can reach at most the same point budget as the player.

## Opposites

### Structure of the exclusion

- **Symmetry:** the files declare both directions for paired traits, such as `ui_trait_brave` and `ui_trait_craven`.
- **Family completeness:** a tier entry lists every senior and junior member of its family, including the vanilla members, so a species is unable to hold two tiers of one family.
- **Cross-family references:** an entry may oppose vanilla traits that occupy an overlapping role, such as `ui_trait_dumb` opposing `trait_intelligent`, `trait_nerve_stapled`, and `trait_erudite`.

### Conflict handling

1. Player selection disables the opposing traits immediately.
2. AI selection skips a candidate that conflicts with an existing choice.
3. Scripted assignment removes conflicting traits first.

### Example family exclusion

```clausewitz
ui_trait_extremely_nonadaptive = {
    cost = -4
    opposites = { "trait_nonadaptive" "trait_adaptive" "trait_extremely_adaptive"
                  "trait_robust" "trait_adaptive_lithoid" "trait_shelled"
                  "trait_permeable_skin" "trait_genetic_memory"
                  "trait_pathogenic_genes" "ui_trait_very_adaptive"
                  "ui_trait_very_nonadaptive" }
}
```

## Genetic modification

When the player modifies a species through genetic engineering:

1. The current traits form the baseline.
2. The available additions are the archetype's traits minus those excluded by `opposites`.
3. The budget is the trait point budget of the archetype.
4. Eligibility blocks decide which traits may be added, removed, or dropped during merging:
   - `species_possible_add` (9 entries) gates acquisition of a beneficial trait, for example `can_add_genetic_traits = yes`.
   - `species_possible_remove` (9 entries) gates removal, for example `can_remove_beneficial_genetic_traits = yes`.
   - `species_possible_merge_remove` (9 entries) gates loss during species merging, mostly `always = yes`.
   - `species_potential_add` (11 entries) gates whether the trait is offered at all, for example `can_add_negative_organic_traits = yes`.
5. The final state is validated against the point budget, the maximum count, and `opposites`.

### Mod-specific conduct

- Mod traits can be added and removed like vanilla traits, subject to the eligibility blocks above.
- Moving between tiers of a family costs the difference in trait points.
- A species template change propagates to every pop of that species.

## Scripted assignment

Events and anomalies may grant a trait by identifier:

```clausewitz
add_trait = ui_trait_very_strong
```

Scripted assignment bypasses the point budget and the maximum count, and it removes conflicting opposites. The mod traits behave like vanilla traits here; no mod-side registration is required.

## Save-game persistence

Selected identifiers are stored with the species in the save game:

```clausewitz
species = {
    traits = { trait_strong ui_trait_agile ui_trait_rapid_breeders }
}
```

On load the engine resolves each identifier against the current trait registry:

- An identifier that no longer exists is lost.
- Modifier values are read from the current definitions rather than from the save, so a rebalance applies to existing saves.

## Multiplayer

Trait selections travel with the empire design, and every participant must run the same mod version. A version mismatch prevents joining or causes a desynchronisation, because the mod's descriptor and file contents contribute to the game checksum.

## Verification scenarios

| Scenario | Expected result |
|----------|-----------------|
| Eighth trait selected with a limit of seven | Selection refused |
| Trait costing 3 selected with 2 points | Selection refused |
| `ui_trait_agile` followed by `ui_trait_sluggish` | Second refused; first retained |
| AI machine empire offered a mod trait | Never offered, because of `allowed_archetypes` |
| Script grants `ui_trait_extremely_strong` | Applied, conflicting traits removed, limits may be exceeded |
| Save loaded without the mod | Mod traits lost |

## Related documentation

| Document | Scope |
|----------|-------|
| [Species creation flow](./species-creation.md) | Interface flow, archetype determination |
| [Components/trait-definitions.md](../components/trait-definitions.md) | Costs, opposites, eligibility blocks |
| [Components/archetype-config.md](../components/archetype-config.md) | Archetype limits |