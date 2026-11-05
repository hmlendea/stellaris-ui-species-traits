# Behaviour: Trait effects

This document describes how trait modifiers are applied in-game, their scope, stacking rules, and interaction with game systems.

## Modifier application model

All trait effects are **static modifiers** defined in the `modifier` block of each trait. The Stellaris engine applies these continuously while the species exists in-game.

```clausewitz
modifier = {
    army_damage_mult = 0.1
    ship_evasion_mult = 0.05
}
```

### Application scope

| Scope | Target | Example modifiers |
|-------|--------|-------------------|
| **Species** | All pops of this species | `leader_lifespan_mult`, `pop_growth_speed_mult` |
| **Pop** | Individual pops | `pop_food_upkeep_mult`, `pop_jobs_produces_mult` |
| **Planet** | Planet where pops reside | `planet_jobs_minerals_produces_mult`, `planet_jobs_food_produces_mult` |
| **Army** | Armies recruited from species | `army_damage_mult`, `army_morale_mult` |
| **Ship** | Ships crewed by species | `ship_evasion_mult`, `ship_fire_rate_mult` |
| **Country** | Empire-wide | `research_speed_mult`, `trade_value_mult`, `monthly_unity_mult` |

### Application timing

- **Game start:** Modifiers applied when species is initialised
- **Pop growth:** New pops inherit species modifiers immediately
- **Gene modification:** Modifiers updated on species template change
- **Save/load:** Modifiers reapplied from current trait definitions

## Modifier categories

### Combat modifiers

| Modifier | Scope | Effect |
|----------|-------|--------|
| `army_damage_mult` | Army | Multiplies army damage output |
| `army_morale_mult` | Army | Multiplies army morale (health) |
| `ship_evasion_mult` | Ship | Adds to ship evasion chance |
| `ship_fire_rate_mult` | Ship | Multiplies weapon fire rate |
| `ship_weapon_damage_mult` | Ship | Multiplies weapon damage |

### Economic modifiers

| Modifier | Scope | Effect |
|----------|-------|--------|
| `pop_jobs_produces_mult` | Pop | Multiplies all job output |
| `pop_jobs_energy_produces_mult` | Pop/Planet | Multiplies energy job output |
| `pop_jobs_minerals_produces_mult` | Pop/Planet | Multiplies minerals job output |
| `pop_jobs_food_produces_mult` | Pop/Planet | Multiplies food job output |
| `pop_jobs_consumer_goods_produces_mult` | Pop/Planet | Multiplies consumer goods output |
| `pop_jobs_unity_produces_mult` | Pop/Planet | Multiplies unity job output |
| `pop_jobs_research_produces_mult` | Pop/Planet | Multiplies research job output |
| `trade_value_mult` | Country | Multiplies trade value from pops |
| `monthly_unity_mult` | Country | Multiplies monthly unity income |

### Research modifiers

| Modifier | Scope | Effect |
|----------|-------|--------|
| `research_speed_mult` | Country | Multiplies all research speed |
| `research_speed_engineering_mult` | Country | Multiplies engineering research |
| `research_speed_physics_mult` | Country | Multiplies physics research |
| `research_speed_society_mult` | Country | Multiplies society research |

### Population modifiers

| Modifier | Scope | Effect |
|----------|-------|--------|
| `pop_growth_speed_mult` | Species/Planet | Multiplies pop growth speed |
| `pop_food_upkeep_mult` | Pop | Multiplies food consumption |
| `pop_consumer_goods_upkeep_mult` | Pop | Multiplies consumer goods consumption |
| `pop_housing_usage_mult` | Pop | Multiplies housing usage |
| `pop_amenities_usage_mult` | Pop | Multiplies amenities usage |
| `pop_environment_tolerance` | Pop | Adds to habitability (0–100%) |

### Leader modifiers

| Modifier | Scope | Effect |
|----------|-------|--------|
| `leader_lifespan_mult` | Species | Multiplies leader lifespan |
| `leader_experience_gain_mult` | Country | Multiplies leader XP gain |
| `leader_skill_levels` | Country | Adds to leader level cap |

## Stacking rules

### Same modifier, same source

- **Not possible:** A species cannot have the same trait twice
- Traits are mutually exclusive via `opposites`

### Same modifier, different sources

| Source combination | Stacking behaviour |
|--------------------|-------------------|
| Trait + Trait (different) | **Additive** — values sum |
| Trait + Tradition | **Additive** |
| Trait + Ascension perk | **Additive** |
| Trait + Technology | **Additive** |
| Trait + Event modifier | **Additive** |
| Trait + Planet modifier | **Additive** |

