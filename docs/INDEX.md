# Index

This is the master navigation index for the Universum Infinitum - Species Traits mod documentation.

## Table of contents

```
docs/
├── INDEX.md                          # This file
├── repository-overview.md            # Repository purpose, scope, entry points
├── repository-structure.md           # Source tree layout, module organisation
├── architecture.md                   # Architectural areas, interfaces, contracts
├── data-model.md                     # Trait schema, scaling policy, archetype variables
├── components/
│   ├── trait-definitions.md          # 38 custom traits + 20 tier extensions
│   ├── archetype-config.md           # Archetype variable definitions and limits
│   ├── localisation.md               # Localisation file structure and formatting
│   └── assets.md                     # Icon naming and artwork workflow
├── flows/
│   ├── mod-load.md                   # Engine load sequence from launcher to interface
│   ├── species-creation.md           # Species creation interface flow
│   └── trait-selection.md            # Selection mechanics, opposites, persistence
├── behaviour/
│   ├── trait-effects.md              # Modifier application, scope, stacking
│   └── scaling.md                    # Very and Extremely tier scaling policy
└── operational/
    ├── build-and-deployment.md       # Release workflows, packaging, distribution
    ├── configuration.md              # Descriptor fields, supported_version
    ├── dependencies.md               # Engine dependencies and version coupling
    └── testing.md                    # Manual verification procedures
```

## Quick navigation

### Architecture
- [Architecture](./architecture.md) — Mod areas, interfaces, contracts
- [Data model](./data-model.md) — Trait schema, scaling policy

### Components
- [Trait definitions](./components/trait-definitions.md) — Complete trait catalogue
- [Archetype configuration](./components/archetype-config.md) — Archetype limits
- [Localisation](./components/localisation.md) — Localisation structure
- [Assets](./components/assets.md) — Icon convention and artwork workflow

### Flows
- [Mod load](./flows/mod-load.md) — Engine load sequence
- [Species creation](./flows/species-creation.md) — Creation interface flow
- [Trait selection](./flows/trait-selection.md) — Selection mechanics

### Behaviour
- [Trait effects](./behaviour/trait-effects.md) — Modifier application
- [Scaling](./behaviour/scaling.md) — Tier scaling policy

### Operational
- [Build and deployment](./operational/build-and-deployment.md) — Release process
- [Configuration](./operational/configuration.md) — Descriptor configuration
- [Dependencies](./operational/dependencies.md) — Engine dependencies
- [Testing](./operational/testing.md) — Manual verification procedures

## Cross-references

| Document | References |
|----------|------------|
| [repository-overview.md](./repository-overview.md) | Links to ARCHITECTURE.md, README.md |
| [repository-structure.md](./repository-structure.md) | Links to ARCHITECTURE.md |
| [architecture.md](./architecture.md) | Links to all component documents and README.md |
| [data-model.md](./data-model.md) | Links to component documents and ARCHITECTURE.md |
| [components/trait-definitions.md](./components/trait-definitions.md) | Links to data-model, localisation, assets |
| [components/archetype-config.md](./components/archetype-config.md) | Links to data-model, trait-definitions |
| [components/localisation.md](./components/localisation.md) | Links to data-model, trait-definitions |
| [components/assets.md](./components/assets.md) | Links to data-model, trait-definitions |
| [flows/mod-load.md](./flows/mod-load.md) | Links to architecture, components |
| [flows/species-creation.md](./flows/species-creation.md) | Links to mod-load, trait-selection |
| [flows/trait-selection.md](./flows/trait-selection.md) | Links to species-creation, trait-definitions |
| [behaviour/trait-effects.md](./behaviour/trait-effects.md) | Links to trait-definitions, data-model |
| [behaviour/scaling.md](./behaviour/scaling.md) | Links to data-model, trait-definitions |
| [operational/build-and-deployment.md](./operational/build-and-deployment.md) | Links to configuration, dependencies |
| [operational/configuration.md](./operational/configuration.md) | Links to dependencies, build-and-deployment |
| [operational/dependencies.md](./operational/dependencies.md) | Links to configuration, build-and-deployment |
| [operational/testing.md](./operational/testing.md) | Links to architecture, components, build-and-deployment |

## Related root documents

| Document | Scope |
|----------|-------|
| [ARCHITECTURE.md](../ARCHITECTURE.md) | Architectural boundaries and contracts |
| [README.md](../README.md) | Project overview, installation, capabilities |
| [CONTRIBUTING.md](../CONTRIBUTING.md) | Contribution guidelines |
| [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md) | Community standards |
| [SECURITY.md](../SECURITY.md) | Security policy |
| [PRIVACY.md](../PRIVACY.md) | Privacy statement |
| [LICENSE](../LICENSE) | Licence terms |

## Trait catalogue (quick reference)

| Group in the definition files | Traits | Examples |
|-------------------------------|--------|----------|
| Combat and army | 12 | `ui_trait_brave`, `ui_trait_craven`, `ui_trait_agile`, `ui_trait_extremely_strong` |
| Research and unity | 9 | `ui_trait_creative`, `ui_trait_uncreative`, `ui_trait_dumb` |
| Economy and upkeep | 12 | `ui_trait_demanding`, `ui_trait_undemanding`, `ui_trait_gluttonous`, `ui_trait_light_eaters` |
| Habitability | 3 | `ui_trait_very_adaptive`, `ui_trait_very_nonadaptive`, `ui_trait_extremely_nonadaptive` |
| Population growth | 4 | `ui_trait_very_rapid_breeders`, `ui_trait_extremely_slow_breeders` |
| Lifespan | 5 | `ui_trait_long_lived`, `ui_trait_momentary`, `ui_trait_very_short_lived` |
| Social cohesion and civic conduct | 6 | `ui_trait_deceitful`, `ui_trait_trusting`, `ui_trait_very_docile`, `ui_trait_very_unruly` |
| Infrastructure and talent | 5 | `ui_trait_urban`, `ui_trait_nonresilient`, `ui_trait_very_talented` |
| **Total** | **58** | — |

The catalogue counts distinguish 38 entries in `common/traits/ui-traits_species.txt` from 20 entries in `common/traits/ui-traits_species_newtiers.txt`; see [components/trait-definitions.md](./components/trait-definitions.md) for the full listing.

## Version history

| Version | Date | Changes |
|---------|------|---------|
| v1.0 | 2020-04-04 | First release |
| v1.1 – v1.1.5 | 2020-04-07 – 2021-12-14 | Additional traits, artwork, and Stellaris 3.2 support |
| v2.0.0 | 2026-08-14 | Tier scaling and opposites rework; release workflows |
| v2.0.1 | 2026-10-24 | Compatibility update for Stellaris 4.5 |
| v2.0.2 | 2026-11-18 | English localisation revision |

Release notes are maintained on GitHub Releases; this table records release identity only.

## Contact

For questions about the mod or its documentation:
- GitHub repository: https://github.com/hmlendea/stellaris-ui-species-traits
- Wiki: https://github.com/hmlendea/stellaris-ui-species-traits/wiki/Traits

## Contributing

See [CONTRIBUTING.md](../CONTRIBUTING.md) for contribution guidelines.

## Licence

See [LICENSE](../LICENSE) for licence terms.

## Disclaimer

This documentation records the current state of the mod as far as the repository can evidence it. Verify against the source files prior to making changes.