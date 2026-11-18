# Component: Assets (icons)

This document details the trait icon convention, the directory contents, the format requirements, and the artwork workflow.

## Location and contents

**Directory:** `ui-species-traits/gfx/interface/icons/traits/`
**Files:** 60 `.dds` icons

| Group | Count |
|-------|-------|
| Icons named after a `ui_trait_*` identifier | 58 |
| Icons with a `trait_*` name | 2 (`trait_very_strong.dds`, `trait_venerable.dds`) |

Every `ui_trait_*` identifier defined in `common/traits/` has a matching icon file, and no `ui_trait_*` icon lacks a definition. The two `trait_*` files do not correspond to any definition in this repository, so they are inert unless a vanilla definition resolves them.

## Naming convention

The engine resolves icons by file name; no trait definition declares an `icon` field.

| Identifier pattern | Icon file | Engine reference |
|--------------------|-----------|------------------|
| `ui_trait_<name>` | `ui_trait_<name>.dds` | `GFX_trait_ui_trait_<name>` |
| `trait_<name>` | `trait_<name>.dds` | `GFX_trait_trait_<name>` |

### Examples

| Identifier | Icon file | Engine reference |
|------------|-----------|------------------|
| `ui_trait_agile` | `ui_trait_agile.dds` | `GFX_trait_ui_trait_agile` |
| `ui_trait_very_adaptive` | `ui_trait_very_adaptive.dds` | `GFX_trait_ui_trait_very_adaptive` |
| `ui_trait_extremely_strong` | `ui_trait_extremely_strong.dds` | `GFX_trait_ui_trait_extremely_strong` |
| `trait_venerable` | `trait_venerable.dds` | `GFX_trait_trait_venerable` |

Because resolution is filename-based, renaming an icon silently changes or removes the displayed icon while leaving the trait definition valid.

## Directory pattern

```
gfx/
└── interface/
    └── icons/
        └── traits/
            ├── trait_venerable.dds
            ├── trait_very_strong.dds
            ├── ui_trait_agile.dds
            ├── ui_trait_brave.dds
            ├── ui_trait_craven.dds
            ├── ui_trait_creative.dds
            ├── ui_trait_deceitful.dds
            ├── ui_trait_demanding.dds
            ├── ui_trait_dumb.dds
            └── ... (60 files total)
```

No subdirectories are used.

## Format requirements

| Property | Requirement |
|----------|-------------|
| Container | DDS (DirectDraw Surface) |
| Resolution | Match the vanilla trait icons so the grid remains uniform |
| Alpha | Preserve if the artwork uses transparency |
| File name | Must match the identifier exactly, including case |

The repository records no compression setting or colour-space metadata, so confirm any new icon against an existing file in the same directory.

## Source artwork

**File:** `src/trait.xcf`
**Format:** GIMP layered source

### Creating an icon

1. Open `src/trait.xcf` in GIMP.
2. Duplicate the layer of the closest existing trait and adapt the artwork.
3. Export as DDS with the file name `ui_trait_<identifier>.dds`.
4. Place the file in `ui-species-traits/gfx/interface/icons/traits/`.
5. Verify that the file name matches the identifier exactly.
6. Verify the icon in game.

### Updating an icon

1. Modify the relevant layer in `src/trait.xcf`.
2. Export the DDS again.
3. Replace the existing file under `gfx/interface/icons/traits/`.
4. Verify the icon in game.

### Removing an icon

1. Delete `ui_trait_<identifier>.dds`.
2. Remove the corresponding layer from `src/trait.xcf` if it is no longer required.

## Completeness check

The current state is complete: 58 `ui_trait_*` icons for 58 identifiers. A missing icon renders a placeholder in the trait selector, and the engine logs no dedicated error for it, so verify visually after any icon change.

Checklist:
- [ ] Every `ui_trait_*` identifier has a matching `.dds` file.
- [ ] File names match identifiers exactly.
- [ ] All files open as valid DDS images.
- [ ] No subdirectories exist under `traits/`.
- [ ] No placeholder icons appear in game.

## Related documentation

| Document | Scope |
|----------|-------|
| [Data model](../data-model.md) | Icon convention, completeness |
| [Architecture](../architecture.md) | Asset area, interfaces |
| [Components/trait-definitions.md](./trait-definitions.md) | Identifiers requiring icons |
| [Components/localisation.md](./localisation.md) | Parallel naming convention |