# Configuration

This document describes the mod descriptor configuration, supported version declarations, and configuration variables.

## Descriptor files

| File | Location | Purpose |
|------|----------|---------|
| `ui-species-traits.mod` | Repository root | Mod descriptor for launcher |
| `ui-species-traits/descriptor.mod` | Mod folder root | Mod descriptor for engine |

### Root descriptor (`ui-species-traits.mod`)

```clausewitz
name = "Universum Infinitum - Species Traits"
path = "mod/ui-species-traits"
tags = { "Species" "Traits" "Gameplay" }
supported_version = "v4.5.*"
```

### Mod folder descriptor (`ui-species-traits/descriptor.mod`)

```clausewitz
name = "Universum Infinitum - Species Traits"
path = "common/species_archetypes"
tags = { "Species" "Traits" "Gameplay" }
supported_version = "v4.5.*"
```

**Note:** The mod folder descriptor's `path` points to a subdirectory of the mod package (typically `common/`), not the mod root. This is standard Stellaris convention.

## Field reference

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Display name shown in launcher |
| `path` | string | Yes | Relative path to mod content directory |
| `tags` | list of strings | Yes | Categorisation tags for launcher filtering |
| `supported_version` | string | Yes | Stellaris version branch this mod supports |

## Supported version declaration

### Current declaration

```clausewitz
supported_version = "v4.5.*"
```

This declares compatibility with Stellaris 4.5.x. The `*` wildcard allows patch updates within 4.5.x.

### Version mismatch handling

| Scenario | Launcher behaviour |
|----------|-------------------|
| Game version ≥ 4.5.x | Mod enabled |
| Game version < 4.5.x | Mod shows as incompatible |
| Game version > 4.6.x | Mod disabled with warning |

### Version update procedure

When Stellaris releases a new major version:

1. Update `supported_version` in both descriptors to new version (e.g., `v4.6.*`)
2. Verify trait definitions still valid in new engine version
3. Test in-game for any breaking changes
4. Update README if version badge changes
5. Tag and release

## Tags

| Tag | Meaning |
|-----|---------|
| `Species` | Categorises as species-related content |
| `Traits` | Categorises as trait definitions |
| `Gameplay` | Categorises as gameplay modifiers |

Tags are used by the launcher for filtering. Multiple tags allowed.

## Configuration variables

The mod uses Clausewitz script variables in `00_species_archetypes.txt` for archetype configuration. These are **not** mod-level configuration — they are archetype definitions consumed by the engine.

### Variable reference

```clausewitz
@species_trait_points = 2
@species_max_traits = 7
@robot_trait_points = 0
@robot_max_traits = 6
@machine_trait_points = 1
@machine_max_traits = 7
```

### Variable modification procedure

1. Open `ui-species-traits/common/species_archetypes/00_species_archetypes.txt`
2. Modify variable values at top
3. All archetypes referencing those variables update automatically
4. Test in-game species creation

## Localisation configuration

### File encoding

| Property | Requirement |
|----------|-------------|
| Encoding | UTF-8 with BOM |
| Language key | `l_english:` |

Stellaris requires BOM for YAML localisation files. Without it, the file is ignored and raw IDs displayed.

### Adding languages

To add a language:

1. Create `localisation/<locale>/` directory
2. Create `<mod>_l_<locale>.yml` file
3. Use root key `l_<locale>:` (lowercase locale code)
4. Translate all keys
5. Save as UTF-8 with BOM

## Icon configuration

### Naming convention

Icons loaded by filename convention:

| Trait ID | Icon file | Engine reference |
|----------|-----------|------------------|
| `ui_trait_<name>` | `ui_trait_<name>.dds` | `GFX_trait_ui_trait_<name>` |
| `trait_<name>` | `trait_<name>.dds` | `GFX_trait_trait_<name>` |

### No explicit icon field

Trait definitions do **not** contain an `icon` field. Icons are referenced by the engine via the `GFX_trait_<id>` convention based on filename. This differs from some vanilla traits that use explicit `icon = "GFX_trait_..."` references.

## Descriptor validation checklist

Before releasing:

- [ ] Both descriptors have identical `name`, `tags`, `supported_version`
- [ ] `supported_version` matches current Stellaris version branch
- [ ] `path` in mod folder descriptor points to existing subdirectory
- [ ] No trailing whitespace or formatting issues in descriptor
- [ ] `supported_version` format is correct (`v<major>.<minor>*`)

## Related documentation

| Document | Scope |
|----------|-------|
| [Build and deployment](./build-and-deployment.md) | Release workflows, packaging |
| [Dependencies](./dependencies.md) | Stellaris version coupling |
| [Architecture](../architecture.md) | Descriptor area |
| [Components/archetype-config.md](../components/archetype-config.md) | Archetype variable definitions |