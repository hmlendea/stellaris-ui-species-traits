# Data model

This document defines the trait definition schema, scaling policy, archetype variables, localisation structure, and icon naming convention for the Universum Infinitum - Species Traits mod.

## Trait definition schema

All traits are defined in Clausewitz script (`.txt`) files under `ui-species-traits/common/traits/`. Each trait is a top-level block with the following fields:

```clausewitz
ui_trait_<id> = {
    cost = <int>                    # Trait points cost (positive or negative)
    category = "<string>"           # "physical" | "social" | "biological" | "industry" | "military" | "leader"
    opposites = { <trait_id> ... }  # Mutually exclusive trait IDs (vanilla and custom)
    allowed_archetypes = { <arch> } # "BIOLOGICAL" | "LITHOID" | "MACHINE" | "ROBOT" | "HOVERING" | "PSIONIC"
    tags = { <tag> ... }            # Optional: "positive" | "negative" | "custom" | "vanilla_extended"
    modifier = {                    # Static modifiers applied to species/pops/planets
        <modifier_key> = <value>
        ...
    }
    slave_cost = <int>              # Cost when enslaved (usually same as cost)
    ai_weight = <float>             # AI selection weight
}
```

### Field reference

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `cost` | integer | Yes | Trait points cost. Positive = costs points; negative = grants points. Range typically -4 to +4 for base traits, higher for tiers. |
| `category` | string | Yes | UI grouping in species creation. Values: `physical`, `social`, `biological`, `industry`, `military`, `leader`. |
| `opposites` | list of trait IDs | Yes | Traits that cannot be selected together. Must include vanilla trait IDs where applicable. |
| `allowed_archetypes` | list of archetypes | Yes | Which species archetypes can select this trait. Common: `BIOLOGICAL`, `LITHOID`, `MACHINE`, `ROBOT`. |
| `tags` | list of strings | No | Metadata tags. Used: `positive`, `negative`, `custom`, `vanilla_extended`. |
| `modifier` | block | Yes | Stellaris static modifiers. Keys are engine-defined (e.g., `planet_jobs_food_produces_mult`, `pop_environment_tolerance`, `army_damage_mult`). |
| `slave_cost` | integer | Yes | Trait cost when species is enslaved. Usually mirrors `cost`. |
| `ai_weight` | float | Yes | AI selection preference. Higher = more likely to pick. |

### Example: Custom trait

```clausewitz
ui_trait_agile = {
    cost = 2
    category = "physical"
    opposites = { trait_clumsy ui_trait_sluggish ui_trait_very_sluggish ui_trait_extremely_sluggish }
    allowed_archtypes = { BIOLOGICAL LITHOID }
    tags = { positive custom }
    modifier = {
        army_damage_mult = 0.1
        ship_evasion_mult = 0.05
    }
    slave_cost = 2
    ai_weight = 1.5
}
```

### Example: Vanilla-extended tier trait

```clausewitz
ui_trait_very_strong = {
    cost = 6
    category = "physical"
    opposites = { trait_weak trait_very_weak ui_trait_weak ui_trait_very_weak ui_trait_extremely_weak }
    allowed_archetypes = { BIOLOGICAL LITHOID }
    tags = { positive vanilla_extended }
    modifier = {
        army_damage_mult = 0.3
        planet_jobs_minerals_produces_mult = 0.15
    }
    slave_cost = 6
    ai_weight = 2.0
}
```

## Scaling policy

The mod extends vanilla trait families with **Very** and **Extremely** tiers. The scaling policy is documented in file headers and applied manually; it is not enforced by tooling.

### Very tier (x11/6 effects, x3 cost)

| Base trait | Very tier ID | Cost multiplier | Effect multiplier |
|------------|--------------|-----------------|-------------------|
| `trait_strong` (cost 2) | `ui_trait_very_strong` | ×3 → 6 | ×11/6 ≈ 1.83× |
| `trait_agile` (cost 2) | `ui_trait_very_agile` | ×3 → 6 | ×11/6 ≈ 1.83× |
| `trait_intelligent` (cost 2) | `ui_trait_very_intelligent` | ×3 → 6 | ×11/6 ≈ 1.83× |
| `trait_rapid_breeders` (cost 2) | `ui_trait_very_rapid_breeders` | ×3 → 6 | ×11/6 ≈ 1.83× |
| `trait_adaptive` (cost 2) | `ui_trait_very_adaptive` | ×3 → 6 | ×11/6 ≈ 1.83× |
| `trait_long_lived` (cost 2) | `ui_trait_very_long_lived` | ×3 → 6 | ×11/6 ≈ 1.83× |
| `trait_thrifty` (cost 2) | `ui_trait_very_thrifty` | ×3 → 6 | ×11/6 ≈ 1.83× |
| `trait_charismatic` (cost 2) | `ui_trait_very_charismatic` | ×3 → 6 | ×11/6 ≈ 1.83× |
| `trait_natural_engineers` (cost 2) | `ui_trait_very_natural_engineers` | ×3 → 6 | ×11/6 ≈ 1.83× |
| `trait_natural_physicists` (cost 2) | `ui_trait_very_natural_physicists` | ×3 → 6 | ×11/6 ≈ 1.83× |
| `trait_natural_sociologists` (cost 2) | `ui_trait_very_natural_sociologists` | ×3 → 6 | ×11/6 ≈ 1.83× |

