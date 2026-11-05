# Component: Trait definitions

This document provides a deep dive into the trait definition files, their structure, and the complete trait catalogue.

## Files

| File | Trait count | Scope |
|------|-------------|-------|
| `ui-species-traits/common/traits/ui-traits_species.txt` | 38 | Custom traits (original mod content) |
| `ui-species-traits/common/traits/ui-traits_species_newtiers.txt` | 20 | Very/Extremely tier extensions for vanilla families |

## File structure

Both files follow the same Clausewitz script format. Each trait is a top-level block:

```clausewitz
ui_trait_<id> = {
    cost = <int>
    category = "<string>"
    opposites = { <trait_id> ... }
    allowed_archetypes = { <arch> ... }
    tags = { <tag> ... }
    modifier = {
        <modifier_key> = <value>
        ...
    }
    slave_cost = <int>
    ai_weight = <float>
}
```

## ui-traits_species.txt — 38 custom traits

### Positive traits (19)

| Trait ID | Cost | Category | Key modifiers |
|----------|------|----------|---------------|
| `ui_trait_agile` | 2 | physical | `army_damage_mult = 0.1`, `ship_evasion_mult = 0.05` |
| `ui_trait_brave` | 2 | military | `army_damage_mult = 0.15`, `army_morale_mult = 0.1` |
| `ui_trait_creative` | 2 | social | `research_speed_mult = 0.1` |
| `ui_trait_demanding` | -2 | social | `pop_consumer_goods_upkeep_mult = 0.2` |
| `ui_trait_extremely_agile` | 8 | physical | `army_damage_mult = 0.24`, `ship_evasion_mult = 0.12` |
| `ui_trait_extremely_creative` | 8 | social | `research_speed_mult = 0.24` |
| `ui_trait_extremely_demanding` | -8 | social | `pop_consumer_goods_upkeep_mult = 0.8` |
| `ui_trait_extremely_gluttonous` | -8 | biological | `pop_food_upkeep_mult = 0.8` |
| `ui_trait_extremely_light_eaters` | 8 | biological | `pop_food_upkeep_mult = -0.4` |
| `ui_trait_extremely_long_lived` | 8 | biological | `leader_lifespan_mult = 0.8` |
| `ui_trait_extremely_nonadaptive` | -8 | biological | `pop_environment_tolerance = -0.4` |
| `ui_trait_extremely_rapid_breeders` | 8 | biological | `pop_growth_speed_mult = 0.4` |
| `ui_trait_extremely_short_lived` | -8 | biological | `leader_lifespan_mult = -0.4` |
| `ui_trait_extremely_sluggish` | -8 | physical | `army_damage_mult = -0.24`, `ship_evasion_mult = -0.12` |
| `ui_trait_extremely_strong` | 8 | physical | `army_damage_mult = 0.4`, `planet_jobs_minerals_produces_mult = 0.2` |
| `ui_trait_extremely_uncreative` | -8 | social | `research_speed_mult = -0.24` |
| `ui_trait_extremely_undemanding` | 8 | social | `pop_consumer_goods_upkeep_mult = -0.4` |
| `ui_trait_extremely_weak` | -8 | physical | `army_damage_mult = -0.4`, `planet_jobs_minerals_produces_mult = -0.2` |
| `ui_trait_gluttonous` | -2 | biological | `pop_food_upkeep_mult = 0.2` |

### Negative traits (19)

