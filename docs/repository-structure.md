# Repository structure

This document describes the source tree layout and module organisation of the Universum Infinitum - Species Traits mod. It complements the root [ARCHITECTURE.md](../ARCHITECTURE.md) and [repository-overview.md](./repository-overview.md).

## Source tree

```
stellaris-ui-species-traits/
├── .github/
│   ├── FUNDING.yml                    # Funding configuration
│   ├── cover.png                      # Repository cover image
│   └── workflows/
│       ├── github-release.yml         # GitHub Release workflow
│       ├── nexus-mods-release.yml     # Nexus Mods release workflow
│       └── steam-workshop-release.yml # Steam Workshop release workflow
├── src/
│   └── trait.xcf                      # GIMP source file for trait icons
├── ui-species-traits/                 # Mod package root (installed into the Stellaris mod directory)
│   ├── descriptor.mod                 # Mod descriptor read by the engine
│   ├── common/
│   │   ├── species_archetypes/
│   │   │   └── 00_species_archetypes.txt   # Archetype trait limits
│   │   └── traits/
│   │       ├── ui-traits_species.txt       # 38 trait definitions
│   │       └── ui-traits_species_newtiers.txt  # 20 additional tiers
│   ├── gfx/
│   │   └── interface/
│   │       └── icons/
│   │           └── traits/                 # 60 DDS icon files
│   └── localisation/
│       └── english/
│           ├── ui-species-traits_l_english.yml       # Localisation for trait definitions
│           └── ui-species-traits_newtiers_l_english.yml  # Localisation for additional tiers
├── ui-species-traits.mod              # Mod descriptor read by the launcher (repository root)
├── ARCHITECTURE.md                    # Root architecture documentation
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE
├── PRIVACY.md
├── README.md
├── SECURITY.md
└── docs/                              # Detailed documentation (this directory)
```

## Module organisation

| Directory | Purpose | File types |
|-----------|---------|------------|
| `ui-species-traits/common/species_archetypes/` | Archetype trait limits | Clausewitz script (`.txt`) |
| `ui-species-traits/common/traits/` | Trait definitions | Clausewitz script (`.txt`) |
| `ui-species-traits/localisation/english/` | English localisation | YAML (`.yml`) |
| `ui-species-traits/gfx/interface/icons/traits/` | Trait icons | DDS (`.dds`) |
| `src/` | Icon source artwork | GIMP (`.xcf`) |
| `.github/workflows/` | Release automation | YAML (`.yml`) |

## Key files

| File | Role |
|------|------|
| `ui-species-traits.mod` / `ui-species-traits/descriptor.mod` | Mod registration; declares `version`, `name`, `path`, `tags`, and `supported_version` |
| `common/species_archetypes/00_species_archetypes.txt` | Defines trait points and maximum traits per archetype through shared variables |
| `common/traits/ui-traits_species.txt` | 38 trait definitions with cost, opposites, allowed archetypes, and modifiers |
| `common/traits/ui-traits_species_newtiers.txt` | 20 additional Very and Extremely tiers for vanilla trait families |
| `localisation/english/ui-species-traits_l_english.yml` | English names and descriptions for the trait definitions |
| `localisation/english/ui-species-traits_newtiers_l_english.yml` | English names and descriptions for the additional tiers |
| `gfx/interface/icons/traits/*.dds` | 60 icons resolved by filename convention |

## Naming conventions

- Trait identifiers: `ui_trait_<name>` for trait definitions; `ui_trait_very_<name>` and `ui_trait_extremely_<name>` for additional tiers
- Localisation keys: `ui_trait_<name>:0` for the name and `ui_trait_<name>_desc:0` for the description
- Icon filenames: `ui_trait_<name>.dds` or `trait_<name>.dds`, corresponding to the `GFX_trait_<name>` reference resolved by the engine
- File encoding: the additional-tiers localisation file carries a UTF-8 byte-order mark; see [components/localisation.md](./components/localisation.md)

## Distribution

The release workflows write the released version into both descriptors, package the mod directory and the root descriptor into a ZIP archive, and publish it to GitHub Releases, Nexus Mods, and the Steam Workshop. Paradox Mods remains a manual upload; see [operational/build-and-deployment.md](./operational/build-and-deployment.md).

## Related documents

| Document | Scope |
|----------|-------|
| [Repository overview](./repository-overview.md) | Purpose, scope, entry points |
| [Architecture](./architecture.md) | Architectural areas, interfaces, contracts |
| [Configuration](./operational/configuration.md) | Descriptor fields and values |
| Root [ARCHITECTURE.md](../ARCHITECTURE.md) | High-level architectural boundaries |