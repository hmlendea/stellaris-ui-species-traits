# Repository structure

This document describes the source tree layout and module organisation of the Universum Infinitum - Species Traits mod. It complements the root [ARCHITECTURE.md](../ARCHITECTURE.md) and [repository-overview.md](./repository-overview.md).

## Source tree

```
stellaris-ui-species-traits/
├── .github/
│   └── workflows/
│       ├── nexus-mods.yml      # Nexus Mods release workflow
│       └── steam-workshop.yml  # Steam Workshop release workflow
├── src/
│   └── trait.xcf               # GIMP source file for trait icons
├── ui-species-traits/          # Mod package root (extracted to Stellaris mod directory)
│   ├── descriptor.mod          # Mod descriptor for launcher (inside mod folder)
│   ├── common/
│   │   ├── species_archetypes/
│   │   │   └── 00_species_archetypes.txt   # Archetype trait limits
│   │   └── traits/
│   │       ├── ui-traits_species.txt       # 38 custom traits
│   │       └── ui-traits_species_newtiers.txt  # 20 vanilla-family tier extensions
│   ├── gfx/
│   │   └── interface/
│   │       └── icons/
│   │           └── traits/                 # 58 DDS icon files
│   └── localisation/
│       └── english/
│           ├── ui-species-traits_l_english.yml       # Localisation for custom traits
│           └── ui-species-traits_newtiers_l_english.yml  # Localisation for tier extensions
├── ui-species-traits.mod       # Mod descriptor for launcher (repository root)
├── ARCHITECTURE.md             # Root architecture documentation
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE
├── PRIVACY.md
├── README.md
├── ROADMAP.md
├── SECURITY.md
└── docs/                       # Generated documentation (this directory)
```

## Module organisation

| Directory | Purpose | File types |
|-----------|---------|------------|
| `ui-species-traits/common/species_archetypes/` | Archetype configuration | Clausewitz script (`.txt`) |
| `ui-species-traits/common/traits/` | Trait definitions | Clausewitz script (`.txt`) |
| `ui-species-traits/localisation/english/` | English localisation | YAML (`.yml`) |
| `ui-species-traits/gfx/interface/icons/traits/` | Trait icons | DDS (`.dds`) |
| `src/` | Icon source assets | GIMP (`.xcf`) |
| `.github/workflows/` | CI/CD release automation | YAML (`.yml`) |

## Key files

| File | Role |
|------|------|
| `ui-species-traits.mod` / `ui-species-traits/descriptor.mod` | Mod registration; declares `name`, `path`, `tags`, `supported_version` |
| `common/species_archetypes/00_species_archetypes.txt` | Defines trait points and max traits per archetype via variables |
| `common/traits/ui-traits_species.txt` | 38 custom trait definitions with cost, category, opposites, modifiers |
| `common/traits/ui-traits_species_newtiers.txt` | 20 Very/Extremely tier extensions for vanilla trait families |
| `localisation/english/ui-species-traits_l_english.yml` | English names/descriptions for custom traits |
| `localisation/english/ui-species-traits_newtiers_l_english.yml` | English names/descriptions for tier extensions |
| `gfx/interface/icons/traits/*.dds` | 58 icons loaded by filename convention (`GFX_trait_<id>`) |

## Naming conventions

- Trait IDs: `ui_trait_<name>` for custom traits; `ui_trait_very_<name>` / `ui_trait_extremely_<name>` for tier extensions
- Localisation keys: `ui_trait_<name>:0` (name), `ui_trait_<name>_desc:0` (description)
- Icon filenames: `ui_trait_<name>.dds` or `trait_<name>.dds` matching the `GFX_trait_<name>` reference
- File encoding: UTF-8 with BOM for `.yml` (Stellaris requirement)

## Distribution

The mod is distributed as a ZIP archive containing the `ui-species-traits/` directory and `ui-species-traits.mod` at the root. Release workflows download the GitHub release asset and push to Steam Workshop and Nexus Mods.