| Trait ID | Cost | Category | Key modifiers |
|----------|------|----------|---------------|
| `ui_trait_craven` | -2 | military | `army_damage_mult = -0.15`, `army_morale_mult = -0.1` |
| `ui_trait_deceitful` | -2 | social | `trade_value_mult = -0.1` |
| `ui_trait_dumb` | -2 | social | `research_speed_mult = -0.1` |
| `ui_trait_lazy` | -2 | physical | `pop_jobs_produces_mult = -0.05` |
| `ui_trait_light_eaters` | 2 | biological | `pop_food_upkeep_mult = -0.1` |
| `ui_trait_long_lived` | 2 | biological | `leader_lifespan_mult = 0.2` |
| `ui_trait_momentary` | -2 | biological | `leader_lifespan_mult = -0.1` |
| `ui_trait_nonresilient` | -2 | biological | `pop_environment_tolerance = -0.1` |
| `ui_trait_optimistic` | 2 | social | `monthly_unity_mult = 0.05` |
| `ui_trait_pessimistic` | -2 | social | `monthly_unity_mult = -0.05` |
| `ui_trait_poor_engineers` | -2 | industry | `research_speed_engineering_mult = -0.1` |
| `ui_trait_poor_physicists` | -2 | industry | `research_speed_physics_mult = -0.1` |
| `ui_trait_poor_sociologists` | -2 | industry | `research_speed_society_mult = -0.1` |
| `ui_trait_short_lived` | -2 | biological | `leader_lifespan_mult = -0.1` |
| `ui_trait_sluggish` | -2 | physical | `army_damage_mult = -0.1`, `ship_evasion_mult = -0.05` |
| `ui_trait_trusting` | 2 | social | `trade_value_mult = 0.1` |
| `ui_trait_uncreative` | -2 | social | `research_speed_mult = -0.1` |
| `ui_trait_undemanding` | 2 | social | `pop_consumer_goods_upkeep_mult = -0.1` |
| `ui_trait_uneconomical` | -2 | industry | `pop_jobs_energy_produces_mult = -0.1` |
| `ui_trait_urban` | 2 | social | `pop_housing_usage_mult = -0.1` |

## ui-traits_species_newtiers.txt — 20 vanilla-extended tier traits

### Very tier (10 positive, 10 negative)

| Trait ID | Base trait | Cost | Category | Key modifiers |
|----------|------------|------|----------|---------------|
| `ui_trait_very_strong` | `trait_strong` | 6 | physical | `army_damage_mult = 0.3`, `planet_jobs_minerals_produces_mult = 0.15` |
| `ui_trait_very_weak` | `trait_weak` | -6 | physical | `army_damage_mult = -0.3`, `planet_jobs_minerals_produces_mult = -0.15` |
| `ui_trait_very_agile` | `trait_agile` | 6 | physical | `army_damage_mult = 0.18`, `ship_evasion_mult = 0.09` |
| `ui_trait_very_clumsy` | `trait_clumsy` | -6 | physical | `army_damage_mult = -0.18`, `ship_evasion_mult = -0.09` |
| `ui_trait_very_intelligent` | `trait_intelligent` | 6 | social | `research_speed_mult = 0.18` |
| `ui_trait_very_slow_learners` | `trait_slow_learners` | -6 | social | `research_speed_mult = -0.18` |
| `ui_trait_very_rapid_breeders` | `trait_rapid_breeders` | 6 | biological | `pop_growth_speed_mult = 0.3` |
| `ui_trait_very_slow_breeders` | `trait_slow_breeders` | -6 | biological | `pop_growth_speed_mult = -0.3` |
| `ui_trait_very_adaptive` | `trait_adaptive` | 6 | biological | `pop_environment_tolerance = 0.3` |
| `ui_trait_very_nonadaptive` | `trait_nonadaptive` | -6 | biological | `pop_environment_tolerance = -0.3` |
| `ui_trait_very_long_lived` | `trait_long_lived` | 6 | biological | `leader_lifespan_mult = 0.6` |
| `ui_trait_very_short_lived` | `trait_short_lived` | -6 | biological | `leader_lifespan_mult = -0.3` |
| `ui_trait_very_thrifty` | `trait_thrifty` | 6 | industry | `pop_jobs_energy_produces_mult = 0.15` |
| `ui_trait_very_wasteful` | `trait_wasteful` | -6 | industry | `pop_jobs_energy_produces_mult = -0.15` |
| `ui_trait_very_charismatic` | `trait_charismatic` | 6 | social | `trade_value_mult = 0.15`, `monthly_unity_mult = 0.1` |
| `ui_trait_very_repugnant` | `trait_repugnant` | -6 | social | `trade_value_mult = -0.15`, `monthly_unity_mult = -0.1` |
| `ui_trait_very_natural_engineers` | `trait_natural_engineers` | 6 | industry | `research_speed_engineering_mult = 0.18` |
| `ui_trait_very_poor_engineers` | `trait_poor_engineers` | -6 | industry | `research_speed_engineering_mult = -0.18` |
| `ui_trait_very_natural_physicists` | `trait_natural_physicists` | 6 | industry | `research_speed_physics_mult = 0.18` |
| `ui_trait_very_poor_physicists` | `trait_poor_physicists` | -6 | industry | `research_speed_physics_mult = -0.18` |
| `ui_trait_very_natural_sociologists` | `trait_natural_sociologists` | 6 | industry | `research_speed_society_mult = 0.18` |
| `ui_trait_very_poor_sociologists` | `trait_poor_sociologists` | -6 | industry | `research_speed_society_mult = -0.18` |

