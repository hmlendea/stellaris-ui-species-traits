# Behaviour: Trait effects

This document describes how trait modifiers are applied in game, their scope, stacking, and relevant interactions.

## Application model

Every trait effect is a **static modifier** declared in the `modifier` block. The engine applies these for as long as the species exists.

```clausewitz
modifier = {
    army_morale = 0.15
    army_damage_mult = 0.15
}
```

### Scope

The modifier key determines the scope; the trait itself declares no scope.

| Modifier family | Scope | Examples in this mod |
|-----------------|-------|----------------------|
| `army_*` | Armies recruited from the species | `army_damage_mult`, `army_morale`, `army_defense_damage_mult` |
| `weapon_type_*_fire_rate_mult` | Ships crewed by the species | kinetic, energy, and explosive fire rate |
| `planet_jobs_*` | Jobs on planets holding the pops | `planet_jobs_energy_produces_mult`, `planet_jobs_unity_produces_mult` |
| `planet_pops_*` | Organic pops on the planet | `planet_pops_organics_food_upkeep_mult` |
| `planet_amenities_mult` | Planet amenities | `planet_amenities_mult` |
| `pop_*` | Individual pops | `pop_happiness`, `pop_environment_tolerance`, `pop_housing_usage_mult` |
| `country_*` | The empire | `country_influence_produces_mult` |
| `leader_*`, `leaders_*` | Leaders | `leader_lifespan_add`, `leaders_upkeep_mult` |
| `logistic_growth_mult` | Population growth | `logistic_growth_mult` |
| `species_empire_size_mult` | Empire size contribution | `species_empire_size_mult` |
| `bureaucrat_jobs_bonus_workforce_mult` | Bureaucrat workforce | `bureaucrat_jobs_bonus_workforce_mult` |
| `negative_leader_traits_species` | Leader trait pool | `negative_leader_traits_species` |

### Timing

- Modifiers are applied when the species is created.
- New pops inherit their species modifiers immediately.
- Genetic modification recalculates the modifier set from the new trait list.
- On load, modifier values come from the current trait definitions rather than from the save.

## Multiplicative and additive modifiers

| Modifier | Kind | Effect |
|----------|------|--------|
| `army_damage_mult` | Multiplicative | Scales army damage |
| `army_morale` | Multiplicative | Scales army morale |
| `army_defense_damage_mult` | Multiplicative | Scales defensive army damage |
| `planet_jobs_energy_produces_mult` | Multiplicative | Scales energy job production |
| `planet_jobs_minerals_produces_mult` | Multiplicative | Scales mineral job production |
| `planet_jobs_food_produces_mult` | Multiplicative | Scales food job production |
| `planet_jobs_unity_produces_mult` | Multiplicative | Scales unity job production |
| `planet_jobs_*_research_produces_mult` | Multiplicative | Scales research job production per field |
| `planet_pops_organics_food_upkeep_mult` | Multiplicative | Scales organic food upkeep |
| `planet_amenities_mult` | Multiplicative | Scales amenities |
| `pop_amenities_usage_mult` | Multiplicative | Scales amenities usage |
| `pop_housing_usage_mult` | Multiplicative | Scales housing usage |
| `pop_happiness` | Additive | Adjusts happiness |
| `pop_environment_tolerance` | Additive | Adjusts habitability |
| `country_influence_produces_mult` | Multiplicative | Scales influence production |
| `logistic_growth_mult` | Multiplicative | Scales population growth |
| `species_empire_size_mult` | Multiplicative | Scales empire size contribution |
| `bureaucrat_jobs_bonus_workforce_mult` | Multiplicative | Scales bureaucrat workforce bonus |
| `leaders_upkeep_mult` | Multiplicative | Scales leader upkeep |
| `negative_leader_traits_species` | Additive | Subtracts from the negative leader trait pool |
| `weapon_type_*_weapon_fire_rate_mult` | Multiplicative | Scales weapon fire rate per weapon class |

`planet_pops_consumer_goods_upkeep_mult` appears in the housing and consumer goods family, but the repository records no scope note for it beyond its use on planet pops.

## Stacking

| Source combination | Stacking |
|--------------------|----------|
| Two traits with the same modifier | Additive sum |
| Trait and tradition | Additive sum |
| Trait and ascension perk | Additive sum |
| Trait and technology | Additive sum |
| Trait and event or planet modifier | Additive sum |

A species cannot hold the same trait twice, and `opposites` prevents holding two traits from one family, so duplication through the mod's own entries is not possible.

### Example