Negative traits follow the same pattern (e.g., `trait_weak` → `ui_trait_very_weak`).

### Extremely tier (+20% effects over Very, cost ×4 base)

| Very tier | Extremely tier ID | Cost multiplier (vs base) | Effect multiplier (vs Very) |
|-----------|-------------------|---------------------------|----------------------------|
| `ui_trait_very_strong` | `ui_trait_extremely_strong` | ×4 → 8 | +20% |
| `ui_trait_very_agile` | `ui_trait_extremely_agile` | ×4 → 8 | +20% |
| `ui_trait_very_intelligent` | `ui_trait_extremely_intelligent` | ×4 → 8 | +20% |
| `ui_trait_very_rapid_breeders` | `ui_trait_extremely_rapid_breeders` | ×4 → 8 | +20% |
| `ui_trait_very_adaptive` | `ui_trait_extremely_adaptive` | ×4 → 8 | +20% |
| `ui_trait_very_long_lived` | `ui_trait_extremely_long_lived` | ×4 → 8 | +20% |
| `ui_trait_very_thrifty` | `ui_trait_extremely_thrifty` | ×4 → 8 | +20% |
| `ui_trait_very_charismatic` | `ui_trait_extremely_charismatic` | ×4 → 8 | +20% |
| `ui_trait_very_natural_engineers` | `ui_trait_extremely_natural_engineers` | ×4 → 8 | +20% |
| `ui_trait_very_natural_physicists` | `ui_trait_extremely_natural_physicists` | ×4 → 8 | +20% |
| `ui_trait_very_natural_sociologists` | `ui_trait_extremely_natural_sociologists` | ×4 → 8 | +20% |

Negative traits follow the same pattern.

### Policy notes

- The x11/6 multiplier for Very tiers approximates 1.833× the base effect.
- The +20% for Extremely tiers is applied on top of the Very tier effect (not the base).
- Cost scaling: Very = 3× base cost; Extremely = 4× base cost.
- This policy applies to 11 vanilla trait families (22 traits including negatives), producing 20 tier extensions (Very + Extremely for each, minus some duplicates).
- **Not validated by CI or tooling.** Maintainers must manually verify scaling when adding or modifying tiers.

## Archetype variables

Archetype trait limits are defined in `ui-species-traits/common/species_archetypes/00_species_archetypes.txt` using variable references for maintainability.

### Variable definitions

```clausewitz
@species_trait_points = 2
@species_max_traits = 7
@robot_trait_points = 0
@robot_max_traits = 6
@machine_trait_points = 1
@machine_max_traits = 7
```

### Archetype blocks

| Archetype | Trait points | Max traits | Inherits from |
|-----------|--------------|------------|---------------|
| `BIOLOGICAL` | `@species_trait_points` (2) | `@species_max_traits` (7) | — |
| `LITHOID` | `@species_trait_points` (2) | `@species_max_traits` (7) | `BIOLOGICAL` via `inherit_trait_points_from = BIOLOGICAL` |
| `MACHINE` | `@machine_trait_points` (1) | `@machine_max_traits` (7) | — |
| `ROBOT` | `@robot_trait_points` (0) | `@robot_max_traits` (6) | — |
| `HOVERING` | `@machine_trait_points` (1) | `@machine_max_traits` (7) | `MACHINE` |
| `PSIONIC` | `@species_trait_points` (2) | `@species_max_traits` (7) | `BIOLOGICAL` |
| `PRESAPIENT` | N/A | N/A | `uses_modifiers = no` |
| `OTHER` | N/A | N/A | `uses_modifiers = no` |

### Modification procedure

To adjust archetype limits:
1. Update the variable values at the top of `00_species_archetypes.txt`.
2. All archetypes referencing those variables update automatically.
3. Test in-game species creation to verify limits apply correctly.

## Localisation structure

Localisation files are YAML (`.yml`) under `ui-species-traits/localisation/english/` with UTF-8 BOM encoding.

### File organisation