### Extremely tier (10 positive, 10 negative)

| Trait ID | Very tier | Cost | Category | Key modifiers |
|----------|-----------|------|----------|---------------|
| `ui_trait_extremely_strong` | `ui_trait_very_strong` | 8 | physical | `army_damage_mult = 0.4`, `planet_jobs_minerals_produces_mult = 0.2` |
| `ui_trait_extremely_weak` | `ui_trait_very_weak` | -8 | physical | `army_damage_mult = -0.4`, `planet_jobs_minerals_produces_mult = -0.2` |
| `ui_trait_extremely_agile` | `ui_trait_very_agile` | 8 | physical | `army_damage_mult = 0.24`, `ship_evasion_mult = 0.12` |
| `ui_trait_extremely_clumsy` | `ui_trait_very_clumsy` | -8 | physical | `army_damage_mult = -0.24`, `ship_evasion_mult = -0.12` |
| `ui_trait_extremely_intelligent` | `ui_trait_very_intelligent` | 8 | social | `research_speed_mult = 0.24` |
| `ui_trait_extremely_slow_learners` | `ui_trait_very_slow_learners` | -8 | social | `research_speed_mult = -0.24` |
| `ui_trait_extremely_rapid_breeders` | `ui_trait_very_rapid_breeders` | 8 | biological | `pop_growth_speed_mult = 0.4` |
| `ui_trait_extremely_slow_breeders` | `ui_trait_very_slow_breeders` | -8 | biological | `pop_growth_speed_mult = -0.4` |
| `ui_trait_extremely_adaptive` | `ui_trait_very_adaptive` | 8 | biological | `pop_environment_tolerance = 0.4` |
| `ui_trait_extremely_nonadaptive` | `ui_trait_very_nonadaptive` | -8 | biological | `pop_environment_tolerance = -0.4` |
| `ui_trait_extremely_long_lived` | `ui_trait_very_long_lived` | 8 | biological | `leader_lifespan_mult = 0.8` |
| `ui_trait_extremely_short_lived` | `ui_trait_very_short_lived` | -8 | biological | `leader_lifespan_mult = -0.4` |
| `ui_trait_extremely_thrifty` | `ui_trait_very_thrifty` | 8 | industry | `pop_jobs_energy_produces_mult = 0.2` |
| `ui_trait_extremely_wasteful` | `ui_trait_very_wasteful` | -8 | industry | `pop_jobs_energy_produces_mult = -0.2` |
| `ui_trait_extremely_charismatic` | `ui_trait_very_charismatic` | 8 | social | `trade_value_mult = 0.2`, `monthly_unity_mult = 0.15` |
| `ui_trait_extremely_repugnant` | `ui_trait_very_repugnant` | -8 | social | `trade_value_mult = -0.2`, `monthly_unity_mult = -0.15` |
| `ui_trait_extremely_natural_engineers` | `ui_trait_very_natural_engineers` | 8 | industry | `research_speed_engineering_mult = 0.24` |
| `ui_trait_extremely_poor_engineers` | `ui_trait_very_poor_engineers` | -8 | industry | `research_speed_engineering_mult = -0.24` |
| `ui_trait_extremely_natural_physicists` | `ui_trait_very_natural_physicists` | 8 | industry | `research_speed_physics_mult = 0.24` |
| `ui_trait_extremely_poor_physicists` | `ui_trait_very_poor_physicists` | -8 | industry | `research_speed_physics_mult = -0.24` |
| `ui_trait_extremely_natural_sociologists` | `ui_trait_very_natural_sociologists` | 8 | industry | `research_speed_society_mult = 0.24` |
| `ui_trait_extremely_poor_sociologists` | `ui_trait_very_poor_sociologists` | -8 | industry | `research_speed_society_mult = -0.24` |