```
Vanilla trait_strong        army_damage_mult = 0.2
ui_trait_extremely_strong   army_damage_mult = 0.48
Supremacy tradition         army_damage_mult = 0.2
```

Note that `trait_strong` and `ui_trait_extremely_strong` are mutually exclusive through `opposites`, so the sum above cannot arise from those two traits; the example illustrates additive combination only.

## Family detail

### Combat and army

| Identifier | Modifiers |
|------------|-----------|
| `ui_trait_brave` | `army_morale = 0.15`, `army_damage_mult = 0.15` |
| `ui_trait_craven` | `army_morale = -0.15`, `army_damage_mult = -0.15` |
| `ui_trait_nonresilient` | `army_defense_damage_mult = -0.5` |
| `ui_trait_agile` family | three weapon fire-rate modifiers, positive for agile and negative for sluggish |
| `ui_trait_very_weak`, `ui_trait_extremely_weak`, `ui_trait_extremely_strong` | army damage plus worker and soldier workforce modifiers |

### Economy and upkeep

| Identifier | Modifiers |
|------------|-----------|
| `ui_trait_demanding` family | amenities usage, consumer goods upkeep, housing usage |
| `ui_trait_undemanding` family | the same three modifiers with the opposite sign |
| `ui_trait_gluttonous` family | organic food upkeep, increased upkeep |
| `ui_trait_light_eaters` family | organic food upkeep, reduced upkeep |
| `ui_trait_urban` | food job production penalty |
| `ui_trait_uneconomical` | energy job production penalty |
| `ui_trait_lazy` | mineral job production penalty |

### Research, unity, and amenities

| Identifier | Modifiers |
|------------|-----------|
| `ui_trait_dumb` | a `-0.1` penalty to each research field |
| `ui_trait_poor_engineers`, `ui_trait_poor_physicists`, `ui_trait_poor_sociologists` | a `-0.15` penalty to one research field each |
| `ui_trait_creative` family | unity production and amenities |
| `ui_trait_uncreative` family | the same two modifiers with the opposite sign |

### Habitability, growth, and lifespan

| Identifier | Modifiers |
|------------|-----------|
| `ui_trait_very_adaptive`, `ui_trait_very_nonadaptive`, `ui_trait_extremely_nonadaptive` | `pop_environment_tolerance` |
| `ui_trait_very_rapid_breeders`, `ui_trait_extremely_rapid_breeders`, `ui_trait_very_slow_breeders`, `ui_trait_extremely_slow_breeders` | `logistic_growth_mult` |
| `ui_trait_long_lived`, `ui_trait_very_long_lived`, `ui_trait_momentary`, `ui_trait_short_lived`, `ui_trait_very_short_lived` | `leader_lifespan_add` |

### Empire and governance

| Identifier | Modifiers |
|------------|-----------|
| `ui_trait_very_docile`, `ui_trait_very_unruly` | `species_empire_size_mult` |
| `ui_trait_very_traditional`, `ui_trait_very_quarrelsome` | `bureaucrat_jobs_bonus_workforce_mult` |
| `ui_trait_very_talented` | `negative_leader_traits_species`, `leaders_upkeep_mult` |

## Interaction with game systems

| System | Interaction |
|--------|-------------|
| Species rights | Trait modifiers apply under every species right |
| Slave market | `slave_cost` on the additional tiers defines the trade value exchanged |
| Genetic modification | Eligibility blocks decide which traits may be added or removed |
| Robot assembly | `assembly_score` on two Strength tiers conditions assembly weight on planet designation |
| Crossbreeding | `ui-traits_species.txt` declares `potential_crossbreeding_chance` on three entries |
| Events | Scripted modifiers stack additively with trait modifiers |

## Performance

Static modifiers impose no per-tick computation after the initial application; the engine aggregates them per species, planet, army, and country. The 58 trait definitions are negligible in number relative to vanilla content.

## Verification

1. Create a species with a known trait and record the effective value.
2. Hover a resource in the planet view to inspect the modifier breakdown.
3. Use the in-game console modifier commands to compare species, pop, planet, and country totals against the trait definitions.
4. Change one modifier value in the file, restart, and confirm that an existing save reflects the new value.

## Related documentation

| Document | Scope |
|----------|-------|
| [Components/trait-definitions.md](../components/trait-definitions.md) | Modifier values per trait |
| [Data model](../data-model.md) | Scaling policy for tier modifiers |
| [Flows/trait-selection.md](../flows/trait-selection.md) | Selection constraints |
| [Architecture](../architecture.md) | Data architecture |