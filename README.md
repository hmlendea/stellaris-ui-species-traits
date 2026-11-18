[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/funding)
[![Latest Release](https://img.shields.io/github/v/release/hmlendea/stellaris-ui-species-traits)](https://github.com/hmlendea/stellaris-ui-species-traits/releases/latest)
[![License](https://img.shields.io/github/license/hmlendea/stellaris-ui-species-traits)](https://github.com/hmlendea/stellaris-ui-species-traits/blob/master/LICENSE)

# Universum Infinitum - Species Traits

A Stellaris mod that adds 58 species trait definitions, extending established vanilla trait families with additional Very and Extremely tiers and increasing the maximum number of selectable traits.

## 📑 Table of Contents

- [Capabilities](#-capabilities)
- [Usage](#-usage)
- [Known Limitations](#-known-limitations)
- [System Requirements](#-system-requirements)
- [Installation](#-installation)
  - [Manual Installation](#manual-installation)
- [Configuration](#-configuration)
  - [Settings](#settings)
- [Localisation](#-localisation)
- [Compatibility](#-compatibility)
- [Privacy and Data](#-privacy-and-data)
- [GitHub Actions](#-github-actions)
- [Project Structure](#-project-structure)
  - [Directories](#directories)
- [Architecture](#-architecture)
- [Documentation](#-documentation)
- [Contributing](#-contributing)
- [Code of Conduct](#-code-of-conduct)
- [Related Projects](#-related-projects)
- [Security](#-security)
- [Support](#-support)
- [Project Engagement](#-project-engagement)
- [License](#-license)

## ✨ Capabilities

- Select from 58 positive and negative species trait definitions covering combat, population, science, economy, habitability, lifespan, social conduct, and specialisation
- Use additional Very and Extremely tiers that extend established vanilla trait families
- Select up to seven biological, presapient, or machine traits and six robot traits
- Recognise each added trait through a dedicated icon and an English description

## 🚀 Usage

After installing the mod, use it through the Stellaris launcher:

1. Add **Universum Infinitum - Species Traits** to the active playset and activate it.
2. Start Stellaris and open species creation.
3. Select the additional traits subject to the available trait points and trait-count limits.

## ⚠️ Known Limitations

- Trait definitions, localisation keys, and icons are verified manually in-game; the repository contains no automated content validation
- Trait points and maximum trait counts apply per species archetype, so a species is unable to exceed those limits even when additional traits are available

## 🖥️ System Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| Stellaris | `4.5.0` | Latest `4.5.*` release |

## 📦 Installation

[![Obtain it from Steam Workshop](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/steam-workshop.png)](https://steamcommunity.com/sharedfiles/filedetails/?id=2047811098)
[![Obtain it from Nexus Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/nexus.png)](https://nexusmods.com/stellaris/mods/78)
[![Obtain it from Paradox Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/paradox-mods.png)](https://mods.paradoxplaza.com/mods/25445/Any)
[![Obtain it from GitHub](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/github.png)](https://github.com/hmlendea/stellaris-ui-species-traits/releases)

### Manual Installation

1. Download the desired archive from [GitHub Releases](https://github.com/hmlendea/stellaris-ui-species-traits/releases).
2. Extract the archive so that `ui-species-traits.mod` and the `ui-species-traits/` directory reside directly in the Stellaris mod directory.
3. Activate the mod in the Stellaris launcher.

## ⚙️ Configuration

The mod is configured through its two mod descriptors, which declare the mod identity, path, tags, and the supported Stellaris version. Both descriptors must declare identical values.

### Settings

The subsequent settings are recognised:

| Section | Key | Type | Default | Required | Description |
|---------|-----|------|---------|----------|-------------|
| Mod descriptor | `version` | String | `"2.0.2"` | Yes | Mod version reported by the launcher; release workflows replace it with the release tag |
| Mod descriptor | `name` | String | `"Universum Infinitum - Species Traits"` | Yes | Display name used by the launcher and distribution platforms |
| Mod descriptor | `path` | String | `"mod/ui-species-traits"` | Yes | Installation path relative to the Stellaris user directory |
| Mod descriptor | `tags` | List | `Species` | Yes | Mod category used by the launcher |
| Mod descriptor | `supported_version` | String | `"v4.5.*"` | Yes | Stellaris version range in which the mod is enabled |

## 🌍 Localisation

The mod provides English names and descriptions for every trait it defines.

| Language | Locale | Status |
|----------|--------|--------|
| English | `en` | Complete; 58 trait names and 58 trait descriptions |

## 🧩 Compatibility

| Component | Supported Versions | Notes |
|-----------|--------------------|-------|
| Stellaris | `4.5.*` | Declared by both mod descriptors; earlier versions display the mod as incompatible |
| Distribution platforms | Steam Workshop, Nexus Mods, Paradox Mods, GitHub Releases | The mod declares no platform-specific conduct |

## 🛡️ Privacy and Data

Universum Infinitum - Species Traits does not request, collect, transmit, or persist personal data.

For the detailed description of how the application handles privacy and personal data, see the [Privacy Statement](./PRIVACY.md).

The mod contains local game definitions, localisation, and image assets. It has no built-in personal-data transfer; interactions with distribution platforms happen separately from mod execution.

## ⚙️ GitHub Actions

| Workflow | Purpose | What it does |
|----------|---------|--------------|
| [github-release.yml](https://github.com/hmlendea/stellaris-ui-species-traits/blob/master/.github/workflows/github-release.yml) | GitHub Release publication | Writes the release tag into both descriptors, packages the release archive, and attaches it to the published GitHub release |
| [nexus-mods-release.yml](https://github.com/hmlendea/stellaris-ui-species-traits/blob/master/.github/workflows/nexus-mods-release.yml) | Nexus Mods publication | Writes the release tag into both descriptors, packages the release archive, and uploads it to Nexus Mods |
| [steam-workshop-release.yml](https://github.com/hmlendea/stellaris-ui-species-traits/blob/master/.github/workflows/steam-workshop-release.yml) | Steam Workshop publication | Writes the release tag into both descriptors, packages the release archive, and uploads it to the Steam Workshop item |

## 🗂️ Project Structure

The repository contains a single mod package, its icon source asset, release automation, and repository documentation.

### Directories

| Directory | Purpose |
|-----------|---------|
| `ui-species-traits/` | Mod package read by the Stellaris launcher and game engine |
| `ui-species-traits/common/` | Clausewitz trait definitions and species archetype limits |
| `ui-species-traits/localisation/english/` | English localisation for all traits and tiers |
| `ui-species-traits/gfx/interface/icons/traits/` | DDS trait icons |
| `src/` | GIMP source file for the trait artwork |
| `.github/workflows/` | Release automation for GitHub, Nexus Mods, and Steam Workshop |
| `docs/` | Repository documentation, indexed by `docs/INDEX.md` |

## 🏗️ Architecture

Universum Infinitum - Species Traits is a declarative, data-driven content package interpreted by the Stellaris engine; it owns no runtime process.

See the [Architecture Documentation](./ARCHITECTURE.md) for the system context, principal components, runtime flows, ownership boundaries, dependencies, constraints, and extension points.

## 📚 Documentation

Trait details are maintained in the project wiki, and comprehensive architectural and operational documentation is available in the `docs/` directory.

| Resource | Description |
|----------|-------------|
| [Trait Catalogue](https://github.com/hmlendea/stellaris-ui-species-traits/wiki/Traits) | Lists the species traits included by the mod |
| [Documentation Index](./docs/INDEX.md) | Master navigation for all repository documentation |
| [Repository Overview](./docs/repository-overview.md) | Purpose, scope, entry points, and capabilities |
| [Repository Structure](./docs/repository-structure.md) | Source tree layout and module organisation |
| [Architecture (docs)](./docs/architecture.md) | Architectural areas, interfaces, and contracts |
| [Data Model](./docs/data-model.md) | Trait schema, scaling policy, and archetype variables |
| [Components](./docs/components/) | Trait definitions, archetype configuration, localisation, and assets |
| [Flows](./docs/flows/) | Mod load, species creation, and trait selection |
| [Behaviour](./docs/behaviour/) | Trait effects and tier scaling policy |
| [Operational](./docs/operational/) | Build and deployment, configuration, dependencies, and testing |

## 🤝 Contributing

You are welcome to submit any suggestion, feedback, or modification to this project.

When doing so, please:
- Maintain cross-platform compatibility
- Preserve the existing public contract unless a breaking change is intentional
- Submit focused pull requests that conform to the existing code style
- Maintain your branch synchronised with `master`
- Raise a new [issue](https://github.com/hmlendea/stellaris-ui-species-traits/issues) for problems or suggestions

See the [Contributing Guidelines](./CONTRIBUTING.md) for details on how to report issues, suggest enhancements, and submit changes.

## 📜 Code of Conduct

See the [Code of Conduct](./CODE_OF_CONDUCT.md) for the standards of conduct expected in all project spaces.

## 🔗 Related Projects

- [Universum Infinitum - Character Traits](https://github.com/hmlendea/stellaris-ui-character-traits): Adds character traits
- [Universum Infinitum - Deposit Blockers](https://github.com/hmlendea/stellaris-ui-deposit-blockers): Adds planetary deposit blockers
- [Universum Infinitum - Flags](https://github.com/hmlendea/stellaris-ui-flags): Adds empire flags
- [Universum Infinitum - Galaxy Sizes](https://github.com/hmlendea/stellaris-ui-galaxy-sizes): Adds galaxy size options
- [Universum Infinitum - Name Lists](https://github.com/hmlendea/stellaris-ui-name-lists): Adds name lists
- [Universum Infinitum - Planet Modifiers](https://github.com/hmlendea/stellaris-ui-planet-modifiers): Adds planet modifiers
- [Universum Infinitum - Prescripted Countries](https://github.com/hmlendea/stellaris-ui-prescripted-countries): Adds prescripted empires
- [Universum Infinitum - Species Names](https://github.com/hmlendea/stellaris-ui-species-names): Adds species names
- [Universum Infinitum - Star Names](https://github.com/hmlendea/stellaris-ui-star-names): Adds star names

## 🔒 Security

Universum Infinitum - Species Traits publishes only static game definitions, localisation, and image assets, so security concerns centre on the integrity of the distributed archives.

For information on reporting security vulnerabilities, see the [Security Policy](./SECURITY.md).

## 💬 Support

For support, please [open an issue](https://github.com/hmlendea/stellaris-ui-species-traits/issues).

## 💝 Project Engagement

Discovered a problem with Universum Infinitum - Species Traits or have a suggestion for it? [Open an issue](https://github.com/hmlendea/stellaris-ui-species-traits/issues)!

If you find this project useful, consider [funding it](https://hmlendea.go.ro/funding) or starring ⭐️ it on GitHub, or giving it a thumbs up and adding it as a favourite on the [Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=2047811098)!

[![Donate](https://raw.githubusercontent.com/hmlendea/readme-assets/master/donate_generic.png)](https://hmlendea.go.ro/funding)

## 📄 License

This project is being distributed under the `GNU General Public License version 3` or later.
See [LICENSE](./LICENSE) for additional information.
