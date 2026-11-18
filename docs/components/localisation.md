# Component: Localisation

This document details the localisation file structure, key format, formatting conventions, and completeness requirements.

## Files

| File | Trait scope | Keys |
|------|-------------|------|
| `ui-species-traits/localisation/english/ui-species-traits_l_english.yml` | 38 trait definitions | 76 (38 names, 38 descriptions) |
| `ui-species-traits/localisation/english/ui-species-traits_newtiers_l_english.yml` | 20 additional tiers | 40 (20 names, 20 descriptions) |

## File structure

Both files use YAML with a single root key:

```yaml
l_english:

 ui_trait_<identifier>_desc:0 "§L<Description>§!\n"
 ui_trait_<identifier>:0 "<Display name>"
```

Each entry is indented by one space. Descriptions precede names in the current files.

### Root key

- `l_english:` is the language identifier; it must be exactly `l_english` for English localisation.

### Key format

| Key type | Pattern | Example |
|----------|---------|---------|
| Trait name | `ui_trait_<identifier>:0` | ` ui_trait_agile:0 "Agile"` |
| Trait description | `ui_trait_<identifier>_desc:0` | ` ui_trait_agile_desc:0 "§L...§!\n"` |

The `:0` suffix is the version marker the engine localisation system requires.

## Formatting conventions

### Names

- Plain text without formatting markers.
- Sentence case, with hyphens preserved where the source uses them, such as `Long-lived` and `Short-lived`.
- No trailing punctuation.

### Descriptions

- One sentence wrapped in `§L...§!` and terminated by `\n`.
- No numeric effect values and no `§G`, `§R`, or `§Y` colour codes are present in either file; the descriptions convey flavour only.
- Modifier magnitudes consequently live solely in the trait definitions, and a description does not need revision when a value changes.

Example:

```yaml
 ui_trait_agile_desc:0 "§LThis species is gifted with rapid reflexes, resulting in prompt reactions in battle.§!\n"
 ui_trait_agile:0 "Agile"
```

## Encoding

| File | Byte-order mark |
|------|-----------------|
| `ui-species-traits_l_english.yml` | absent; the file begins directly with `l_english:` |
| `ui-species-traits_newtiers_l_english.yml` | present (`EF BB BF`) |

Both files are UTF-8. Preserve each file's existing state when editing; do not add or strip a byte-order mark as a side effect of an unrelated change.

## Completeness requirements

Every trait identifier must have both keys in the corresponding file:

1. `ui_trait_<identifier>:0` for the name.
2. `ui_trait_<identifier>_desc:0` for the description.

The trait file holds 38 names and 38 descriptions; the tier file holds 20 names and 20 descriptions. These counts match the 58 trait definitions exactly.

Missing keys cause the engine to display the raw identifier, for example `ui_trait_agile` instead of `Agile`.

Verification checklist:
- [ ] Both files contain a name and a description for every identifier.
- [ ] No duplicate keys within a file.
- [ ] No keys without a matching trait definition.
- [ ] The existing byte-order-mark state of each file is unchanged.

## Adding a language

To add localisation for another language:

1. Create `ui-species-traits/localisation/<locale>/`.
2. Create `ui-species-traits_l_<locale>.yml` and `ui-species-traits_newtiers_l_<locale>.yml`.
3. Change the root key to `l_<locale>`.
4. Translate every value while keeping every key unchanged.
5. Match the byte-order-mark state of an equivalent vanilla localisation file.
6. Verify in game that the engine selects the new locale.

## Maintenance procedures

### Adding localisation for a new trait

1. Choose the file: the trait file for a definition, the tier file for an additional tier.
2. Add the description key, then the name key, inside the `l_english:` block:
   ```yaml
    ui_trait_new_trait_desc:0 "§LDescription text.§!\n"
    ui_trait_new_trait:0 "New Trait"
   ```
3. Preserve the file's byte-order-mark state.
4. Verify the trait in game.

### Updating existing localisation

1. Locate the key in the appropriate file.
2. Change the value while keeping the `:0` suffix on the key.
3. Verify in game.

### Removing localisation for a deleted trait

1. Delete both keys from the appropriate file.
2. Remove the trait block and its icon as described in [components/trait-definitions.md](./trait-definitions.md).

## Terminology

Descriptions refer to game concepts in their in-game form, such as habitability, empire size, pop growth, and unity. Reuse the terms already present in the files rather than introducing synonyms, so that a reader can match a description to the corresponding interface element.

## Related documentation

| Document | Scope |
|----------|-------|
| [Data model](../data-model.md) | Localisation structure, key format |
| [Architecture](../architecture.md) | Localisation area, interfaces |
| [Components/trait-definitions.md](./trait-definitions.md) | Identifiers requiring localisation |
| [Components/assets.md](./assets.md) | Icon naming convention |