| File | Scope |
|------|-------|
| `ui-species-traits_l_english.yml` | 38 custom traits |
| `ui-species-traits_newtiers_l_english.yml` | 20 vanilla-extended tier traits |

### Key format

```yaml
l_english:
  ui_trait_<id>:0 "<Display Name>"
  ui_trait_<id>_desc:0 "§L<Description>§!"
```

### Formatting conventions

- **Name key:** `ui_trait_<id>:0` — plain text, no formatting.
- **Description key:** `ui_trait_<id>_desc:0` — wrapped in `§L...§!` for in-game styling (larger font, line breaks).
- **Modifier references:** Use `§G+X%§!` for positive, `§R-X%§!` for negative in descriptions.
- **Line breaks:** Use `\n` within the `§L...§!` block.

### Example

```yaml
l_english:
  ui_trait_agile:0 "Agile"
  ui_trait_agile_desc:0 "§LThis species possesses exceptional reflexes and coordination.\n§G+10%§! Army Damage\n§G+5%§! Ship Evasion§!"
```

### Completeness requirement

Every trait ID in `common/traits/*.txt` must have both `:0` and `_desc:0` keys in the corresponding localisation file. Missing keys result in raw IDs displayed in-game.

## Icon naming convention

Trait icons are DDS files in `ui-species-traits/gfx/interface/icons/traits/`. The engine loads them by filename convention, not by explicit `icon` field in trait definitions.

### Convention

| Trait ID pattern | Icon filename | Engine reference |
|------------------|---------------|------------------|
| `ui_trait_<name>` | `ui_trait_<name>.dds` | `GFX_trait_ui_trait_<name>` |
| `trait_<name>` (vanilla) | `trait_<name>.dds` | `GFX_trait_trait_<name>` |

### Examples

| Trait ID | Icon file | Engine reference |
|----------|-----------|------------------|
| `ui_trait_agile` | `ui_trait_agile.dds` | `GFX_trait_ui_trait_agile` |
| `ui_trait_very_strong` | `ui_trait_very_strong.dds` | `GFX_trait_ui_trait_very_strong` |
| `ui_trait_extremely_agile` | `ui_trait_extremely_agile.dds` | `GFX_trait_ui_trait_extremely_agile` |
| `trait_very_strong` (vanilla) | `trait_very_strong.dds` | `GFX_trait_trait_very_strong` |

### Requirements

- Format: DDS (DirectDraw Surface), BC3/DXT5 compression recommended.
- Resolution: 64×64 or 128×128 pixels (Stellaris standard).
- No subdirectories; all icons directly in `traits/`.
- Source assets: `src/trait.xcf` (GIMP layered source for icon creation).

### Completeness requirement

Every trait ID must have a matching `.dds` file. Missing icons show a placeholder in-game.

## Trait catalogue summary

| Category | Count | Source file |
|----------|-------|-------------|
| Custom positive traits | 19 | `ui-traits_species.txt` |
| Custom negative traits | 19 | `ui-traits_species.txt` |
| Very tier extensions (positive) | 10 | `ui-traits_species_newtiers.txt` |
| Very tier extensions (negative) | 10 | `ui-traits_species_newtiers.txt` |
| Extremely tier extensions (positive) | 10 | `ui-traits_species_newtiers.txt` |
| Extremely tier extensions (negative) | 10 | `ui-traits_species_newtiers.txt` |
| **Total** | **58** | — |

### Custom trait categories (from `ui-traits_species.txt`)

| Category | Traits |
|----------|--------|
| Combat | `ui_trait_brave`, `ui_trait_craven`, `ui_trait_agile`, `ui_trait_sluggish`, `ui_trait_very_agile`, `ui_trait_very_sluggish`, `ui_trait_extremely_agile`, `ui_trait_extremely_sluggish` |
| Population | `ui_trait_rapid_breeders`, `ui_trait_slow_breeders`, `ui_trait_very_rapid_breeders`, `ui_trait_very_slow_breeders`, `ui_trait_extremely_rapid_breeders`, `ui_trait_extremely_slow_breeders` |
| Science | `ui_trait_creative`, `ui_trait_uncreative`, `ui_trait_very_creative`, `ui_trait_very_uncreative`, `ui_trait_extremely_creative`, `ui_trait_extremely_uncreative` |
| Economy | `ui_trait_demanding`, `ui_trait_undemanding`, `ui_trait_very_demanding`, `ui_trait_very_undemanding`, `ui_trait_extremely_demanding`, `ui_trait_extremely_undemanding`, `ui_trait_gluttonous`, `ui_trait_light_eaters`, `ui_trait_very_gluttonous`, `ui_trait_very_light_eaters`, `ui_trait_extremely_gluttonous`, `ui_trait_extremely_light_eaters` |
| Habitability | `ui_trait_adaptive`, `ui_trait_nonadaptive`, `ui_trait_very_adaptive`, `ui_trait_very_nonadaptive`, `ui_trait_extremely_adaptive`, `ui_trait_extremely_nonadaptive` |
| Lifespan | `ui_trait_long_lived`, `ui_trait_short_lived`, `ui_trait_very_long_lived`, `ui_trait_very_short_lived`, `ui_trait_extremely_long_lived`, `ui_trait_extremely_short_lived` |
| Social | `ui_trait_trusting`, `ui_trait_deceitful`, `ui_trait_optimistic`, `ui_trait_pessimistic`, `ui_trait_docile`, `ui_trait_very_docile` |
| Special | `ui_trait_urban`, `ui_trait_uneconomical`, `ui_trait_poor_engineers`, `ui_trait_poor_physicists`, `ui_trait_poor_sociologists`, `ui_trait_lazy`, `ui_trait_momentary`, `ui_trait_nonresilient` |

