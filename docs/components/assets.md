# Component: Assets (Icons)

This document details the trait icon pipeline, naming convention, format requirements, and source assets.

## File location

**Directory:** `ui-species-traits/gfx/interface/icons/traits/`
**Count:** 58 `.dds` files

## Naming convention

The engine loads icons by filename convention, not by explicit `icon` field in trait definitions.

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

## Format requirements

| Property | Requirement |
|----------|-------------|
| Format | DDS (DirectDraw Surface) |
| Compression | BC3 / DXT5 (recommended) |
| Resolution | 64×64 or 128×128 pixels |
| Colour space | sRGB |
| Alpha channel | Yes (for transparency) |
| Mipmaps | Optional (Stellaris generates if missing) |

## Directory structure

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
            ├── ui_trait_extremely_agile.dds
            ├── ui_trait_extremely_creative.dds
            ├── ui_trait_extremely_demanding.dds
            ├── ui_trait_extremely_gluttonous.dds
            ├── ui_trait_extremely_light_eaters.dds
            ├── ui_trait_extremely_nonadaptive.dds
            ├── ui_trait_extremely_rapid_breeders.dds
            ├── ui_trait_extremely_short_lived.dds
            ├── ui_trait_extremely_sluggish.dds
            ├── ui_trait_extremely_strong.dds
            ├── ui_trait_extremely_uncreative.dds
            ├── ui_trait_extremely_undemanding.dds
            ├── ui_trait_extremely_weak.dds
            ├── ui_trait_gluttonous.dds
            ├── ui_trait_lazy.dds
            ├── ui_trait_light_eaters.dds
            ├── ui_trait_long_lived.dds
            ├── ui_trait_momentary.dds
            ├── ui_trait_nonresilient.dds
            ├── ui_trait_optimistic.dds
            ├── ui_trait_pessimistic.dds
            ├── ui_trait_poor_engineers.dds
            ├── ui_trait_poor_physicists.dds
            ├── ui_trait_poor_sociologists.dds
            ├── ui_trait_short_lived.dds
            ├── ui_trait_sluggish.dds
            ├── ui_trait_trusting.dds
            ├── ui_trait_uncreative.dds
            ├── ui_trait_undemanding.dds
            ├── ui_trait_uneconomical.dds
            ├── ui_trait_urban.dds
            ├── ui_trait_very_adaptive.dds
            ├── ui_trait_very_agile.dds
            ├── ui_trait_very_creative.dds
            ├── ui_trait_very_demanding.dds
            ├── ui_trait_very_docile.dds
            ├── ui_trait_very_gluttonous.dds
            ├── ui_trait_very_light_eaters.dds
            ├── ui_trait_very_long_lived.dds
            ├── ui_trait_very_natural_engineers.dds
            ├── ui_trait_very_natural_physicists.dds
            ├── ui_trait_very_natural_sociologists.dds
            ├── ui_trait_very_nonadaptive.dds
            ├── ui_trait_very_poor_engineers.dds
            ├── ui_trait_very_poor_physicists.dds
            ├── ui_trait_very_poor_sociologists.dds
            ├── ui_trait_very_rapid_breeders.dds
            ├── ui_trait_very_repugnant.dds
            ├── ui_trait_very_short_lived.dds
            ├── ui_trait_very_slow_breeders.dds
            ├── ui_trait_very_slow_learners.dds
            ├── ui_trait_very_strong.dds
            ├── ui_trait_very_thrifty.dds
            ├── ui_trait_very_wasteful.dds
            ├── ui_trait_very_weak.dds
            └── ... (58 total)
```

No subdirectories; all icons directly in `traits/`.

## Source assets

**File:** `src/trait.xcf`
**Format:** GIMP (`.xcf`) — layered source for icon creation
**Purpose:** Master file for creating/editing trait icons before exporting to DDS

### Workflow

1. Open `src/trait.xcf` in GIMP
2. Create/edit layer for new trait icon
3. Export as DDS: File → Export As → `ui_trait_<name>.dds`
4. Place in `ui-species-traits/gfx/interface/icons/traits/`
5. Verify in-game

## Completeness requirement

Every trait ID in `common/traits/*.txt` must have a matching `.dds` file in `gfx/interface/icons/traits/`.

| Trait count | Icon files required | Status |
|-------------|---------------------|--------|
| 38 custom traits | 38 | ✅ Complete |
| 20 vanilla-extended tiers | 20 | ✅ Complete |
| **Total** | **58** | ✅ Complete |

Missing icons show a placeholder (generic question mark) in-game.

## Icon design guidelines

### Visual style

- Consistent with Stellaris vanilla trait icons
- Clean, recognisable at 64×64
- Colour coding by trait category:
  - **Physical/Combat:** Red/orange tones
  - **Social:** Blue/purple tones
  - **Biological:** Green/brown tones
  - **Industry/Research:** Yellow/gold tones
  - **Negative traits:** Desaturated, darker variants

### Tier visual progression

| Tier | Visual indicator |
|------|------------------|
| Base (vanilla) | Standard icon |
| Very | Enhanced glow, additional elements, brighter |
| Extremely | Maximum glow, complex elements, most saturated |

Example: `ui_trait_strong` → `ui_trait_very_strong` → `ui_trait_extremely_strong` show progressive intensification.

## Maintenance procedures

### Adding an icon for a new trait

1. Open `src/trait.xcf` in GIMP
2. Duplicate an existing similar trait layer
3. Modify artwork for new trait
4. Export as DDS: `ui_trait_<new_id>.dds`
5. Copy to `ui-species-traits/gfx/interface/icons/traits/`
6. Verify filename matches trait ID exactly
7. Test in-game

### Updating an existing icon

1. Open `src/trait.xcf` in GIMP
2. Modify the relevant layer
3. Re-export DDS
4. Replace file in `gfx/interface/icons/traits/`
5. Test in-game

### Removing an icon (trait deleted)

1. Delete `ui_trait_<id>.dds` from `gfx/interface/icons/traits/`
2. Optionally remove layer from `src/trait.xcf`

## DDS export settings (GIMP)

Recommended export options:
- **Compression:** BC3 / DXT5
- **Generate mipmaps:** Yes
- **sRGB colour space:** Yes
- **Alpha channel:** Preserve

## Verification checklist

- [ ] All 58 trait IDs have matching `.dds` files
- [ ] Filenames match trait IDs exactly (case-sensitive)
- [ ] All files are valid DDS (open in GIMP or texture viewer)
- [ ] Resolution is 64×64 or 128×128
- [ ] No subdirectories in `traits/`
- [ ] In-game: no placeholder icons in species creation trait picker

## Related documentation

| Document | Scope |
|----------|-------|
| [Data model](./data-model.md) | Icon naming convention, completeness |
| [Architecture](./architecture.md) | Asset area, interfaces |
| [Components/trait-definitions.md](./trait-definitions.md) | Trait IDs requiring icons |
| [Components/localisation.md](./localisation.md) | Parallel naming convention |
| Root [ARCHITECTURE.md](../ARCHITECTURE.md) | High-level asset area |