### Example: Army damage stacking

```
Species traits:
- trait_strong: army_damage_mult = 0.2
- ui_trait_brave: army_damage_mult = 0.15

Tradition: Supremacy → army_damage_mult = 0.2
Technology: Gene Warriors → army_damage_mult = 0.1

Total army_damage_mult = 0.2 + 0.15 + 0.2 + 0.1 = 0.65 (65% bonus)
```

### Multiplicative vs additive

- **Additive:** Most `*_mult` modifiers (sum of percentages)
- **Multiplicative:** Some base values (e.g., `pop_growth_speed` base × (1 + sum))

The engine handles this internally; trait definitions use additive values.

## Trait-specific behaviour details

### Positive traits (cost > 0)

Provide bonuses to one or more modifiers. Examples:

| Trait | Key modifiers | Gameplay impact |
|-------|---------------|-----------------|
| `ui_trait_very_strong` | `army_damage_mult = 0.3`, `planet_jobs_minerals_produces_mult = 0.15` | Stronger armies, more minerals |
| `ui_trait_very_intelligent` | `research_speed_mult = 0.18` | Faster research |
| `ui_trait_very_rapid_breeders` | `pop_growth_speed_mult = 0.3` | Faster population growth |
| `ui_trait_very_adaptive` | `pop_environment_tolerance = 0.3` | +30% habitability on all planets |

### Negative traits (cost < 0)

Provide penalties but grant trait points. Examples:

| Trait | Key modifiers | Gameplay impact |
|-------|---------------|-----------------|
| `ui_trait_very_weak` | `army_damage_mult = -0.3`, `planet_jobs_minerals_produces_mult = -0.15` | Weaker armies, fewer minerals |
| `ui_trait_very_slow_learners` | `research_speed_mult = -0.18` | Slower research |
| `ui_trait_very_slow_breeders` | `pop_growth_speed_mult = -0.3` | Slower population growth |
| `ui_trait_very_nonadaptive` | `pop_environment_tolerance = -0.3` | -30% habitability |

### Special traits

| Trait | Unique behaviour |
|-------|------------------|
| `ui_trait_urban` | `pop_housing_usage_mult = -0.1` — reduces housing needs |
| `ui_trait_uneconomical` | `pop_jobs_energy_produces_mult = -0.1` — reduces energy output |
| `ui_trait_lazy` | `pop_jobs_produces_mult = -0.05` — reduces all job output |
| `ui_trait_momentary` | `leader_lifespan_mult = -0.1` — shorter leader lives |
| `ui_trait_nonresilient` | `pop_environment_tolerance = -0.1` — lower habitability |

## Interaction with game systems

### Species rights

| Right | Trait effect interaction |
|-------|-------------------------|
| Full Citizenship | All trait modifiers apply |
| Residence | All trait modifiers apply |
| Slaves | `slave_cost` used instead of `cost`; modifiers still apply |
| Purge | Modifiers apply until pop removed |
| Assimilation | Species traits replaced; modifiers change |

### Robot/Drone species

- Machine archetype traits only affect robot pops
- `ui_trait_very_natural_engineers` etc. apply to machine pop job output
- No biological modifiers (food, consumer goods) apply

### Hybrid species (gene clinics)

- Primary species traits determine modifiers
- Sub-species can have different trait selections
- Each sub-species gets its own modifier set

### Crisis/Event modifiers

- Temporary modifiers from events stack additively with trait modifiers
- Example: "Genetic Resequencing" event + `ui_trait_very_strong` = combined bonus

## Performance considerations

- Static modifiers have **zero runtime cost** after initial application
- Engine caches modifier totals per species/planet/army
- No per-tick computation for trait modifiers
- Large trait counts (58) have no performance impact vs vanilla

## Verification in-game

### Console commands

```bash
# View species modifiers
> species_modifiers <species_id>

# View pop modifiers
> pop_modifiers <pop_id>

# View planet modifiers
> planet_modifiers <planet_id>

# View country modifiers
> country_modifiers
```

### Debug tooltip

Hover over resource output in planet view → shows modifier breakdown including trait contributions.

## Related documentation

| Document | Scope |
|----------|-------|
| [Components/trait-definitions.md](../components/trait-definitions.md) | Trait modifier values |
| [Data model](../data-model.md) | Scaling policy for tier modifiers |
| [Flows/trait-selection.md](../flows/trait-selection.md) | Selection constraints |
| [Architecture](../architecture.md) | Data architecture |