### Vanilla-extended families (from `ui-traits_species_newtiers.txt`)

| Family | Base trait | Very tier | Extremely tier |
|--------|------------|-----------|----------------|
| Strength | `trait_strong` / `trait_weak` | `ui_trait_very_strong` / `ui_trait_very_weak` | `ui_trait_extremely_strong` / `ui_trait_extremely_weak` |
| Agility | `trait_agile` / `trait_clumsy` | `ui_trait_very_agile` / `ui_trait_very_clumsy` | `ui_trait_extremely_agile` / `ui_trait_extremely_clumsy` |
| Intelligence | `trait_intelligent` / `trait_slow_learners` | `ui_trait_very_intelligent` / `ui_trait_very_slow_learners` | `ui_trait_extremely_intelligent` / `ui_trait_extremely_slow_learners` |
| Breeding | `trait_rapid_breeders` / `trait_slow_breeders` | `ui_trait_very_rapid_breeders` / `ui_trait_very_slow_breeders` | `ui_trait_extremely_rapid_breeders` / `ui_trait_extremely_slow_breeders` |
| Adaptability | `trait_adaptive` / `trait_nonadaptive` | `ui_trait_very_adaptive` / `ui_trait_very_nonadaptive` | `ui_trait_extremely_adaptive` / `ui_trait_extremely_nonadaptive` |
| Lifespan | `trait_long_lived` / `trait_short_lived` | `ui_trait_very_long_lived` / `ui_trait_very_short_lived` | `ui_trait_extremely_long_lived` / `ui_trait_extremely_short_lived` |
| Thrift | `trait_thrifty` / `trait_wasteful` | `ui_trait_very_thrifty` / `ui_trait_very_wasteful` | `ui_trait_extremely_thrifty` / `ui_trait_extremely_wasteful` |
| Charisma | `trait_charismatic` / `trait_repugnant` | `ui_trait_very_charismatic` / `ui_trait_very_repugnant` | `ui_trait_extremely_charismatic` / `ui_trait_extremely_repugnant` |
| Engineering | `trait_natural_engineers` / `trait_poor_engineers` | `ui_trait_very_natural_engineers` / `ui_trait_very_poor_engineers` | `ui_trait_extremely_natural_engineers` / `ui_trait_extremely_poor_engineers` |
| Physics | `trait_natural_physicists` / `trait_poor_physicists` | `ui_trait_very_natural_physicists` / `ui_trait_very_poor_physicists` | `ui_trait_extremely_natural_physicists` / `ui_trait_extremely_poor_physicists` |
| Sociology | `trait_natural_sociologists` / `trait_poor_sociologists` | `ui_trait_very_natural_sociologists` / `ui_trait_very_poor_sociologists` | `ui_trait_extremely_natural_sociologists` / `ui_trait_extremely_poor_sociologists` |

## Related documentation

| Document | Scope |
|----------|-------|
| [Architecture](./architecture.md) | Architectural areas, interfaces, contracts |
| [Repository structure](./repository-structure.md) | Source tree layout |
| [Components/trait-definitions.md](./components/trait-definitions.md) | Per-trait deep dive (planned) |
| [Components/archetype-config.md](./components/archetype-config.md) | Archetype configuration detail (planned) |
| [Components/localisation.md](./components/localisation.md) | Localisation structure detail (planned) |
| [Components/assets.md](./components/assets.md) | Icon pipeline detail (planned) |
| Root [ARCHITECTURE.md](../ARCHITECTURE.md) | High-level architectural boundaries |
| [Trait Catalogue (Wiki)](https://github.com/hmlendea/stellaris-ui-species-traits/wiki/Traits) | Detailed trait list with effects and costs |