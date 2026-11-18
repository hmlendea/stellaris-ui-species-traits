# Configuration

This document describes the mod descriptor configuration, the supported version declaration, and the shared archetype variables.

## Descriptor files

| File | Location | Role |
|------|----------|------|
| `ui-species-traits.mod` | Repository root | Descriptor read by the launcher |
| `ui-species-traits/descriptor.mod` | Mod directory root | Descriptor read by the engine |

### Root descriptor

```clausewitz
version="2.0.2"
name="Universum Infinitum - Species Traits"
path="mod/ui-species-traits"
tags={
    "Species"
}
supported_version="v4.5.*"
```

### Mod directory descriptor

The mod directory descriptor declares the same five fields with the same values.

### Field reference

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `version` | string | Yes | Release version, rewritten from the release tag by the release workflows |
| `name` | string | Yes | Display name in the launcher and on distribution platforms |
| `path` | string | Yes | Location of the mod directory relative to the Stellaris user directory |
| `tags` | list | Yes | Launcher categories; the repository declares the single tag `Species` |
| `supported_version` | string | Yes | Stellaris branch in which the mod is enabled |

### Consistency rule

Both descriptors must declare identical values. The release workflows rewrite `version` in both; a divergence leaves the launcher and the engine reporting different versions for the same package.

## Supported version

Both descriptors declare:

```clausewitz
supported_version="v4.5.*"
```

The `*` wildcard accepts patch releases within the 4.5 branch.

| Game version | Launcher conduct |
|--------------|------------------|
| `4.5.*` | Mod enabled |
| Earlier branches | Mod shown as incompatible |
| Later branches | Mod shown as incompatible until `supported_version` is raised |

### Raising the supported version

1. Change `supported_version` in both descriptors to the new branch, for example `v4.6.*`.
2. Revalidate every vanilla identifier referenced in `opposites`.
3. Verify that every modifier key still exists in the new engine version.
4. Verify the mod in game.
5. Tag and release, then update the compatibility sections of the root documents and this documentation set.

## Archetype variables

The mod's only other configurable values are the Clausewitz variables in `common/species_archetypes/00_species_archetypes.txt`. They are archetype definitions consumed by the engine rather than runtime settings.

```clausewitz
@robot_trait_points = 0
@robot_max_traits = 6
@machine_trait_points = 1
@machine_max_traits = 7
@species_trait_points = 2
@species_max_traits = 7
```

### Modification procedure

1. Open `ui-species-traits/common/species_archetypes/00_species_archetypes.txt`.
2. Change a variable value.
3. Every archetype referencing that variable follows; `LITHOID` follows through its inheritance.
4. Verify the effective limits in species creation.

## Trait-level configuration

Trait values are not settings; they are content. Each entry's `cost`, `opposites`, `allowed_archetypes`, and `modifier` block define its behaviour, as documented in [components/trait-definitions.md](../components/trait-definitions.md).

## Localisation configuration

| Property | Requirement |
|----------|-------------|
| Encoding | UTF-8 |
| Root key | `l_english` |
| Byte-order mark | present in the additional-tiers file, absent in the trait-definitions file |

Preserve each file's existing byte-order-mark state when editing.

## Icon configuration

Icons carry no configuration. The engine resolves them from the file name, and no trait definition declares an `icon` field; see [components/assets.md](../components/assets.md).

## Pre-release checklist

- [ ] Both descriptors declare identical `version`, `name`, `path`, `tags`, and `supported_version`.
- [ ] `supported_version` matches the targeted game branch.
- [ ] `path` points to an existing mod directory.
- [ ] No trailing whitespace or stray characters in either descriptor.
- [ ] Every trait identifier has a name key, a description key, and an icon.

## Related documentation

| Document | Scope |
|----------|-------|
| [Build and deployment](./build-and-deployment.md) | Release workflows, packaging |
| [Dependencies](./dependencies.md) | Engine version coupling |
| [Architecture](../architecture.md) | Descriptor area |