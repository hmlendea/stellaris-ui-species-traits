# Component: Trait definitions

This document details the trait definition files, their structure, and the complete trait catalogue.

## Files

| File | Trait count | Scope |
|------|-------------|-------|
| `ui-species-traits/common/traits/ui-traits_species.txt` | 38 | Trait definitions |
| `ui-species-traits/common/traits/ui-traits_species_newtiers.txt` | 20 | Additional Very and Extremely tiers |

## File structure

Both files use Clausewitz script. The two field sets differ:

```clausewitz
# ui-traits_species.txt
ui_trait_<identifier> = {
    cost = <integer>
    opposites = { "<identifier>" ... }
    allowed_archetypes = { <ARCHETYPE> ... }
    modifier = {
        <modifier_key> = <value>
    }
}

# ui-traits_species_newtiers.txt
ui_trait_<identifier> = {
    cost = <integer>
    category = normal
    species_potential_add = { ... }
    species_possible_add = { ... }
    species_possible_remove = { ... }
    species_possible_merge_remove = { ... }
    opposites = { "<identifier>" ... }
    allowed_archetypes = { <ARCHETYPE> ... }
    tags = { organic <positive|negative> <group> ... }
    modifier = {
        <modifier_key> = <value>
    }
    slave_cost = {
        trade = <integer>
    }
    assembly_score = { ... }
    ai_weight = { weight = <number> }
}
```

Neither file declares an `icon` field; the engine resolves icons by filename. See [Components/assets.md](./assets.md).

## ui-traits_species.txt — 38 trait definitions

Entries appear in the order in which they were added, grouped loosely by family: infrastructure and upkeep first, then combat, social, agility, food, and unity.

### Infrastructure and upkeep

| Identifier | Cost | Allowed archetypes | Modifiers |
|------------|------|--------------------|-----------|
| `ui_trait_urban` | -2 | `BIOLOGICAL` | `planet_jobs_food_produces_mult = -0.15` |
| `ui_trait_uneconomical` | -2 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_energy_produces_mult = -0.15` |
| `ui_trait_lazy` | -2 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_minerals_produces_mult = -0.15` |
| `ui_trait_nonresilient` | -1 | `BIOLOGICAL`, `LITHOID` | `army_defense_damage_mult = -0.5` |

### Research

| Identifier | Cost | Allowed archetypes | Modifiers |
|------------|------|--------------------|-----------|
| `ui_trait_dumb` | -2 | `BIOLOGICAL`, `LITHOID` | three `planet_jobs_*_research_produces_mult` at `-0.1` |
| `ui_trait_poor_engineers` | -1 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_engineering_research_produces_mult = -0.15` |
| `ui_trait_poor_physicists` | -1 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_physics_research_produces_mult = -0.15` |
| `ui_trait_poor_sociologists` | -1 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_society_research_produces_mult = -0.15` |

### Combat

| Identifier | Cost | Allowed archetypes | Modifiers |
|------------|------|--------------------|-----------|
| `ui_trait_brave` | 2 | `BIOLOGICAL`, `LITHOID` | `army_morale = 0.15`, `army_damage_mult = 0.15` |
| `ui_trait_craven` | -2 | `BIOLOGICAL`, `LITHOID` | `army_morale = -0.15`, `army_damage_mult = -0.15` |
| `ui_trait_agile` | 2 | `BIOLOGICAL`, `LITHOID` | three weapon fire-rate modifiers at `0.05` |
| `ui_trait_very_agile` | 6 | `BIOLOGICAL`, `LITHOID` | three weapon fire-rate modifiers at `0.091667` |
| `ui_trait_extremely_agile` | 8 | `BIOLOGICAL`, `LITHOID` | three weapon fire-rate modifiers at `0.11` |
| `ui_trait_sluggish` | -2 | `BIOLOGICAL`, `LITHOID` | three weapon fire-rate modifiers at `-0.05` |
| `ui_trait_very_sluggish` | -3 | `BIOLOGICAL`, `LITHOID` | three weapon fire-rate modifiers at `-0.091667` |
| `ui_trait_extremely_sluggish` | -4 | `BIOLOGICAL`, `LITHOID` | three weapon fire-rate modifiers at `-0.11` |

The weapon modifiers are `weapon_type_kinetic_weapon_fire_rate_mult`, `weapon_type_energy_weapon_fire_rate_mult`, and `weapon_type_explosive_weapon_fire_rate_mult`.

### Social conduct

| Identifier | Cost | Allowed archetypes | Modifiers |
|------------|------|--------------------|-----------|
| `ui_trait_deceitful` | 2 | `BIOLOGICAL`, `LITHOID` | `country_influence_produces_mult = 0.1` |
| `ui_trait_trusting` | -2 | `BIOLOGICAL`, `LITHOID` | `country_influence_produces_mult = -0.1` |
| `ui_trait_optimistic` | 1 | `BIOLOGICAL`, `LITHOID` | `pop_happiness = 0.05` |
| `ui_trait_pessimistic` | -1 | `BIOLOGICAL`, `LITHOID` | `pop_happiness = -0.05` |

### Food upkeep

| Identifier | Cost | Allowed archetypes | Modifiers |
|------------|------|--------------------|-----------|
| `ui_trait_light_eaters` | 1 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = -0.10` |
| `ui_trait_very_light_eaters` | 3 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = -0.183333` |
| `ui_trait_extremely_light_eaters` | 4 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = -0.22` |
| `ui_trait_gluttonous` | -1 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = 0.10` |
| `ui_trait_very_gluttonous` | -2 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = 0.183333` |
| `ui_trait_extremely_gluttonous` | -3 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = 0.22` |

### Unity and amenities

| Identifier | Cost | Allowed archetypes | Modifiers |
|------------|------|--------------------|-----------|
| `ui_trait_creative` | 2 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_unity_produces_mult = 0.10`, `planet_amenities_mult = 0.10` |
| `ui_trait_very_creative` | 6 | `BIOLOGICAL`, `LITHOID` | both modifiers at `0.183333` |
| `ui_trait_extremely_creative` | 8 | `BIOLOGICAL`, `LITHOID` | both modifiers at `0.22` |
| `ui_trait_uncreative` | -2 | `BIOLOGICAL`, `LITHOID` | both modifiers at `-0.10` |
| `ui_trait_very_uncreative` | -3 | `BIOLOGICAL`, `LITHOID` | both modifiers at `-0.183333` |
| `ui_trait_extremely_uncreative` | -4 | `BIOLOGICAL`, `LITHOID` | both modifiers at `-0.22` |

