# Testing

This document describes the verification strategy and the manual test procedures for the Universum Infinitum - Species Traits mod.

## Approach

The mod is a content package with no executable code, so there is no unit test suite and no continuous integration check for content. Verification is manual and performed in game.

### Why no automation

- The Clausewitz content is declarative and parsed by the engine at load time.
- There is no runtime logic to exercise in isolation.
- Validating identifiers, localisation keys, and icons requires the engine's content context.
- Species creation in Stellaris serves as the integration surface.

## Test categories

### 1. Load verification

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Start Stellaris with the mod active | No error-log entries mentioning `ui_trait_` or `ui-species-traits` |
| 2 | Open species creation | All 58 trait definitions are offered for `BIOLOGICAL` species |
| 3 | Confirm the limits | `BIOLOGICAL`: 2 points, 7 traits; `MACHINE`: 1 point, 7 traits; `ROBOT`: 0 points, 6 traits |

### 2. Trait verification

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Create a biological species | Every trait appears with its localised name |
| 2 | Hover each trait | The description renders with its `§L...§!` styling |
| 3 | Select `ui_trait_very_agile` | Cost displays 6 and the icon matches the identifier |
| 4 | Inspect effective modifiers | The engine reports the values declared in the trait file |
| 5 | Select `ui_trait_agile`, then `ui_trait_sluggish` | The second selection is refused |
| 6 | Create a machine empire | No mod trait is offered |

### 3. Tier verification

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Inspect `ui_trait_very_agile` | Cost 6, effect 1.833× the base tier |
| 2 | Inspect `ui_trait_extremely_agile` | Cost 8, effect 2.2× the base tier |
| 3 | Compare Very and Extremely | Extremely is 20 per cent stronger than Very |
| 4 | Inspect a Lifespan tier | The modifier is additive years, not a multiplier |
| 5 | Confirm cost progression | 2 → 6 → 8 for a two-point family; 1 → 3 → 4 for a one-point family |

### 4. Archetype verification

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Create a biological species | Two trait points and seven trait slots |
| 2 | Select seven traits | An eighth is refused |
| 3 | Create a lithoid species | The same limits, inherited from `BIOLOGICAL` |
| 4 | Create a machine empire | One trait point and seven trait slots, with no mod traits |
| 5 | Create a robot species | Zero trait points and six trait slots |

### 5. Localisation verification

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Inspect trait names | 58 names render, none as raw identifiers |
| 2 | Inspect descriptions | 58 descriptions render with their styling intact |
| 3 | Search for a raw identifier in the interface | None appears |
| 4 | Reopen the files after editing | No encoding artefacts appear |

### 6. Icon verification

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Open the trait selector | Every trait displays its icon |
| 2 | Inspect tier icons | Very and Extremely tiers differ from their base tier |
| 3 | Search for placeholders | No placeholder image appears |
| 4 | Open a few files in an image viewer | They decode as valid DDS images |

### 7. Save compatibility verification

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Load a save that uses mod traits | The traits are intact |
| 2 | Change a modifier value and reload | The save reflects the new value |
| 3 | Disable the mod and load | The traits are lost |
| 4 | Re-enable the mod and load | The traits return |

### 8. AI verification

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Generate an AI empire | The AI selects within its budget |
| 2 | Inspect the AI selection | No Extremely positive tier, which is unaffordable |
| 3 | Inspect for conflicts | No opposed pair is selected together |

### 9. Edge cases

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Create a species with no traits | Accepted |
| 2 | Create a species mixing positive and negative traits | Accepted while the point total stays non-negative |
| 3 | Modify a species genetically | Modifiers update from the new trait list |
| 4 | Grant a trait by script | Applied, with conflicting traits removed |
| 5 | Rename an icon file and restart | The trait still works and displays a placeholder |

## Error-log verification

```
Stellaris user directory/logs/error.log
```

Search for `ui_trait` and `ui-species-traits`. A clean load produces no matching entries.

| Pattern | Meaning | Resolution |
|---------|---------|------------|
| `unknown trait id` | Missing or mismatched identifier | Align the identifier across the trait file, the localisation, and the icon |
| `missing icon` | Icon file absent or misnamed | Add or rename the DDS file |
| `modifier not found` | Invalid modifier key | Use a key the targeted engine version supplies |
| parse error | Syntax error in a `.txt` file | Review the block syntax |

## Release checklist

- [ ] Every category above passes.
- [ ] The error log contains no mod-related entries.
- [ ] Both descriptors declare the same `version` and `supported_version`.
- [ ] Every trait identifier has a name, a description, and an icon.
- [ ] The release archive contains the mod directory and the root descriptor.
- [ ] The published archive installs and loads.

## Known limitations

| Limitation | Impact | Mitigation |
|------------|--------|------------|
| No automated tests | Every change needs manual verification | Follow the categories above |
| No content validation in continuous integration | Identifier and icon drift is possible | Check the three-way identifier alignment |
| Single language | No translated verification | English-only scope |
| No regression suite | An older behaviour can regress unnoticed | Compare against an earlier release tag |
| Scaling policy not enforced | Tier values can drift | Review a family when changing any of its tiers |

## Related documentation

| Document | Scope |
|----------|-------|
| [Architecture](../architecture.md) | Design constraints, verification requirements |
| [Components/trait-definitions.md](../components/trait-definitions.md) | Trait fields to verify |
| [Components/archetype-config.md](../components/archetype-config.md) | Archetype limits to verify |
| [Build and deployment](./build-and-deployment.md) | Release procedure |