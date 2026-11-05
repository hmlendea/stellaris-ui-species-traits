# Index

This is the master navigation index for the Universum Infinitum - Species Traits mod documentation.

## Table of contents

```
docs/
├── INDEX.md                          # This file
├── repository-overview.md             # Repository purpose, scope, entry points
├── repository-structure.md            # Source tree layout, module organisation
├── architecture.md                   # Architectural areas, interfaces, contracts
├── data-model.md                     # Trait schema, scaling policy, archetype variables
├── components/
│   ├── trait-definitions.md           # 38 custom traits + 20 vanilla tier extensions
│   ├── archetype-config.md            # Archetype variable definitions and limits
│   ├── localisation.md                # Localisation file structure and formatting
│   └── assets.md                      # Icon naming convention and pipeline
├── flows/
│   ├── mod-load.md                    # Engine load sequence from launcher to UI
│   ├── species-creation.md            # Species creation UI flow
│   └── trait-selection.md             # Selection mechanics, AI behaviour, opposites
├── behaviour/
│   ├── trait-effects.md              # Modifier application, stacking, scope
│   └── scaling.md                    # Very/Extremely tier scaling policy
└── operational/
    ├── build-and-deployment.md        # Release workflows, packaging, distribution
    ├── configuration.md               # Descriptor fields, supported_version
    ├── dependencies.md                # External dependencies, version coupling
    └── testing.md                     # Manual testing procedures and checklist
```

## Quick navigation

### Architecture
- [Architecture](./architecture.md) — Mod areas, interfaces, contracts
- [Data model](./data-model.md) — Trait schema, scaling policy

### Components
- [Trait definitions](./components/trait-definitions.md) — 58 traits catalogue
- [Archetype config](./components/archetype-config.md) — Archetype limits
- [Localisation](./components/localisation.md) — Localisation structure
- [Assets](./components/assets.md) — Icon pipeline

### Flows
- [Mod load](./flows/mod-load.md) — Engine load sequence
- [Species creation](./flows/species-creation.md) — Creation UI flow
- [Trait selection](./flows/trait-selection.md) — Selection mechanics

### Behaviour
- [Trait effects](./behaviour/trait-effects.md) — Modifier application
- [Scaling](./behaviour/scaling.md) — Tier scaling policy

### Operational
- [Build and deployment](./operational/build-and-deployment.md) — Release process
- [Configuration](./operational/configuration.md) — Descriptor configuration
- [Dependencies](./operational/dependencies.md) — External dependencies
- [Testing](./operational/testing.md) — Manual testing procedures

## Cross-references

| Document | References |
|----------|------------|
| [repository-overview.md](./repository-overview.md) | Links to ARCHITECTURE.md, README.md |
| [repository-structure.md](./repository-structure.md) | Links to ARCHITECTURE.md |
| [architecture.md](./architecture.md) | Links to all component docs, README.md |
| [data-model.md](./data-model.md) | Links to component docs, ARCHITECTURE.md |
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
| [ARCHITECTURE.md](../ARCHITECTURE.md) | High-level architectural boundaries |
| [README.md](../README.md) | Project overview, installation, capabilities |
| [CONTRIBUTING.md](../CONTRIBUTING.md) | Contribution guidelines |
| [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md) | Community standards |
| [SECURITY.md](../SECURITY.md) | Security policy |
| [PRIVACY.md](../PRIVACY.md) | Privacy policy |
| [ROADMAP.md](../ROADMAP.md) | Future plans |
| [LICENSE](../LICENSE) | License terms |

## Trait catalogue (quick reference)

| Category | Count | Custom traits | Vanilla tiers |
|----------|-------|---------------|--------------|
| Combat | 8 | `ui_trait_brave`, `ui_trait_craven`, `ui_trait_agile`, `ui_trait_sluggish` (+ Very/Extremely) | — |
| Population | 6 | `ui_trait_rapid_breeders`, `ui_trait_slow_breeders` (+ Very/Extremely) | — |
| Science | 6 | `ui_trait_creative`, `ui_trait_uncreative` (+ Very/Extremely) | — |
| Economy | 12 | `ui_trait_demanding`, `ui_trait_undemanding`, `ui_trait_gluttonous`, `ui_trait_light_eaters` (+ Very/Extremely) | — |
| Habitability | 6 | `ui_trait_adaptive`, `ui_trait_nonadaptive` (+ Very/Extremely) | — |
| Lifespan | 6 | `ui_trait_long_lived`, `ui_trait_short_lived` (+ Very/Extremely) | — |
| Social | 6 | `ui_trait_trusting`, `ui_trait_deceitful`, `ui_trait_optimistic`, `ui_trait_pessimistic`, `ui_trait_docile`, `ui_trait_very_docile` | — |
| Special | 7 | `ui_trait_urban`, `ui_trait_uneconomical`, `ui_trait_poor_engineers`, `ui_trait_poor_physicists`, `ui_trait_poor_sociologists`, `ui_trait_lazy`, `ui_trait_momentary`, `ui_trait_nonresilient` | — |
| Vanilla tiers (Very/Extremely) | 20 | — | 11 families × 2 tiers |
| **Total** | **58** | **38** | **20** |

## Version history

| Version | Date | Changes |
|---------|------|---------|
| v1.0.0 | Initial release | First release with all 58 traits |
| v1.1.0 | Later | Bug fixes, balance adjustments |
| v1.2.0 | Later | New trait additions, icon updates |
| v1.2.3 | Current | Latest release |

*Note: Specific version dates are not tracked in this documentation. Check GitHub Releases for exact dates.*

## Contact

For questions about the mod or documentation:
- GitHub repository: https://github.com/hmlendea/stellaris-ui-species-traits
- Wiki: https://github.com/hmlendea/stellaris-ui-species-traits/wiki/Traits
- Discord (if available): [Discord server link]

## Contributing

See [CONTRIBUTING.md](../CONTRIBUTING.md) for contribution guidelines.

## License

See [LICENSE](../LICENSE) for license terms.

## Disclaimer

This documentation is generated as part of institutional knowledge preservation. It may not reflect the latest state of the mod. Always verify against the current source files before making changes.