### Housing, amenities, and consumer goods

| Identifier | Cost | Allowed archetypes | Modifiers |
|------------|------|--------------------|-----------|
| `ui_trait_demanding` | -1 | `BIOLOGICAL`, `LITHOID` | `pop_amenities_usage_mult = 0.10`, `planet_pops_consumer_goods_upkeep_mult = 0.01`, `pop_housing_usage_mult = 0.01` |
| `ui_trait_very_demanding` | -2 | `BIOLOGICAL`, `LITHOID` | same three at `0.183333`, `0.018333`, `0.018333` |
| `ui_trait_extremely_demanding` | -3 | `BIOLOGICAL`, `LITHOID` | same three at `0.22`, `0.022`, `0.022` |
| `ui_trait_undemanding` | 1 | `BIOLOGICAL`, `LITHOID` | same three at `-0.10`, `-0.01`, `-0.01` |
| `ui_trait_very_undemanding` | 3 | `BIOLOGICAL`, `LITHOID` | same three at `-0.183333`, `-0.018333`, `-0.018333` |
| `ui_trait_extremely_undemanding` | 4 | `BIOLOGICAL`, `LITHOID` | same three at `-0.22`, `-0.022`, `-0.022` |

## ui-traits_species_newtiers.txt — 20 additional tiers

These entries extend vanilla trait families and each carries a `# Name:` and `# Group:` comment above the block.

### Adaptability

| Identifier | Cost | Modifier | Tags |
|------------|------|----------|------|
| `ui_trait_very_adaptive` | 3 | `pop_environment_tolerance = 0.166667` | `organic positive habitability` |
| `ui_trait_very_nonadaptive` | -3 | `pop_environment_tolerance = -0.183333` | `organic negative habitability` |
| `ui_trait_extremely_nonadaptive` | -4 | `pop_environment_tolerance = -0.22` | `organic negative habitability` |

### Breeding

| Identifier | Cost | Modifier | Tags |
|------------|------|----------|------|
| `ui_trait_very_rapid_breeders` | 6 | `logistic_growth_mult = 0.183333` | `organic positive pop_growth` |
| `ui_trait_extremely_rapid_breeders` | 8 | `logistic_growth_mult = 0.22` | `organic positive pop_growth` |
| `ui_trait_very_slow_breeders` | -3 | `logistic_growth_mult = -0.183333` | `organic negative pop_growth` |
| `ui_trait_extremely_slow_breeders` | -4 | `logistic_growth_mult = -0.22` | `organic negative pop_growth` |

### Civic Behaviour

| Identifier | Cost | Modifier | Tags |
|------------|------|----------|------|
| `ui_trait_very_docile` | 6 | `species_empire_size_mult = -0.183333` | `organic positive empire_size` |
| `ui_trait_very_unruly` | -3 | `species_empire_size_mult = 0.183333` | `organic negative empire_size` |

### Lifespan

| Identifier | Cost | Modifier | Tags |
|------------|------|----------|------|
| `ui_trait_long_lived` | 3 | `leader_lifespan_add = 36.666667` | `organic positive leader` |
| `ui_trait_very_long_lived` | 4 | `leader_lifespan_add = 44` | `organic positive leader` |
| `ui_trait_momentary` | -4 | `leader_lifespan_add = -26.4` | `organic negative leader` |
| `ui_trait_short_lived` | -2 | `leader_lifespan_add = -18.333333` | `organic negative leader` |
| `ui_trait_very_short_lived` | -3 | `leader_lifespan_add = -22` | `organic negative leader` |

The Lifespan family is additive rather than multiplicative, so no scaling ratio applies.

