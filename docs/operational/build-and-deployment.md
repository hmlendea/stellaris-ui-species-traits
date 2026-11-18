# Build and deployment

This document describes the release process, packaging, and distribution workflows for the Universum Infinitum - Species Traits mod.

## Release model

Releases are **tag-driven** and run through GitHub Actions. No build step exists, because the mod is a content package.

### Versioning

- Tags follow `v<major>.<minor>.<patch>`.
- The release workflows strip the leading `v` and write the remainder into `version` in both descriptors.
- Both descriptors currently declare `version="2.0.2"` and `supported_version="v4.5.*"`.

## Packaging

### Archive layout

```
ui-species-traits_<version>.zip
├── ui-species-traits/
│   ├── descriptor.mod
│   ├── common/
│   │   ├── species_archetypes/00_species_archetypes.txt
│   │   └── traits/
│   │       ├── ui-traits_species.txt
│   │       └── ui-traits_species_newtiers.txt
│   ├── gfx/interface/icons/traits/*.dds
│   └── localisation/english/*.yml
└── ui-species-traits.mod
```

### Manual packaging

```bash
VERSION="2.0.2"
zip -r "ui-species-traits_${VERSION}.zip" "ui-species-traits/" "ui-species-traits.mod"
```

## Workflows

### GitHub Release (`.github/workflows/github-release.yml`)

**Trigger:** publication of a GitHub release.

**Steps:**
1. Check out the repository.
2. Read the release tag and strip the leading `v`.
3. Write the version into `ui-species-traits.mod` and `ui-species-traits/descriptor.mod`.
4. Build the release archive.
5. Attach the archive to the published release.

**Permissions:** `contents: write`.

### Nexus Mods (`.github/workflows/nexus-mods-release.yml`)

**Trigger:** push of a `v*.*.*` tag, or a manual dispatch with a tag input.

**Steps:**
1. Check out the requested tag.
2. Write the version into both descriptors.
3. Build the release archive.
4. Upload the archive to Nexus Mods.

**Secrets required:** `NEXUSMODS_API_KEY`.

### Steam Workshop (`.github/workflows/steam-workshop-release.yml`)

**Trigger:** push of a `v*.*.*` tag, or a manual dispatch with a tag input.

**Steps:**
1. Check out the requested tag.
2. Write the version into both descriptors.
3. Build the release archive.
4. Upload the archive to the Steam Workshop item.

**Secrets required:** `STEAM_USERNAME`, `STEAM_PASSWORD`, and `STEAM_2FA_SEED`.

Do not print, commit, or transmit these values. Configure them as repository secrets only.

## Release procedure

### 1. Prepare

```bash
git status
```

Confirm that the descriptors, the localisation, and the icons are consistent, and verify the mod in game before tagging.

### 2. Tag and publish

```bash
git tag -a v2.0.3 -m "Release v2.0.3"
git push origin v2.0.3
gh release create v2.0.3 --title "v2.0.3" --notes "Release notes"
```

Pushing the tag starts the Nexus Mods and Steam Workshop workflows; publishing the release starts the GitHub Release workflow.

### 3. Verify

- Confirm that all three workflow runs completed.
- Confirm that the GitHub release carries the archive asset.
- Confirm that the Nexus Mods and Steam Workshop pages show the new version.
- Install the released archive and confirm that the mod loads and that the traits appear.

## Paradox Mods

Paradox Mods receives no automated publication. Upload the released archive manually and set the version, changelog, and supported game version on the platform.

## Rollback

If a release proves faulty:

1. Publish a corrected release rather than deleting a tag, so that the platform versions stay monotonic.
2. Republish the previous version through `workflow_dispatch` with the earlier tag for Steam Workshop and Nexus Mods.
3. Replace the archive on Paradox Mods manually.

Deleting a tag requires explicit maintainer approval, because it breaks the provenance of an already published archive.

## Compatibility

| Mod version series | Stellaris version | Notes |
|--------------------|-------------------|-------|
| 2.x | `4.5.*` | Current declaration in both descriptors |
| 1.x | 3.2 and earlier | Superseded |

A new Stellaris branch requires a descriptor update and revalidation of every vanilla identifier referenced in `opposites`.

## Troubleshooting

| Symptom | Probable cause | Resolution |
|---------|----------------|------------|
| Steam upload fails | Missing or invalid credentials | Verify the three Steam secrets |
| Nexus upload fails | Missing or invalid API key | Verify `NEXUSMODS_API_KEY` |
| Release asset missing | The archive step failed before the upload | Inspect the workflow log |
| Mod not listed | `supported_version` mismatch | Update the descriptors for the current branch |
| Icons missing in game | DDS files absent from the archive | Verify that `gfx/interface/icons/traits/` is packaged |
| Traits show raw identifiers | Localisation files absent or malformed | Verify that both `.yml` files are packaged and parse |

## Related documentation

| Document | Scope |
|----------|-------|
| [Configuration](./configuration.md) | Descriptor fields and values |
| [Dependencies](./dependencies.md) | Engine version coupling |
| [Architecture](../architecture.md) | Deployment model |