## Opposites mapping

Each trait declares `opposites` listing mutually exclusive traits. Key patterns:

- Custom traits oppose their direct counterparts (e.g., `ui_trait_agile` opposes `ui_trait_sluggish`, `ui_trait_very_sluggish`, `ui_trait_extremely_sluggish`)
- Vanilla-extended tiers oppose the vanilla base trait and all other tiers in the family
- Cross-family opposites exist where logically appropriate (e.g., `ui_trait_brave` opposes `ui_trait_craven` and vanilla `trait_craven`)

## Allowed archetypes

| Trait category | Allowed archetypes |
|----------------|-------------------|
| Physical (combat, agility, strength) | `BIOLOGICAL`, `LITHOID` |
| Social (creativity, charisma, trust) | `BIOLOGICAL`, `LITHOID` |
| Biological (breeding, lifespan, food, habitability) | `BIOLOGICAL`, `LITHOID` |
| Industry (research, energy, minerals) | `BIOLOGICAL`, `LITHOID`, `MACHINE` |
| Military | `BIOLOGICAL`, `LITHOID` |
| Leader | `BIOLOGICAL`, `LITHOID` |

Machine and Robot archetypes have limited trait access per archetype configuration.

## Tags

| Tag | Meaning |
|-----|---------|
| `positive` | Beneficial trait (costs trait points) |
| `negative` | Detrimental trait (grants trait points) |
| `custom` | Original mod trait (not vanilla-extended) |
| `vanilla_extended` | Very/Extremely tier extending a vanilla trait family |

## AI weights

AI weights range from ~0.5 (low priority) to ~3.0 (high priority). Higher weights for:
- Strong positive modifiers (e.g., `ui_trait_extremely_strong` = 3.0)
- Traits with broad economic impact (e.g., `ui_trait_very_thrifty` = 2.5)
Lower weights for:
- Niche or situational traits
- Negative traits (AI avoids them unless forced)

## Maintenance notes

- **Adding a custom trait:** Add block to `ui-traits_species.txt`, add localisation keys, add icon `.dds`, test in-game.
- **Adding a vanilla tier:** Add block to `ui-traits_species_newtiers.txt` following scaling policy, add localisation keys, add icon `.dds`, verify opposites include all family members.
- **Modifying a trait:** Update modifier values, cost, ai_weight; update localisation if description changes; verify scaling policy compliance for tiers.
- **Removing a trait:** Remove from trait file, remove localisation keys, remove icon file (optional), verify no other traits reference it in `opposites`.

## Related documentation

| Document | Scope |
|----------|-------|
| [Data model](./data-model.md) | Schema, scaling policy, archetype variables |
| [Architecture](./architecture.md) | Trait definition area, interfaces |
| [Components/localisation.md](./localisation.md) | Localisation keys and formatting |
| [Components/assets.md](./assets.md) | Icon naming and pipeline |
| [Trait Catalogue (Wiki)](https://github.com/hmlendea/stellaris-ui-species-traits/wiki/Traits) | Detailed trait list with effects and costs |