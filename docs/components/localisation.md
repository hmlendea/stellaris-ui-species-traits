# Component: Localisation

This document details the localisation file structure, key format, formatting conventions, and completeness requirements.

## Files

| File | Trait scope | Line count (approx) |
|------|-------------|---------------------|
| `ui-species-traits/localisation/english/ui-species-traits_l_english.yml` | 38 custom traits | ~150 |
| `ui-species-traits/localisation/english/ui-species-traits_newtiers_l_english.yml` | 20 vanilla-extended tier traits | ~80 |

## File structure

Both files follow the same YAML format with UTF-8 BOM encoding (required by Stellaris):

```yaml
l_english:
  ui_trait_<id>:0 "<Display Name>"
  ui_trait_<id>_desc:0 "§L<Description>§!"
  ...
```

### Root key

- `l_english:` — Language identifier. Must be exactly `l_english` for English localisation.

### Key naming convention

| Key type | Pattern | Example |
|----------|---------|---------|
| Trait name | `ui_trait_<id>:0` | `ui_trait_agile:0` |
| Trait description | `ui_trait_<id>_desc:0` | `ui_trait_agile_desc:0` |

The `:0` suffix is required by Stellaris' localisation system (version marker).

## Formatting conventions

### Trait names

- Plain text, no formatting codes
- Capitalised (first letter of each word)
- No trailing punctuation
- Maximum ~30 characters for UI fit

```yaml
ui_trait_agile:0 "Agile"
ui_trait_very_strong:0 "Very Strong"
ui_trait_extremely_rapid_breeders:0 "Extremely Rapid Breeders"
```

### Trait descriptions

- Wrapped in `§L...§!` for in-game styling (larger font, word wrap)
- Use `\n` for explicit line breaks within the `§L...§!` block
- Modifier values shown with colour codes:
  - `§G+X%§!` — Positive bonus (green)
  - `§R-X%§!` — Negative penalty (red)
  - `§YX%§!` — Neutral/informational (yellow)
- Modifier names use in-game terminology (e.g., "Army Damage", "Ship Evasion", "Research Speed")
- End with `§!` to close the formatting block

```yaml
ui_trait_agile_desc:0 "§LThis species possesses exceptional reflexes and coordination.\n§G+10%§! Army Damage\n§G+5%§! Ship Evasion§!"
```

### Description template

Standard pattern for positive traits:

```yaml
ui_trait_<id>_desc:0 "§L<Flavour text>.\n§G+X%§! <Modifier 1>\n§G+Y%§! <Modifier 2>§!"
```

Standard pattern for negative traits:

```yaml
ui_trait_<id>_desc:0 "§L<Flavour text>.\n§R-X%§! <Modifier 1>\n§R-Y%§! <Modifier 2>§!"
```

Mixed (rare):

```yaml
ui_trait_<id>_desc:0 "§L<Flavour text>.\n§G+X%§! <Positive>\n§R-Y%§! <Negative>§!"
```

## Completeness requirements

### Mandatory keys

Every trait ID defined in `common/traits/*.txt` must have **both** keys in the corresponding localisation file:

1. `ui_trait_<id>:0` — Display name
2. `ui_trait_<id>_desc:0` — Description

### Verification

Missing keys result in raw key names displayed in-game (e.g., `ui_trait_agile` instead of "Agile").

Manual verification checklist:
- [ ] All 38 custom traits have keys in `ui-species-traits_l_english.yml`
- [ ] All 20 tier traits have keys in `ui-species-traits_newtiers_l_english.yml`
- [ ] No duplicate keys within a file
- [ ] No orphaned keys (keys without matching trait definition)

## Encoding requirement

**UTF-8 with BOM (Byte Order Mark)** is mandatory for Stellaris to recognise the file.

- VS Code: Save with "UTF-8 with BOM" encoding
- Command line: `iconv -f UTF-8 -t UTF-8-BOM input.yml > output.yml`
- Git: Ensure `.gitattributes` does not strip BOM

## Adding a new language

To add localisation for another language (e.g., French):

1. Create directory: `ui-species-traits/localisation/french/`
2. Create files:
   - `ui-species-traits_l_french.yml`
   - `ui-species-traits_newtiers_l_french.yml`
3. Use root key `l_french:` (lowercase language code)
4. Translate all name and description keys
5. Save as UTF-8 with BOM
6. Test in-game with language set to French

Supported language codes: `english`, `french`, `german`, `spanish`, `russian`, `polish`, `portuguese_brazil`, `chinese_simplified`, `chinese_traditional`, `japanese`, `korean`, `italian`, `turkish`, `czech`, `polish`.

## Maintenance procedures

### Adding localisation for a new trait

1. Determine which file: `ui-species-traits_l_english.yml` (custom) or `ui-species-traits_newtiers_l_english.yml` (vanilla tier)
2. Add two keys at end of `l_english:` block:
   ```yaml
   ui_trait_new_trait:0 "New Trait"
   ui_trait_new_trait_desc:0 "§LDescription text.\n§G+X%§! Modifier§!"
   ```
3. Save as UTF-8 with BOM
4. Test in-game

### Updating existing localisation

1. Locate the key in the appropriate file
2. Modify the value (keep `:0` suffix on key)
3. Save as UTF-8 with BOM
4. Test in-game

### Removing localisation for a deleted trait

1. Delete both keys (`:0` and `_desc:0`) from the appropriate file
2. Save as UTF-8 with BOM

## Common modifier display names

| Modifier key | Display name |
|--------------|--------------|
| `army_damage_mult` | Army Damage |
| `ship_evasion_mult` | Ship Evasion |
| `research_speed_mult` | Research Speed |
| `research_speed_engineering_mult` | Engineering Research Speed |
| `research_speed_physics_mult` | Physics Research Speed |
| `research_speed_society_mult` | Society Research Speed |
| `pop_growth_speed_mult` | Population Growth Speed |
| `pop_environment_tolerance` | Habitability |
| `leader_lifespan_mult` | Leader Lifespan |
| `pop_food_upkeep_mult` | Food Upkeep |
| `pop_consumer_goods_upkeep_mult` | Consumer Goods Upkeep |
| `pop_jobs_energy_produces_mult` | Energy Production |
| `pop_jobs_minerals_produces_mult` | Minerals Production |
| `pop_jobs_food_produces_mult` | Food Production |
| `trade_value_mult` | Trade Value |
| `monthly_unity_mult` | Monthly Unity |
| `army_morale_mult` | Army Morale |
| `pop_housing_usage_mult` | Housing Usage |
| `pop_jobs_produces_mult` | Job Output |

## Related documentation

| Document | Scope |
|----------|-------|
| [Data model](./data-model.md) | Localisation structure, key format |
| [Architecture](./architecture.md) | Localisation area, interfaces |
| [Components/trait-definitions.md](./trait-definitions.md) | Trait IDs requiring localisation |
| [Components/assets.md](./assets.md) | Icon naming (parallel convention) |
| Root [ARCHITECTURE.md](../ARCHITECTURE.md) | High-level localisation area |