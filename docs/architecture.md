# Architecture

This document elaborates on the root [ARCHITECTURE.md](../ARCHITECTURE.md) with implementation-level detail for maintainers and contributors. It covers the mod's declarative data-driven plugin architecture, the exact file contracts, and the integration boundaries with the Stellaris engine.

## Architectural style

The mod follows a **declarative data-driven plugin** architecture. All content is expressed as static files interpreted by the Stellaris engine at load time:

- **Clausewitz script** (`.txt`) — trait definitions, archetype configuration
- **YAML localisation** (`.yml`) — display names and descriptions
- **DDS images** (`.dds`) — trait icons

The mod supplies no executable code, scripts, or runtime services. The Stellaris engine is the sole runtime; the mod is a passive data package.

```mermaid
flowchart TB
    Descriptor[descriptor.mod / ui-species-traits.mod] --> Archetypes[common/species_archetypes/]
    Descriptor --> Traits[common/traits/]
    Descriptor --> Localisation[localisation/]
    Descriptor --> Icons[gfx/interface/icons/traits/]
    Archetypes --> Engine[Stellaris Engine]
    Traits --> Engine
    Localisation --> Engine
    Icons --> Engine
```

## Architectural areas

### Descriptor area

**Paths:**
- `ui-species-traits.mod` (repository root)
- `ui-species-traits/descriptor.mod` (mod folder root)

**Responsibilities:**
- Declare mod metadata: `name`, `path`, `tags`, `supported_version`
- Provide the entry point for the launcher and engine

