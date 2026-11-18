[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/funding)

# Contributing to Universum Infinitum - Species Traits

This document covers the guidelines and processes for contributing to this project, including how to report issues, suggest enhancements, submit code changes, and follow project standards.

## 📑 Table of Contents

- [Contributing](#-contributing)
  - [Reporting Issues](#reporting-issues)
  - [Suggesting Enhancements](#suggesting-enhancements)
  - [Code Contributions](#code-contributions)
    - [Prerequisites](#prerequisites)
    - [Development Setup](#development-setup)
    - [Making Changes](#making-changes)
    - [Pull Request Guidelines](#pull-request-guidelines)
  - [Code Style](#code-style)
  - [Documentation](#documentation)
- [Code of Conduct](#-code-of-conduct)
- [Security](#-security)
- [Donations](#-donations)
- [License](#-license)

## 🤝 Contributing

### Reporting Issues

- Search existing issues first.
- Use the issue templates if available.
- Provide clear reproduction steps.
- Include environment details.

### Suggesting Enhancements

- Check the existing discussions.
- Explain the use case and expected conduct.
- Consider the implementation complexity.

### Code Contributions

#### Prerequisites

- Stellaris `4.5.*` installed for local verification
- A text editor with support for Clausewitz script files and YAML
- An image editor capable of exporting DDS icons, such as GIMP

#### Development Setup

```bash
# Clone the repository
git clone https://github.com/hmlendea/stellaris-ui-species-traits.git
cd stellaris-ui-species-traits

# No dependencies to install: the mod consists of static game definitions, localisation, and image assets
```

#### Making Changes

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Make your changes to the trait definitions, localisation, or icons.
4. Verify the mod by installing it locally and inspecting species creation in Stellaris.
5. Commit with clear and descriptive messages.
6. Push to your fork.
7. Open a Pull Request.

#### Pull Request Guidelines

- Target the `master` branch.
- Keep PRs focused and atomic.
- Revise documentation if applicable.
- Include tests for any new functionality.
- Ensure the CI checks pass.
- Ensure the mod loads without errors in Stellaris.

### Code Style

Follow the project's coding standards:
- Use tabs for indentation in Clausewitz script files
- Follow the existing naming scheme for trait identifiers and localisation keys
- Keep trait entries ordered as they are grouped in the existing files
- Use British spelling in localisation text, such as `behaviour` and `localisation`

### Documentation

- Revise relevant docs for changes.
- Follow the documentation style guide.
- Preview changes locally if possible.
- For structural or behavioural changes, update the corresponding documents in the [`docs/`](./docs/INDEX.md) directory, including architecture, data model, components, flows, behaviour, and operational topics.

## 📜 Code of Conduct

This project follows the [Code of Conduct](./CODE_OF_CONDUCT.md).

## 🔒 Security

Report security vulnerabilities per the [Security Policy](./SECURITY.md).

## 💝 Donations

This project is developed and maintained during spare time. If you find it useful, consider [donating](https://hmlendea.go.ro/funding) and giving it a star ⭐️ on GitHub!

## 📄 License

By contributing, you agree that your contributions will be licensed under the GNU General Public License version 3 or later.