### Social Cohesion

| Identifier | Cost | Modifier | Tags |
|------------|------|----------|------|
| `ui_trait_very_traditional` | 3 | `bureaucrat_jobs_bonus_workforce_mult = 0.183333` | `organic positive pop_output unity` |
| `ui_trait_very_quarrelsome` | -2 | `bureaucrat_jobs_bonus_workforce_mult = -0.183333` | `organic negative pop_output unity` |

### Strength

| Identifier | Cost | Modifiers | Tags |
|------------|------|-----------|------|
| `ui_trait_extremely_strong` | 4 | `army_damage_mult = 0.48`, `worker_and_simple_drone_cat_bonus_workforce_mult = 0.06`, `soldier_jobs_bonus_workforce_mult = 0.18` | `organic positive pop_output army` |
| `ui_trait_very_weak` | -2 | `army_damage_mult = -0.366667`, `worker_and_simple_drone_cat_bonus_workforce_mult = -0.045833`, `soldier_jobs_bonus_workforce_mult = -0.183333` | `organic negative pop_output army` |
| `ui_trait_extremely_weak` | -3 | `army_damage_mult = -0.44`, `worker_and_simple_drone_cat_bonus_workforce_mult = -0.055`, `soldier_jobs_bonus_workforce_mult = -0.22` | `organic negative pop_output army` |

The Strength family extends vanilla `trait_strong` and `trait_weak`; `ui_trait_extremely_strong` corresponds to the vanilla scaling that already supplies an Extremely tier.

### Talent

| Identifier | Cost | Modifiers | Tags |
|------------|------|-----------|------|
| `ui_trait_very_talented` | 3 | `negative_leader_traits_species = -2`, `leaders_upkeep_mult = -0.183333` | `organic positive leader` |

## Opposites

Each trait declares `opposites` listing mutually exclusive identifiers. The stated patterns are:

- Paired opposites within the mod, such as `ui_trait_brave` and `ui_trait_craven`, or `ui_trait_deceitful` and `ui_trait_trusting`.
- Tier families that oppose every vanilla and custom member of the same family, such as `ui_trait_extremely_nonadaptive`, which lists `trait_nonadaptive`, `trait_adaptive`, `trait_extremely_adaptive`, `trait_robust`, `trait_adaptive_lithoid`, `trait_shelled`, `trait_permeable_skin`, `trait_genetic_memory`, `trait_pathogenic_genes`, `ui_trait_very_adaptive`, and `ui_trait_very_nonadaptive`.
- Cross-family references where a vanilla trait occupies an overlapping role, such as `ui_trait_dumb` opposing `trait_intelligent`, `trait_nerve_stapled`, and `trait_erudite`.

Every identifier in `opposites` must exist in the targeted game version. No tooling verifies this.

## Allowed archetypes

Every entry declares either `{ BIOLOGICAL }` or `{ BIOLOGICAL LITHOID }`. No entry declares `MACHINE` or `ROBOT`, so machine and robot species never see these traits regardless of their trait points.

## Tags

`tags` appears only in `ui-traits_species_newtiers.txt` and always begins with `organic`, followed by `positive` or `negative` and one or more group tags.

| Tag observed | Meaning |
|--------------|---------|
| `organic` | Applies to organic species |
| `positive` | Costs trait points |
| `negative` | Grants trait points |
| `habitability` | Alters environment tolerance |
| `pop_growth` | Alters population growth |
| `empire_size` | Alters empire size contribution |
| `leader` | Alters leader lifespan, upkeep, or trait count |
| `army` | Alters army performance |
| `unity` | Alters unity production |
| `pop_output` | Alters pop workforce production |

## Eligibility blocks and AI weighting

- `species_potential_add` appears in 11 entries and gates whether the trait can be offered at all.
- `species_possible_add`, `species_possible_remove`, and `species_possible_merge_remove` appear in 9 entries and gate genetic modification.
- `ai_weight` appears in 10 entries; negative tiers mostly declare `weight = 0`, so the AI does not select them deliberately.
- `assembly_score` appears in two Strength entries and conditions robot assembly weight on planet designation.
- `potential_crossbreeding_chance` appears in three entries in `ui-traits_species.txt`.

## Maintenance notes

- **Adding a trait definition:** add the block to `ui-traits_species.txt`, add both localisation keys, add the icon, and update the `opposites` of every conflicting trait.
- **Adding a tier:** add the block to `ui-traits_species_newtiers.txt` following the header policy, then add the localisation keys and icon, and verify that every vanilla family member appears in `opposites`.
- **Changing a trait:** update the modifier values and cost, then update the description if the flavour text no longer fits.
- **Removing a trait:** delete the block, both localisation keys, and the icon, then remove the identifier from every `opposites` list.

## Related documentation

| Document | Scope |
|----------|-------|
| [Data model](../data-model.md) | Schema, scaling policy, archetype variables |
| [Architecture](../architecture.md) | Trait definition area, interfaces |
| [Components/localisation.md](./localisation.md) | Localisation keys and formatting |
| [Components/assets.md](./assets.md) | Icon convention and artwork workflow |