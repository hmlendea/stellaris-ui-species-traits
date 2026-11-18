# Repository Overview

This document summarises the purpose, scope, entry points, and capabilities of the Universum Infinitum - Species Traits repository.

## Purpose

The repository hosts a Stellaris mod that adds 58 species trait definitions, extends established vanilla trait families with additional Very and Extremely tiers, and raises the maximum number of selectable traits per species archetype. The mod is a static content package: Clausewitz script definitions, YAML localisation, and DDS icons, interpreted by the Stellaris engine at load time.

## Scope

| Included | Excluded |
|----------|----------|
| Trait definitions (`common/traits/*.txt`) | Executable code |
| Archetype limit adjustments (`common/species_archetypes/`) | Runtime services or processes |
| English localisation (`localisation/english/*.yml`) | Languages other than English |
| Trait icons (`gfx/interface/icons/traits/*.dds`) | Additional icon source files (`src/trait.xcf` holds the sole artwork source) |
| Mod descriptors (`*.mod`) | Save-game data |
| Release workflows (`.github/workflows/`) | Automated content validation |

## Entry points

| Entry point | Role |
|-------------|------|
| `ui-species-traits.mod` | Repository-root descriptor used by the Stellaris launcher to register the mod |
| `ui-species-traits/descriptor.mod` | Mod-root descriptor read by the game engine |
| `ui-species-traits/common/traits/ui-traits_species.txt` | 38 trait definitions |
| `ui-species-traits/common/traits/ui-traits_species_newtiers.txt` | 20 additional tiers for vanilla trait families |
| `ui-species-traits/common/species_archetypes/00_species_archetypes.txt` | Trait points and maximum trait counts per archetype |
| `docs/INDEX.md` | Documentation navigation index |

## Capabilities

- 58 trait definitions spanning combat and army, research and unity, economy and upkeep, habitability, population growth, lifespan, social cohesion, and infrastructure
- 20 additional Very and Extremely tiers extending established vanilla trait families
- Raised selection limits: two trait points and seven traits for `BIOLOGICAL` and `PRESAPIENT` (inherited by `LITHOID`), one point and seven traits for `MACHINE`, and zero points and six traits for `ROBOT`
- Dedicated DDS icons for every trait, resolved by filename convention
- English names and descriptions for all 58 traits

## Distribution channels

| Channel | Mechanism |
|---------|-----------|
| Steam Workshop | `steam-workshop-release.yml` release workflow |
| Nexus Mods | `nexus-mods-release.yml` release workflow |
| Paradox Mods | Manual upload |
| GitHub Releases | `github-release.yml` release workflow, triggered by a published release |

## Version targeting

Both descriptors declare `supported_version="v4.5.*"` and `version="2.0.2"`. The launcher evaluates these at activation; see [operational/dependencies.md](./operational/dependencies.md) for the compatibility window.

## Related documents

| Document | Scope |
|----------|-------|
| [README.md](../README.md) | Project overview, installation, capabilities |
| [ARCHITECTURE.md](../ARCHITECTURE.md) | Architectural boundaries and contracts |
| [repository-structure.md](./repository-structure.md) | Source tree layout |
| [architecture.md](./architecture.md) | Detailed architectural documentation |