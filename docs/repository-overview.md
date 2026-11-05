# Repository Overview

This document summarises the purpose, scope, entry points, and capabilities of the Universum Infinitum - Species Traits repository.

## Purpose

The repository hosts a Stellaris mod that adds 58 custom species traits, extends vanilla trait families with additional Very and Extremely tiers, and raises the maximum number of selectable traits per species archetype. The mod is a static data package: Clausewitz script definitions, YAML localisation, and DDS icons, interpreted by the Stellaris engine at load time.

## Scope

| Included | Excluded |
|----------|----------|
| Trait definitions (`common/traits/*.txt`) | Executable code |
| Archetype limit adjustments (`common/species_archetypes/`) | Runtime services or processes |
| English localisation (`localisation/english/*.yml`) | Languages other than English |
| Trait icons (`gfx/interface/icons/traits/*.dds`) | Icon source files (only `src/trait.xcf` kept) |
| Mod descriptors (`*.mod`) | Save-game data |
| Release workflows (`.github/workflows/`) | Automated content validation |

## Entry points

| Entry point | Role |
|-------------|------|
| `ui-species-traits.mod` | Repository-root descriptor used by the Stellaris launcher to register the mod |
| `ui-species-traits/descriptor.mod` | Mod-root descriptor read by the game engine |
| `ui-species-traits/common/traits/ui-traits_species.txt` | 38 custom trait definitions |
| `ui-species-traits/common/traits/ui-traits_species_newtiers.txt` | 20 Very/Extremely tier extensions for vanilla trait families |
| `ui-species-traits/common/species_archetypes/00_species_archetypes.txt` | Trait points and max-trait limits per archetype |
| `docs/INDEX.md` | Documentation navigation index |

## Capabilities

- 38 custom positive and negative traits spanning combat, population, science, economy, habitability, lifespan, social, and special categories
- 20 additional tiers (Very, Extremely) extending 11 vanilla trait families
- Increased selection limits: 7 traits for BIOLOGICAL and MACHINE archetypes, 6 for ROBOT
- Dedicated DDS icons for every trait, loaded by filename convention (`GFX_trait_<id>`)
- English names and descriptions for all traits

## Distribution channels

| Channel | Mechanism |
|---------|-----------|
| Steam Workshop | `steam-workshop.yml` release workflow |
| Nexus Mods | `nexus-mods.yml` release workflow |
| Paradox Mods | Manual upload |
| GitHub Releases | Release archives consumed by the workflows |

## Version targeting

The descriptors declare `supported_version="v4.5.*"`. Documentation stating `4.4.*` predates this and requires reconciliation; see [operational/dependencies.md](./operational/dependencies.md).

## Related documents

| Document | Scope |
|----------|-------|
| [README.md](../README.md) | Project overview, installation, capabilities |
| [ARCHITECTURE.md](../ARCHITECTURE.md) | Architectural boundaries and contracts |
| [repository-structure.md](./repository-structure.md) | Source tree layout |
| [architecture.md](./architecture.md) | In-depth architectural documentation |
