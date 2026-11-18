# Dependencies

This document lists the runtime, development, and distribution dependencies of the mod, and explains the version coupling that governs compatibility.

## Runtime dependencies

| Dependency | Version | Scope | Purpose | Provider |
|------------|---------|-------|---------|----------|
| Stellaris game engine | `4.5.*` | Runtime | Parses all mod files and provides the species creation interface | Paradox Interactive |
| Clausewitz script parser | Built into Stellaris | Load | Interprets `.txt` and `.mod` files | Paradox Interactive |
| YAML parser | Built into Stellaris | Load | Interprets `.yml` localisation files | Paradox Interactive |
| DDS image loader | Built into Stellaris | Load | Loads `.dds` trait icons | Paradox Interactive |

## Version coupling

Both descriptors declare:

```clausewitz
supported_version="v4.5.*"
```

The launcher enforces this at activation.

| Game version | Conduct |
|--------------|---------|
| `4.5.*` | Enabled and functional |
| Earlier branches | Shown as incompatible |
| Later branches | Shown as incompatible until the declaration is raised |

### Update triggers

| Trigger | Required action |
|---------|-----------------|
| New Stellaris branch | Raise `supported_version` in both descriptors, revalidate every referenced vanilla identifier, and release |
| Patch within the supported branch | No action; the wildcard covers it |
| A modifier key renamed by the engine | Update the affected trait definitions and release |
| A vanilla trait used in `opposites` renamed or removed | Update the reference and release |

## Content overlap

| Component | Vanilla path | Mod path | Interaction |
|-----------|--------------|----------|-------------|
| Trait definitions | `common/traits/` | `common/traits/ui-traits_*.txt` | Additive; no vanilla file is modified |
| Archetype limits | `common/species_archetypes/` | `common/species_archetypes/00_species_archetypes.txt` | Declares the same archetypes; the load order decides which declaration the engine applies |
| Localisation | `localisation/english/` | `localisation/english/ui-species-traits_*.yml` | Additive; new keys only |
| Icons | `gfx/interface/icons/traits/` | `gfx/interface/icons/traits/` | Additive; new files only |

The archetype file is the only place where the mod could conflict with another mod that also declares the same archetypes, because both files supply the same archetype names.

### Risk

| Risk | Severity | Mitigation |
|------|----------|------------|
| Trait identifier collision with vanilla | Low | Every identifier uses the `ui_trait_` prefix |
| Trait identifier collision with another mod | Low | The prefix is specific to this mod |
| Archetype declaration conflict | Medium | A second mod declaring the same archetypes competes for the same names; verify compatibility prior to combining mods |
| Localisation key collision | Low | Keys follow the identifiers |
| Icon filename collision | Low | Filenames follow the identifiers |

## Game content dependencies

No downloadable content is required or referenced. Every trait is limited to the `BIOLOGICAL` and `LITHOID` archetypes, and no trait, modifier, or identifier in the repository names content from a specific expansion.

## Platform support

| Platform | Supported | Notes |
|----------|-----------|-------|
| Linux | ✅ | Same mod files |
| macOS | ✅ | Same mod files |
| Windows | ✅ | Same mod files |
| Steam Deck | ✅ | Linux build of the game; same mod files |
| Console | ❌ | The console build does not accept mods |

## Distribution dependencies

| Platform | Workflow | Conduct |
|----------|----------|---------|
| Steam Workshop | `.github/workflows/steam-workshop-release.yml` | Automated on a version tag |
| Nexus Mods | `.github/workflows/nexus-mods-release.yml` | Automated on a version tag |
| GitHub Releases | `.github/workflows/github-release.yml` | Automated on release publication |
| Paradox Mods | None | Manual upload |

## Development dependencies

| Tool | Purpose | Required |
|------|---------|----------|
| Text editor | Edit `.txt`, `.yml`, and `.mod` files | Yes |
| Git | Version control | Yes |
| Stellaris | In-game verification | Yes |
| GIMP | Edit `src/trait.xcf` and export DDS icons | Only for icon work |

No compiler, package manager, or build tool is involved.

## File formats

| Format | Used for | Reader |
|--------|----------|--------|
| Clausewitz script (`.txt`, `.mod`) | Trait definitions, archetype limits, descriptors | Engine |
| YAML (`.yml`) | Localisation | Engine |
| DDS (`.dds`) | Trait icons | Engine |
| GIMP (`.xcf`) | Source artwork | GIMP, during development |

## Related documentation

| Document | Scope |
|----------|-------|
| [Configuration](./configuration.md) | Descriptor fields and values |
| [Build and deployment](./build-and-deployment.md) | Distribution workflows |
| [Architecture](../architecture.md) | External dependency table |
| Root [README.md](../../README.md) | Installation, system requirements |