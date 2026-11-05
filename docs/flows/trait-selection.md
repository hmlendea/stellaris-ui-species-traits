# Flow: Trait selection

This document details the trait selection logic, validation, and AI behaviour during species creation and empire generation.

## Selection contexts

| Context | Actor | Trait pool | Constraints |
|---------|-------|------------|-------------|
| Player empire creation | Human | All traits for archetype | Points, count, opposites |
| AI empire generation | AI | All traits for archetype | Points, count, opposites, `ai_weight` |
| Robot assembly | Player/AI | Machine/ROBOT traits | Points, count, opposites |
| Gene modification | Player | Current species traits + available | Points, count, opposites |
| Event/Anomaly rewards | Script | Specific trait IDs | Script-defined |

## Player selection algorithm

### Budget tracking

```
available_points = archetype.trait_points
selected_traits = []
selected_count = 0

on_trait_select(trait):
    if selected_count >= archetype.max_traits:
        return REJECT "Max traits reached"
    if available_points - trait.cost < 0:
        return REJECT "Insufficient trait points"
    if any(t in selected_traits for t in trait.opposites):
        return REJECT "Conflicts with selected trait"

    selected_traits.add(trait)
    selected_count += 1
    available_points -= trait.cost
    disable_traits(trait.opposites)
    return ACCEPT

on_trait_deselect(trait):
    selected_traits.remove(trait)
    selected_count -= 1
    available_points += trait.cost
    enable_traits(trait.opposites)  # if no other selected trait opposes them
```

### UI feedback

- **Green highlight:** Affordable, no conflicts
- **Red highlight:** Exceeds points or conflicts
- **Grayed out:** Opposite of selected trait
- **Tooltip on hover:** Shows cost, modifiers, conflicts

## AI selection algorithm

AI uses `ai_weight` as primary selection heuristic, modified by archetype and empire focus.

### Weight calculation

```
effective_weight = trait.ai_weight * archetype_modifier * focus_modifier * random_factor
```

| Factor | Values |
|--------|--------|
| `archetype_modifier` | BIOLOGICAL/LITHOID: 1.0, MACHINE: 0.8, ROBOT: 0.5 |
| `focus_modifier` | Militarist: +military traits, Materialist: +research traits, etc. |
| `random_factor` | 0.8–1.2 (prevents deterministic picks) |

### Selection loop (simplified)

```
available = filter_traits_by_archetype(all_traits, archetype)
available = sort_by_effective_weight(available)
budget = archetype.trait_points
count = 0

for trait in available:
    if count >= archetype.max_traits: break
    if budget - trait.cost < 0: continue
    if conflicts_with_selected(trait, selected): continue
    if random() > selection_probability(trait): continue

    selected.add(trait)
    budget -= trait.cost
    count += 1
    disable_opposites(trait.opposites)
```

### AI preferences by archetype

| Archetype | Preferred traits | Avoided traits |
|-----------|------------------|----------------|
| BIOLOGICAL | `ui_trait_very_strong`, `ui_trait_very_intelligent`, `ui_trait_very_rapid_breeders` | High-cost negatives unless needed for points |
| LITHOID | Same as BIOLOGICAL + `ui_trait_very_adaptive` | — |
| MACHINE | `ui_trait_very_natural_engineers`, `ui_trait_very_natural_physicists` | Positive physical/social (not allowed) |
| ROBOT | Negative industry traits (grant points) | Positive traits (0 budget) |

## Opposites resolution

### Mutual exclusion graph

Traits form a conflict graph via `opposites`. Key properties:

- **Symmetric:** If A opposes B, B opposes A (enforced by mod author)
- **Transitive within families:** Base ↔ Very ↔ Extremely all mutually oppose
- **Cross-family:** Some traits oppose across categories (e.g., `ui_trait_brave` ↔ `ui_trait_craven`)

### Conflict resolution order

1. Player selection: Immediate — opposite traits disabled instantly
2. AI selection: Check before adding — skip if conflicts
3. Script assignment: Force — removes conflicting traits first

### Tier family conflicts

Each vanilla-extended family forms a complete mutual exclusion clique:

```
trait_strong ↔ trait_weak
trait_strong ↔ ui_trait_very_strong
trait_strong ↔ ui_trait_extremely_strong
trait_weak ↔ ui_trait_very_weak
trait_weak ↔ ui_trait_extremely_weak
ui_trait_very_strong ↔ ui_trait_very_weak
ui_trait_very_strong ↔ ui_trait_extremely_strong
ui_trait_very_strong ↔ ui_trait_extremely_weak
... (all pairs)
```

Player/AI can select **at most one** trait from each family.

## Gene modification flow

When player uses gene clinics to modify species:

1. Current traits = baseline
2. Available traits = all for archetype minus current opposites
3. Budget = current trait points spent (can go negative temporarily)
4. Player adds/removes traits
5. On confirm: validate final state (points ≤ archetype limit, count ≤ max_traits)
6. Cost: Unity + Time based on changes

### Mod-specific considerations

- Custom traits can be added/removed like vanilla traits
- Tier traits: switching from base → Very costs trait point difference
- `opposites` enforced during modification
- Species template updates; all pops of that species update

## Event/script trait assignment

Some events/anomalies grant traits via script:

```clausewitz
# Example event effect
add_trait = ui_trait_very_strong
```

Behaviour:
- Ignores trait points and max_traits limits
- Removes opposites automatically
- Can exceed normal limits (temporary or permanent)
- Mod traits work identically to vanilla traits in scripts

## Save game persistence

Selected traits stored in save game as trait ID list:

```clausewitz
species = {
    traits = { trait_strong ui_trait_agile ui_trait_rapid_breeders }
    ...
}
```

On load:
- Engine reads trait IDs
- Looks up each ID in current mod+vanilla registry
- Missing IDs (mod disabled/updated): trait lost, warning logged
- Modifiers applied from current trait definitions (not saved values)

## Multiplayer synchronisation

- Trait selections synced via empire design at game start
- All players must have same mod version enabled
- Mod checksum verified in lobby
- Mismatch → cannot join / desync

## Testing scenarios

| Scenario | Expected result |
|----------|-----------------|
| Player picks 8 traits (max 7) | 8th rejected |
| Player picks trait costing 3 with 2 points | Rejected |
| Player picks `ui_trait_agile` then `ui_trait_sluggish` | Second rejected, first stays |
| AI Machine empire gets `ui_trait_very_natural_engineers` | Only if budget allows (cost 6 > 1 point → never) |
| Gene mod: remove negative, add positive | Points recalculated, validated on confirm |
| Event grants `ui_trait_extremely_strong` | Added, opposites removed, may exceed limits |
| Load save with mod disabled | Traits missing, species weakened |

## Related documentation

| Document | Scope |
|----------|-------|
| [Species creation flow](./species-creation.md) | UI flow, archetype determination |
| [Components/trait-definitions.md](../components/trait-definitions.md) | Trait fields: cost, opposites, ai_weight |
| [Components/archetype-config.md](../components/archetype-config.md) | Archetype limits |
| [Data model](../data-model.md) | Scaling policy, trait catalogue |