**Boundary rules:**
- Must remain at repository root and mod root respectively
- `supported_version` must match the target Stellaris version branch (currently `v4.5.*` in descriptors, `4.4.*` in README — see [Known Limitations](../README.md#known-limitations))
- Purely declarative; no executable logic

### Archetype area

**Paths:**
- `ui-species-traits/common/species_archetypes/00_species_archetypes.txt`

**Responsibilities:**
- Define trait points and maximum selectable traits for each species archetype
- Increase limits: BIOLOGICAL/MACHINE to 7 traits and 2/1 trait points; ROBOT to 6 traits and 0 trait points

**Boundary rules:**
- Uses variable references (`@species_trait_points`, `@species_max_traits`, etc.) for maintainability
- `LITHOID` inherits from `BIOLOGICAL` via `inherit_trait_points_from`
- `PRESAPIENT` and `OTHER` disable modifiers via `uses_modifiers = no`

### Trait definition area

**Paths:**
- `ui-species-traits/common/traits/ui-traits_species.txt` (38 custom traits)
- `ui-species-traits/common/traits/ui-traits_species_newtiers.txt` (20 vanilla-family tier extensions)

**Responsibilities:**
- Define 58 species traits (positive and negative) covering combat, population, science, economy, habitability, and lifespan
- Extend vanilla trait families with Very and Extremely tiers using a documented scaling policy

**Boundary rules:**
- Trait IDs follow the `ui_trait_*` prefix for custom traits; vanilla-extended tiers use `ui_trait_very_*` and `ui_trait_extremely_*`
- Each trait specifies: `cost`, `category`, `opposites`, `allowed_archetypes`, `tags`, `modifier` block, `slave_cost`, `ai_weight`
- **No `icon` field is present in trait definitions** — icons are loaded by filename convention (`GFX_trait_<id>`)
- `opposites` lists mutually exclusive trait IDs (vanilla and custom)
- `allowed_archetypes` restricts to `BIOLOGICAL`, `LITHOID`, or both
- `modifier` blocks use Stellaris static modifiers (e.g., `planet_jobs_food_produces_mult`, `pop_environment_tolerance`)
- Scaling policy documented in file headers; not enforced by tooling

### Localisation area

**Paths:**
- `ui-species-traits/localisation/english/ui-species-traits_l_english.yml`
- `ui-species-traits/localisation/english/ui-species-traits_newtiers_l_english.yml`

**Responsibilities:**
- Provide English display names (`ui_trait_*:0`) and descriptions (`ui_trait_*_desc:0`) for all traits
- Use `§L...§!` formatting for in-game description styling

**Boundary rules:**
- File encoding: UTF-8 with BOM (Stellaris requirement)
- Keys must exactly match trait IDs from trait definition files
- Only English (`l_english`) is currently provided

### Asset area

**Paths:**
- `ui-species-traits/gfx/interface/icons/traits/*.dds` (58 files)

**Responsibilities:**
- Provide a dedicated icon for each custom trait and tier
- Referenced by the engine via `GFX_trait_<id>` convention (filename-based)

**Boundary rules:**
- Format: DDS (DirectDraw Surface), required by Stellaris
- Naming: `trait_<trait_id>.dds` or `ui_trait_<trait_id>.dds` matching the `GFX_trait_<id>` reference
- No subdirectories; all icons reside directly in `traits/`

## Data architecture

The mod owns no persistent state at runtime. All data is static configuration loaded by the engine at startup.

```mermaid
flowchart LR
    TraitDefs[Trait Definitions\ncommon/traits/*.txt] --> Engine[Stellaris Engine]
    Archetypes[Archetype Config\ncommon/species_archetypes/*.txt] --> Engine
    Loc[Localisation\nlocalisation/english/*.yml] --> Engine
    Icons[Icons\ngfx/interface/icons/traits/*.dds] --> Engine
    Engine --> SpeciesUI[Species Creation UI]
```

| Data or Store | Owner | Representation and Storage | Lifecycle or Consistency |
|---------------|-------|----------------------------|--------------------------|
| Trait definitions | Trait Definition Area | Clausewitz script (`.txt`) in `common/traits/` | Static; loaded once at game start |
| Archetype config | Archetype Area | Clausewitz script (`.txt`) in `common/species_archetypes/` | Static; loaded once at game start |
| Localisation strings | Localisation Area | YAML (`.yml`) in `localisation/english/` | Static; loaded once at game start |
| Trait icons | Asset Area | DDS images in `gfx/interface/icons/traits/` | Static; loaded once at game start |
| Mod descriptors | Descriptor Area | Clausewitz script (`.mod`) at root and mod root | Static; read by launcher and engine |

## Interfaces and integrations

| Interface | Direction | Contract | Owner | Failure Semantics |
|-----------|-----------|----------|-------|-------------------|
| Mod descriptor | Engine → Mod | `descriptor.mod` / `ui-species-traits.mod` with `name`, `path`, `tags`, `supported_version` | Descriptor Area | Mod not listed or version mismatch → mod disabled |
| Species archetypes | Engine → Mod | `common/species_archetypes/*.txt` defining trait points and max traits per archetype | Archetype Area | Parse error → archetype defaults used |
| Trait definitions | Engine → Mod | `common/traits/*.txt` with `cost`, `category`, `opposites`, `allowed_archetypes`, `modifier`, `slave_cost`, `ai_weight` | Trait Definition Area | Parse error → trait omitted; missing icon → placeholder |
| Localisation | Engine → Mod | `localisation/english/*.yml` with `l_english:` keys matching trait IDs | Localisation Area | Missing key → raw ID displayed |
| Trait icons | Engine → Mod | `gfx/interface/icons/traits/*.dds` loaded by `GFX_trait_<id>` convention | Asset Area | Missing file → placeholder icon |

## Dependency direction and rules

- **Descriptor → All Areas:** The mod descriptor is the root; it must exist for the mod to be recognised.
- **Trait Definitions → Localisation:** Every trait ID in `common/traits/` must have a corresponding key in `localisation/english/`.
- **Trait Definitions → Assets:** Every trait ID must have a matching `.dds` file in `gfx/interface/icons/traits/` (by filename convention).
- **Archetype Area → Trait Definitions:** Archetype trait-point and max-trait limits constrain which traits are selectable but do not reference specific trait IDs.
- **No circular dependencies:** All dependencies flow from engine to static mod files; the mod has no internal runtime dependencies.
- **Prohibited coupling:** Trait definitions must not reference other mod files by path; only by trait ID in `opposites` and `allowed_archetypes`.

## External dependencies

| Dependency | Version | Scope | Purpose |
|------------|---------|-------|---------|
| Stellaris Game Engine | `4.4.*` / `4.5.*` | Runtime | Parses all mod files; provides species creation UI |
| Clausewitz Script Parser | Built into Stellaris | Build/Load | Interprets `.txt` and `.mod` files |
| YAML Parser | Built into Stellaris | Build/Load | Interprets `.yml` localisation files |
| DDS Image Loader | Built into Stellaris | Build/Load | Loads `.dds` trait icons |

No third-party libraries, package managers, or build tools are required. The mod is a pure data package.

## Deployment and operations

- **Process topology:** None. The mod is a static archive extracted to the Stellaris mod directory.
- **Deployment model:** Manual installation (extract archive to mod directory) or platform-managed (Steam Workshop, Nexus Mods, Paradox Mods subscribe/install).
- **Persistent state:** None owned by the mod. Stellaris manages playset enablement and save-game trait selections.
- **Scaling assumptions:** Single-player and multiplayer compatible; no server component.
- **Operational consequences:** Mod updates require re-extraction or platform update; save games with custom traits require the mod to remain enabled.

## Compatibility contracts

| Contract | Owner | Invariant | Verification Method | Change Policy |
|----------|-------|-----------|---------------------|---------------|
| `supported_version` | Descriptor Area | Must match target Stellaris version branch | Launcher validation at mod load | Update on new Stellaris major version |
| Trait ID ↔ Localisation key | Trait Definition Area / Localisation Area | Every `ui_trait_*` ID has `ui_trait_*:0` and `ui_trait_*_desc:0` in `.yml` | Manual review; engine shows raw ID if missing | Add localisation when adding traits |
| Trait ID ↔ Icon file | Trait Definition Area / Asset Area | Every `ui_trait_*` ID has matching `.dds` in `gfx/interface/icons/traits/` | Manual review; engine shows placeholder if missing | Add icon when adding traits |
| Trait scaling policy | Trait Definition Area | Very tiers: x11/6 effects, x3 cost; Extremely: +20% effects, cost x4 base | Documented in file headers; not automated | Policy changes require rebalancing all tiers |
| Archetype limits | Archetype Area | BIOLOGICAL/MACHINE: 7 traits, 2/1 points; ROBOT: 6 traits, 0 points | Engine enforces at species creation | Adjust variables in `00_species_archetypes.txt` |

## Testing and verification

- **No automated test suite exists.** Verification is manual:
  1. Install mod locally (extract to Stellaris mod directory).
  2. Enable in launcher.
  3. Start Stellaris and open species creation.
  4. Verify all 58 custom traits appear with correct names, descriptions, icons, costs, and modifiers.
  5. Verify Very/Extremely tiers for vanilla families appear and scale correctly.
  6. Verify archetype trait-point and max-trait limits apply (biological/machine 7 traits, robot 6 traits).
  7. Verify no script errors in `error.log` or console.
- **Material coverage gaps:** No automated regression testing; no CI pipeline for content validation.
- **Prohibitions:** Do not commit untested trait definitions; do not modify vanilla trait IDs in `opposites` without verifying they exist in the target Stellaris version.

## Design constraints

- **Stellaris version coupling:** `supported_version="v4.5.*"` (descriptors) / `4.4.*` (README) ties the mod to the 4.4.x/4.5.x branch. New major versions require descriptor updates and potential trait rebalancing.
- **Clausewitz script limitations:** No variables, loops, or functions in trait definitions; all values are literal.
- **Localisation single-language:** Only English provided; adding languages requires new `localisation/<locale>/` directories and `.yml` files.
- **Icon format lock-in:** DDS is mandatory; no runtime conversion from PNG or other formats.
- **Trait count limits:** Engine enforces `species_max_traits` per archetype; cannot be exceeded by mod configuration alone.
- **No runtime modification:** Traits cannot be added, removed, or modified during gameplay; all changes require game restart.
- **Scaling policy not enforced:** The documented x11/6 and +20% scaling is a design convention; the engine does not validate it.
- **Icon loading convention:** Icons are loaded by filename (`GFX_trait_<id>`), not by explicit `icon =` field in trait definitions. This differs from some vanilla traits and must be maintained for consistency.

## Extension points

- **New custom traits:** Add entries to `ui-traits_species.txt` (or a new `.txt` in `common/traits/`), add localisation keys, add icon `.dds`.
- **New tier extensions for vanilla traits:** Add entries to `ui-traits_species_newtiers.txt` following the documented scaling policy.
- **Additional archetypes:** Extend `00_species_archetypes.txt` with new archetype blocks (rarely needed).
- **New languages:** Create `localisation/<locale>/` with translated `.yml` files.
- **Icon replacements:** Replace `.dds` files in `gfx/interface/icons/traits/` keeping the same filename.

Integration location: All extensions are additive file placements within the existing directory structure; no registration or composition root modification is required.

## Source map

| Area | Repository-Relative Paths |
|------|---------------------------|
| Descriptor | `ui-species-traits.mod`, `ui-species-traits/descriptor.mod` |
| Archetype | `ui-species-traits/common/species_archetypes/00_species_archetypes.txt` |
| Trait Definitions | `ui-species-traits/common/traits/ui-traits_species.txt`, `ui-species-traits/common/traits/ui-traits_species_newtiers.txt` |
| Localisation | `ui-species-traits/localisation/english/ui-species-traits_l_english.yml`, `ui-species-traits/localisation/english/ui-species-traits_newtiers_l_english.yml` |
| Assets | `ui-species-traits/gfx/interface/icons/traits/*.dds` |
| Source Assets | `src/trait.xcf` (GIMP source for icons) |
| Distribution | `.github/workflows/nexus-mods.yml`, `.github/workflows/steam-workshop.yml` |

## Related documentation

| Document | Scope |
|----------|-------|
| [Repository overview](./repository-overview.md) | Purpose, scope, entry points |
| [Repository structure](./repository-structure.md) | Source tree layout, module organisation |
| [Data model](./data-model.md) | Trait definition schema, scaling policy, trait catalogue |
| [Components](./components/) | Per-area deep dives |
| [Flows](./flows/) | End-to-end execution flows |
| Root [ARCHITECTURE.md](../ARCHITECTURE.md) | Architectural boundaries and contracts |
| Root [README.md](../README.md) | Project overview, installation, capabilities |
| Root [CONTRIBUTING.md](../CONTRIBUTING.md) | Contribution guidelines |
| [Trait Catalogue (Wiki)](https://github.com/hmlendea/stellaris-ui-species-traits/wiki/Traits) | Detailed trait list with effects and costs |