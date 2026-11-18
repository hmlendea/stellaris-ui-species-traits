# Data model

This document defines the trait definition schema, the tier scaling policy, the archetype variables, the localisation structure, and the icon naming convention for the Universum Infinitum - Species Traits mod.

## Trait definition schema

All traits are defined in Clausewitz script files under `ui-species-traits/common/traits/`. The two files do not use an identical field set.

### Fields in `ui-traits_species.txt`

Every one of the 38 entries declares exactly four fields:

```clausewitz
ui_trait_<identifier> = {
    cost = <integer>
    opposites = { "<identifier>" ... }
    allowed_archetypes = { <ARCHETYPE> ... }
    modifier = {
        <modifier_key> = <value>
        ...
    }
}
```

### Fields in `ui-traits_species_newtiers.txt`

Every one of the 20 entries declares the four fields above plus `category`, `tags`, and `slave_cost`, and most add eligibility blocks:

```clausewitz
ui_trait_<identifier> = {
    cost = <integer>
    category = normal
    species_potential_add = { <condition> }          # 11 entries
    species_possible_add = { <condition> }           # 9 entries
    species_possible_remove = { <condition> }        # 9 entries
    species_possible_merge_remove = { <condition> }  # 9 entries
    opposites = { "<identifier>" ... }
    allowed_archetypes = { <ARCHETYPE> ... }
    tags = { organic <positive|negative> <group> ... }
    modifier = {
        <modifier_key> = <value>
    }
    slave_cost = {
        trade = <integer>
    }
    assembly_score = { ... }                         # 2 entries
    ai_weight = { weight = <number> }                # 10 entries
}
```

### Field reference

| Field | Introduced in | Required | Description |
|-------|---------------|----------|-------------|
| `cost` | Both files | Yes | Trait point cost. Positive values consume points; negative values grant them. Observed range: `-4` to `8`. |
| `opposites` | Both files | Yes | Mutually exclusive identifiers, quoted. Custom and vanilla identifiers are mixed freely. |
| `allowed_archetypes` | Both files | Yes | Either `{ BIOLOGICAL }` or `{ BIOLOGICAL LITHOID }`. No entry declares `MACHINE` or `ROBOT`. |
| `modifier` | Both files | Yes | Static modifiers applied to pops, planets, armies, or the country. Keys are engine-defined. |
| `category` | New tiers only | Yes | Always `normal` in the current content. |
| `tags` | New tiers only | Yes | Starts with `organic`, then `positive` or `negative`, then group tags such as `habitability`, `pop_growth`, `leader`, `army`, `empire_size`, `unity`, or `pop_output`. |
| `slave_cost` | New tiers only (one entry in the trait file) | Yes where present | A block containing `trade = <integer>`, giving the trade value exchanged when the trait is applied through the slave market. |
| Eligibility blocks | New tiers only | No | `species_potential_add`, `species_possible_add`, `species_possible_remove`, and `species_possible_merge_remove` gate genetic modification. |
| `assembly_score` | New tiers only | No | Two entries (`ui_trait_extremely_strong`, `ui_trait_extremely_weak` region) add robot assembly weight conditioned on planet designation. |
| `ai_weight` | New tiers only | No | A block containing `weight = <number>`; ten entries declare it, including `weight = 0` on negative tiers. |

### Absent field

