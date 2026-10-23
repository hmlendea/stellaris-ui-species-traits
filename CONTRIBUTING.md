# Contributing to Universum Infinitum - Species Traits

This document covers the guidelines and processes for contributing to this project, including how to report issues, suggest enhancements, submit code changes, and follow project standards.

## 📑 Table of Contents

- [How to Contribute](#-how-to-contribute)
  - [Reporting Issues](#reporting-issues)
  - [Suggesting Enhancements](#suggesting-enhancements)
  - [Code Contributions](#code-contributions)
    - [Prerequisites](#prerequisites)
    - [Development Setup](#development-setup)
    - [Making Changes](#making-changes)
    - [Pull Request Guidelines](#pull-request-guidelines)
- [Code Style](#code-style)
- [Documentation](#documentation)
- [License](#-license)

## 🤝 How to Contribute

### Reporting Issues

- Search existing issues first.
- Use the issue templates if available.
- Provide clear reproduction steps.
- Include environment details.

### Suggesting Enhancements

- Check the roadmap and existing discussions.
- Explain the use case and expected behaviour.
- Consider implementation complexity.

### Code Contributions

#### Prerequisites

- Stellaris `4.4.*` installed for testing
- Basic understanding of Stellaris mod structure (common/, gfx/, localisation/)
- Text editor with YAML and Clausewitz script support

#### Development Setup

```bash
# Clone the repository
git clone https://github.com/hmlendea/stellaris-ui-species-traits.git
cd stellaris-ui-species-traits

# No dependencies to install — the mod consists of static game definitions, localisation, and image assets
```

#### Making Changes

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Make your changes to trait definitions, localisation, or icons.
4. Test by installing the mod locally and verifying in Stellaris.
5. Commit with clear and descriptive messages.
6. Push to your fork.
7. Open a Pull Request.

#### Pull Request Guidelines

- Target the `master` branch.
- Keep PRs focused and atomic.
- Revise documentation if applicable.
- Ensure the mod loads without errors in Stellaris.

### Code Style

Follow the project's coding standards:
- Use consistent indentation (tabs) in Clausewitz script files
- Follow existing naming conventions for trait IDs and localisation keys
- Maintain alphabetical ordering where applicable
- Use British spelling in localisation (e.g. `colour`, `behaviour`, `localisation`)

### Documentation

- Revise relevant docs for changes.
- Follow the documentation style guide.
- Preview changes locally if possible.

##  License

This project is licensed under the [GNU General Public License v3.0](LICENSE).