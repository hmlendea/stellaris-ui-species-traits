# Build and deployment

This document describes the release process, packaging, and distribution workflows for the Universum Infinitum - Species Traits mod.

## Release model

The mod uses **tag-based releases** via GitHub Actions. No build step is required — the mod is a pure data package.

### Version scheme

- **Format:** `v<major>.<minor>.<patch>` (e.g., `v1.2.3`)
- **Tags:** Git tags prefixed with `v` trigger release workflows
- **Supported version:** Declared in `descriptor.mod` as `supported_version = "v4.5.*"`

## Packaging

### Package structure

The release asset is a ZIP archive containing:

```
ui-species-traits_<version>.zip
├── ui-species-traits/           # Mod folder (extracted to Stellaris mod directory)
│   ├── descriptor.mod
│   ├── common/
│   │   ├── species_archetypes/
│   │   │   └── 00_species_archetypes.txt
│   │   └── traits/
│   │       ├── ui-traits_species.txt
│   │       └── ui-traits_species_newtiers.txt
│   ├── gfx/
│   │   └── interface/
│   │       └── icons/
│   │           └── traits/
│   │               └── *.dds (58 files)
│   └── localisation/
│       └── english/
│           ├── ui-species-traits_l_english.yml
│           └── ui-species-traits_newtiers_l_english.yml
└── ui-species-traits.mod        # Descriptor at archive root (for launcher)
```

### Package creation (manual)

```bash
# From repository root
cd /path/to/stellaris-ui-species-traits

# Set the version being packaged
VERSION="1.2.3"

# Create ZIP with the same layout as the automated workflow
zip -r "ui-species-traits_${VERSION}.zip" "ui-species-traits/" "ui-species-traits.mod"
```

### Package creation (automated via GitHub Actions)

The release workflows package the checked-out tag themselves:
1. Update `version` in `ui-species-traits.mod` and `ui-species-traits/descriptor.mod`
2. Build `ui-species-traits_<version>.zip` containing `ui-species-traits/` and `ui-species-traits.mod`
3. Publish to GitHub Releases, Steam Workshop, and Nexus Mods

## GitHub Actions workflows

### GitHub Release (`.github/workflows/github-release.yml`)

**Trigger:** Published GitHub release

**Steps:**
1. Check out the released tag
2. Extract the version from the release tag
3. Update the `version` field in both mod descriptors
4. Build the `ui-species-traits_<version>.zip` archive
5. Attach the archive to the GitHub release via `softprops/action-gh-release@v2`

**Required secrets:**
- `GITHUB_TOKEN` (provided automatically)

### Steam Workshop (`.github/workflows/steam-workshop-release.yml`)

**Trigger:** Push tag `v*.*.*` or `workflow_dispatch` with tag input

**Steps:**
1. Check out the requested tag
2. Update the `version` field in both mod descriptors
3. Build and extract the release archive
4. Upload to Steam Workshop via `hmlendea/steam-workshop-update@latest`

**Required secrets:**
- `STEAM_USERNAME`
- `STEAM_PASSWORD`
- `STEAM_2FA_SEED`

**Configuration (in workflow):**
- `appid`: Steam App ID for Stellaris (281990)
- `itemid`: Workshop item ID (2047811098)
- `path`: `release/ui-species-traits/`
- `changenote`: Links to GitHub release

### Nexus Mods (`.github/workflows/nexus-mods-release.yml`)

**Trigger:** Push tag `v*.*.*` or `workflow_dispatch` with tag input

**Steps:**
1. Check out the requested tag
2. Update the `version` field in both mod descriptors
3. Build the release archive
4. Upload to Nexus Mods via `Nexus-Mods/upload-action@v1.0.0-beta.11`

**Required secrets:**
- `NEXUSMODS_API_KEY`

**Configuration (in workflow):**
- `file_id`: Nexus Mods file ID (923479)
- `filename`: `release.zip`
- `display_name`: `ui-species-traits.zip`
- `version`: Tag version
- `description`: Changelog link
- `category`: `main`
- `archive_existing_version`: `true`
- `update_mod_version`: `true`

## Release procedure

### 1. Prepare release

```bash
# Ensure all changes committed
git status

# Update version references if needed
# - descriptor.mod supported_version (if Stellaris version changed)
# - README.md version badge
# - CHANGELOG.md (if exists)

# Run manual verification
# 1. Create package locally
# 2. Install in Stellaris
# 3. Verify all traits work
```

### 2. Create GitHub release

```bash
# Tag the release
git tag -a v1.2.3 -m "Release v1.2.3: <summary>"

# Push tag (triggers the Steam Workshop and Nexus Mods workflows)
git push origin v1.2.3

# Publish the GitHub release (triggers the GitHub Release workflow,
# which builds and attaches ui-species-traits_1.2.3.zip)
gh release create v1.2.3 --title "v1.2.3" --notes "Release notes..."
```

### 3. Verify deployment

- **GitHub Actions:** Check workflow runs for github-release, steam-workshop-release, and nexus-mods-release
- **Steam Workshop:** Verify mod page shows new version
- **Nexus Mods:** Verify mod page shows new version
- **Paradox Mods:** Manual upload required (no workflow)

## Manual distribution

### Paradox Mods

No automated workflow. Manual steps:

1. Go to https://mods.paradoxplaza.com/
2. Log in, navigate to mod page
3. Upload new version ZIP
4. Set version, changelog, supported game version

### Direct download

Users can download ZIP from GitHub Releases page and install manually:

1. Download `ui-species-traits_<version>.zip`
2. Extract to `Documents/Paradox Interactive/Stellaris/mod/`
3. Enable in launcher

## Rollback procedure

If a release has critical issues:

1. **GitHub Releases:** Delete the release and tag
   ```bash
   git tag -d v1.2.3
   git push origin :refs/tags/v1.2.3
   gh release delete v1.2.3
   ```
2. **Steam Workshop:** Re-upload previous version via workflow_dispatch with old tag
3. **Nexus Mods:** Re-upload previous version via workflow_dispatch with old tag
4. **Paradox Mods:** Re-upload previous version manually

## Version compatibility

| Mod version | Stellaris version | Notes |
|-------------|-------------------|-------|
| 1.x.x | 4.4.* / 4.5.* | Current supported range |
| Future | 4.6.* | Update `supported_version` in descriptors |

## Troubleshooting

| Issue | Cause | Resolution |
|-------|-------|------------|
| Steam upload fails | Invalid credentials | Check `STEAM_USERNAME`, `STEAM_PASSWORD`, `STEAM_2FA_SEED` secrets |
| Nexus upload fails | Invalid API key | Check `NEXUSMODS_API_KEY` secret |
| Release asset not found | Asset name mismatch | Ensure the archive label matches the mod directory name `ui-species-traits` |
| Mod not loading | `supported_version` mismatch | Update descriptor to match current Stellaris version |
| Icons missing | DDS files not in package | Verify `gfx/interface/icons/traits/` included in ZIP |

## Related documentation

| Document | Scope |
|----------|-------|
| [Configuration](./configuration.md) | Descriptor variables, supported_version |
| [Dependencies](./dependencies.md) | Stellaris engine version coupling |
| [Architecture](../architecture.md) | Deployment model |
| Root [README.md](../README.md) | Installation instructions |