No trait definition declares an `icon` field. The engine resolves icons from the file name, so renaming an icon file changes or breaks the displayed icon with no script error. See [Icon naming convention](#icon-naming-convention).

### Example: trait definition

```clausewitz
# Name: Brave
ui_trait_brave = {
    cost = 2
    opposites = { "ui_trait_craven" }

    allowed_archetypes = { BIOLOGICAL LITHOID }

    modifier = {
        army_morale = 0.15
        army_damage_mult = 0.15
    }
}
```

### Example: additional tier

```clausewitz
# Name: Very Adaptive
# Group: Adaptability
ui_trait_very_adaptive = {
    cost = 3
    category = normal
    species_possible_add = {
        can_add_genetic_traits = yes
    }
    species_possible_remove = {
        can_remove_beneficial_genetic_traits = yes
    }
    species_possible_merge_remove = {
        always = yes
    }
    opposites = { "trait_nonadaptive" "trait_adaptive" "trait_extremely_adaptive" "trait_robust" "trait_adaptive_lithoid" "trait_shelled" "trait_permeable_skin" "trait_terraphobic" "trait_pathogenic_genes" "ui_trait_very_nonadaptive" "ui_trait_extremely_nonadaptive" }
    allowed_archetypes = { BIOLOGICAL }
    tags = { organic positive habitability }
    modifier = {
        pop_environment_tolerance = 0.166667
    }
    slave_cost = {
        trade = 750
    }
}
```

## Tier scaling policy

The policy is recorded as comments at the top of both trait files. It describes intent; nothing validates it.

| Aspect | Very tier | Extremely tier |
|--------|-----------|----------------|
| Effect target | 11/6 of the base effect (approximately 1.833) | 20 per cent above the corresponding Very tier |
| Positive cost | Three times the base cost | Four times the base cost (a 33.333333 per cent increase over Very) |
| Negative cost | Follows the effect factor, because vanilla inverse pairs retain unit cost elasticity | Same rule, with refunds truncated toward zero and raised by at least one point per stronger tier |
| Vanilla precedence | — | Where vanilla already supplies an Extremely tier, its fixed values take precedence and the custom Very tier is back-calculated |

The policy intentionally excludes slave prices, assembly scores, AI weights, trigger thresholds, and crossbreeding chances from scaling. Discrete count modifiers are rounded away from zero to the nearest valid integer.

### Observed values

The recorded values do not all match the 11/6 target, because the entries align with the vanilla values they extend:

| Family | Base cost | Very cost | Extremely cost | Very effect | Extremely effect |
|--------|-----------|-----------|----------------|-------------|------------------|
| Strength (`army_damage_mult`) | `trait_strong`, 2 | `ui_trait_very_weak` -2, `ui_trait_extremely_strong` 4 | — | about 1.83× (`0.366667` for the negative tier) | `0.48` for the positive tier |
| Agility (`weapon_type_*_fire_rate_mult`) | `ui_trait_agile`, 2 at `0.05` | `ui_trait_very_agile`, 6 at `0.091667` | `ui_trait_extremely_agile`, 8 at `0.11` | 1.833× | 2.2× base |
| Adaptability (`pop_environment_tolerance`) | vanilla base, 2 | `ui_trait_very_adaptive`, 3 at `0.166667` | `ui_trait_extremely_nonadaptive`, -4 at `-0.22` | 1.667× | 2.2× base |
| Breeding (`logistic_growth_mult`) | vanilla base, 2 | `ui_trait_very_rapid_breeders`, 6 at `0.183333` | `ui_trait_extremely_rapid_breeders`, 8 at `0.22` | 1.833× | 2.2× base |
| Lifespan (`leader_lifespan_add`, years) | vanilla base | `ui_trait_long_lived`, 3 at `36.666667` | `ui_trait_very_long_lived`, 4 at `44` | additive years, not a multiplier | additive years |
| Unity (`planet_jobs_unity_produces_mult`) | `ui_trait_creative`, 2 at `0.10` | `ui_trait_very_creative`, 6 at `0.183333` | `ui_trait_extremely_creative`, 8 at `0.22` | 1.833× | 2.2× base |

Lifespan and talent tiers use additive rather than multiplicative modifiers, so the 11/6 ratio is inapplicable to them; the header states that the ratio is a design reference rather than a literal multiplier.

## Archetype variables

Archetype limits are defined in `ui-species-traits/common/species_archetypes/00_species_archetypes.txt` through shared variables.

### Variable definitions

```clausewitz
@robot_trait_points = 0
@robot_max_traits = 6
@machine_trait_points = 1
@machine_max_traits = 7
@species_trait_points = 2
@species_max_traits = 7
```

### Archetype blocks

| Archetype | Trait points | Maximum traits | Inheritance or flags |
|-----------|--------------|----------------|----------------------|
| `BIOLOGICAL` | `@species_trait_points` (2) | `@species_max_traits` (7) | — |
| `ROBOT` | `@robot_trait_points` (0) | `@robot_max_traits` (6) | — |
| `MACHINE` | `@machine_trait_points` (1) | `@machine_max_traits` (7) | `robotic = yes` |
| `PRESAPIENT` | `@species_trait_points` (2) | `@species_max_traits` (7) | `uses_modifiers = no` |
| `LITHOID` | inherited | inherited | `inherit_trait_points_from = BIOLOGICAL` |
| `OTHER` | not declared | not declared | `uses_modifiers = no` |

The file declares no `HOVERING` or `PSIONIC` archetype block; those names appear nowhere in the repository.

### Modification procedure

1. Update the variable values at the top of `00_species_archetypes.txt`.
2. Every archetype that references a variable updates with it.
3. Verify the effective limits in species creation for each archetype.

## Localisation structure

Localisation files are YAML (`.yml`) under `ui-species-traits/localisation/english/`.

### File organisation

| File | Scope | Keys |
|------|-------|------|
| `ui-species-traits_l_english.yml` | 38 trait definitions | 76 keys (38 names, 38 descriptions) |
| `ui-species-traits_newtiers_l_english.yml` | 20 additional tiers | 40 keys (20 names, 20 descriptions) |

### Key format

```yaml
l_english:
 ui_trait_<identifier>:0 "<Display name>"
 ui_trait_<identifier>_desc:0 "§L<Description>§!\n"
```

Each key is indented by a single space and carries the `:0` version suffix the engine requires.

### Formatting conventions

- **Name key:** plain text without formatting markers, for example ` ui_trait_agile:0 "Agile"`.
- **Description key:** a single sentence wrapped in `§L...§!` and terminated by `\n`, for example `"§LThis species is gifted with rapid reflexes, resulting in prompt reactions in battle.§!\n"`.
- **Effect values:** no key currently embeds numeric effect values or colour codes, so descriptions do not restate modifier magnitudes.

### Encoding

The additional-tiers file begins with a UTF-8 byte-order mark (`EF BB BF`); the trait-definitions file begins directly with `l_english:`. Both are read as UTF-8. When editing, preserve each file's existing byte-order-mark state rather than normalising it.

### Completeness requirement

Every trait identifier must have both keys in the corresponding file: 38 + 38 in the trait file and 20 + 20 in the tier file. The current counts match the number of trait definitions exactly.

## Icon naming convention

Trait icons are DDS files in `ui-species-traits/gfx/interface/icons/traits/`. The directory holds 60 files: one per `ui_trait_*` identifier (58) plus `trait_venerable.dds` and `trait_very_strong.dds`.

### Convention

| Identifier pattern | Icon filename | Engine reference |
|--------------------|---------------|------------------|
| `ui_trait_<name>` | `ui_trait_<name>.dds` | `GFX_trait_ui_trait_<name>` |
| `trait_<name>` (vanilla) | `trait_<name>.dds` | `GFX_trait_trait_<name>` |

### Examples

| Identifier | Icon file | Engine reference |
|------------|-----------|------------------|
| `ui_trait_agile` | `ui_trait_agile.dds` | `GFX_trait_ui_trait_agile` |
| `ui_trait_very_adaptive` | `ui_trait_very_adaptive.dds` | `GFX_trait_ui_trait_very_adaptive` |
| `ui_trait_extremely_strong` | `ui_trait_extremely_strong.dds` | `GFX_trait_ui_trait_extremely_strong` |
| `trait_venerable` (not defined by this mod) | `trait_venerable.dds` | `GFX_trait_trait_venerable` |

### Requirements

- Format: DDS (DirectDraw Surface).
- No subdirectories; all icons reside directly in `traits/`.
- Filenames must match identifiers exactly, because the engine resolves icons by name.
- Source artwork: `src/trait.xcf`.

## Trait catalogue

### Trait definitions in `ui-traits_species.txt`

| Identifier | Cost | Allowed archetypes | Modifiers |
|------------|------|--------------------|-----------|
| `ui_trait_urban` | -2 | `BIOLOGICAL` | `planet_jobs_food_produces_mult = -0.15` |
| `ui_trait_uneconomical` | -2 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_energy_produces_mult = -0.15` |
| `ui_trait_lazy` | -2 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_minerals_produces_mult = -0.15` |
| `ui_trait_dumb` | -2 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_engineering_research_produces_mult = -0.1`, `planet_jobs_physics_research_produces_mult = -0.1`, `planet_jobs_society_research_produces_mult = -0.1` |
| `ui_trait_poor_engineers` | -1 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_engineering_research_produces_mult = -0.15` |
| `ui_trait_poor_physicists` | -1 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_physics_research_produces_mult = -0.15` |
| `ui_trait_poor_sociologists` | -1 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_society_research_produces_mult = -0.15` |
| `ui_trait_nonresilient` | -1 | `BIOLOGICAL`, `LITHOID` | `army_defense_damage_mult = -0.5` |
| `ui_trait_brave` | 2 | `BIOLOGICAL`, `LITHOID` | `army_morale = 0.15`, `army_damage_mult = 0.15` |
| `ui_trait_craven` | -2 | `BIOLOGICAL`, `LITHOID` | `army_morale = -0.15`, `army_damage_mult = -0.15` |
| `ui_trait_deceitful` | 2 | `BIOLOGICAL`, `LITHOID` | `country_influence_produces_mult = 0.1` |
| `ui_trait_trusting` | -2 | `BIOLOGICAL`, `LITHOID` | `country_influence_produces_mult = -0.1` |
| `ui_trait_optimistic` | 1 | `BIOLOGICAL`, `LITHOID` | `pop_happiness = 0.05` |
| `ui_trait_pessimistic` | -1 | `BIOLOGICAL`, `LITHOID` | `pop_happiness = -0.05` |
| `ui_trait_agile` | 2 | `BIOLOGICAL`, `LITHOID` | `weapon_type_kinetic_weapon_fire_rate_mult = 0.05`, `weapon_type_energy_weapon_fire_rate_mult = 0.05`, `weapon_type_explosive_weapon_fire_rate_mult = 0.05` |
| `ui_trait_very_agile` | 6 | `BIOLOGICAL`, `LITHOID` | same three modifiers at `0.091667` |
| `ui_trait_extremely_agile` | 8 | `BIOLOGICAL`, `LITHOID` | same three modifiers at `0.11` |
| `ui_trait_sluggish` | -2 | `BIOLOGICAL`, `LITHOID` | same three modifiers at `-0.05` |
| `ui_trait_very_sluggish` | -3 | `BIOLOGICAL`, `LITHOID` | same three modifiers at `-0.091667` |
| `ui_trait_extremely_sluggish` | -4 | `BIOLOGICAL`, `LITHOID` | same three modifiers at `-0.11` |
| `ui_trait_light_eaters` | 1 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = -0.10` |
| `ui_trait_very_light_eaters` | 3 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = -0.183333` |
| `ui_trait_extremely_light_eaters` | 4 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = -0.22` |
| `ui_trait_gluttonous` | -1 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = 0.10` |
| `ui_trait_very_gluttonous` | -2 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = 0.183333` |
| `ui_trait_extremely_gluttonous` | -3 | `BIOLOGICAL` | `planet_pops_organics_food_upkeep_mult = 0.22` |
| `ui_trait_creative` | 2 | `BIOLOGICAL`, `LITHOID` | `planet_jobs_unity_produces_mult = 0.10`, `planet_amenities_mult = 0.10` |
| `ui_trait_very_creative` | 6 | `BIOLOGICAL`, `LITHOID` | both modifiers at `0.183333` |
| `ui_trait_extremely_creative` | 8 | `BIOLOGICAL`, `LITHOID` | both modifiers at `0.22` |
| `ui_trait_uncreative` | -2 | `BIOLOGICAL`, `LITHOID` | both modifiers at `-0.10` |
| `ui_trait_very_uncreative` | -3 | `BIOLOGICAL`, `LITHOID` | both modifiers at `-0.183333` |
| `ui_trait_extremely_uncreative` | -4 | `BIOLOGICAL`, `LITHOID` | both modifiers at `-0.22` |
| `ui_trait_demanding` | -1 | `BIOLOGICAL`, `LITHOID` | `pop_amenities_usage_mult = 0.10`, `planet_pops_consumer_goods_upkeep_mult = 0.01`, `pop_housing_usage_mult = 0.01` |
| `ui_trait_very_demanding` | -2 | `BIOLOGICAL`, `LITHOID` | same three at `0.183333`, `0.018333`, `0.018333` |
| `ui_trait_extremely_demanding` | -3 | `BIOLOGICAL`, `LITHOID` | same three at `0.22`, `0.022`, `0.022` |
| `ui_trait_undemanding` | 1 | `BIOLOGICAL`, `LITHOID` | same three at `-0.10`, `-0.01`, `-0.01` |
| `ui_trait_very_undemanding` | 3 | `BIOLOGICAL`, `LITHOID` | same three at `-0.183333`, `-0.018333`, `-0.018333` |
| `ui_trait_extremely_undemanding` | 4 | `BIOLOGICAL`, `LITHOID` | same three at `-0.22`, `-0.022`, `-0.022` |

### Additional tiers in `ui-traits_species_newtiers.txt`

| Group | Identifier | Cost | Modifiers | Tags |
|-------|------------|------|-----------|------|
| Adaptability | `ui_trait_very_adaptive` | 3 | `pop_environment_tolerance = 0.166667` | `organic positive habitability` |
| Adaptability | `ui_trait_very_nonadaptive` | -3 | `pop_environment_tolerance = -0.183333` | `organic negative habitability` |
| Adaptability | `ui_trait_extremely_nonadaptive` | -4 | `pop_environment_tolerance = -0.22` | `organic negative habitability` |
| Breeding | `ui_trait_very_rapid_breeders` | 6 | `logistic_growth_mult = 0.183333` | `organic positive pop_growth` |
| Breeding | `ui_trait_extremely_rapid_breeders` | 8 | `logistic_growth_mult = 0.22` | `organic positive pop_growth` |
| Breeding | `ui_trait_very_slow_breeders` | -3 | `logistic_growth_mult = -0.183333` | `organic negative pop_growth` |
| Breeding | `ui_trait_extremely_slow_breeders` | -4 | `logistic_growth_mult = -0.22` | `organic negative pop_growth` |
| Civic Behaviour | `ui_trait_very_docile` | 6 | `species_empire_size_mult = -0.183333` | `organic positive empire_size` |
| Civic Behaviour | `ui_trait_very_unruly` | -3 | `species_empire_size_mult = 0.183333` | `organic negative empire_size` |
| Lifespan | `ui_trait_long_lived` | 3 | `leader_lifespan_add = 36.666667` | `organic positive leader` |
| Lifespan | `ui_trait_momentary` | -4 | `leader_lifespan_add = -26.4` | `organic negative leader` |
| Lifespan | `ui_trait_short_lived` | -2 | `leader_lifespan_add = -18.333333` | `organic negative leader` |
| Lifespan | `ui_trait_very_long_lived` | 4 | `leader_lifespan_add = 44` | `organic positive leader` |
| Lifespan | `ui_trait_very_short_lived` | -3 | `leader_lifespan_add = -22` | `organic negative leader` |
| Social Cohesion | `ui_trait_very_quarrelsome` | -2 | `bureaucrat_jobs_bonus_workforce_mult = -0.183333` | `organic negative pop_output unity` |
| Social Cohesion | `ui_trait_very_traditional` | 3 | `bureaucrat_jobs_bonus_workforce_mult = 0.183333` | `organic positive pop_output unity` |
| Strength | `ui_trait_very_weak` | -2 | `army_damage_mult = -0.366667`, `worker_and_simple_drone_cat_bonus_workforce_mult = -0.045833`, `soldier_jobs_bonus_workforce_mult = -0.183333` | `organic negative pop_output army` |
| Strength | `ui_trait_extremely_weak` | -3 | `army_damage_mult = -0.44`, `worker_and_simple_drone_cat_bonus_workforce_mult = -0.055`, `soldier_jobs_bonus_workforce_mult = -0.22` | `organic negative pop_output army` |
| Strength | `ui_trait_extremely_strong` | 4 | `army_damage_mult = 0.48`, `worker_and_simple_drone_cat_bonus_workforce_mult = 0.06`, `soldier_jobs_bonus_workforce_mult = 0.18` | `organic positive pop_output army` |
| Talent | `ui_trait_very_talented` | 3 | `negative_leader_traits_species = -2`, `leaders_upkeep_mult = -0.183333` | `organic positive leader` |

### Distribution across the catalogue

| Measure | Count |
|---------|-------|
| Total definitions | 58 |
| Definitions in `ui-traits_species.txt` | 38 |
| Definitions in `ui-traits_species_newtiers.txt` | 20 |
| Positive cost | 24 |
| Negative cost | 34 |
| Limited to `BIOLOGICAL` | 17 |
| Available to `BIOLOGICAL` and `LITHOID` | 41 |
| Identifiers without the `very` or `extremely` prefix | 25 |

## Related documentation

| Document | Scope |
|----------|-------|
| [Architecture](./architecture.md) | Architectural areas, interfaces, contracts |
| [Repository structure](./repository-structure.md) | Source tree layout |
| [Components/trait-definitions.md](./components/trait-definitions.md) | Trait definition detail |
| [Components/archetype-config.md](./components/archetype-config.md) | Archetype configuration detail |
| [Components/localisation.md](./components/localisation.md) | Localisation structure detail |
| [Components/assets.md](./components/assets.md) | Icon convention and artwork workflow |
| Root [ARCHITECTURE.md](../ARCHITECTURE.md) | Architectural boundaries |