# Testing

This document describes the testing strategy, verification procedures, and manual testing checklist for the Universum Infinitum - Species Traits mod.

## Testing philosophy

The mod is a **pure data package** with no executable code. There is no automated test suite, CI pipeline, or unit tests. All verification is **manual** and performed by the developer/maintainer in-game.

### Why no automated tests?

- Clausewitz script is declarative; engine parses at load time
- No runtime logic to unit-test
- Data validation (trait IDs, localisation keys, icon files) requires game context
- Stellaris itself provides integration testing via species creation UI

## Test categories

### 1. Load test

Verify the mod loads without errors.

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Start Stellaris with mod enabled | No `error.log` entries mentioning `ui_trait_` or `ui-species-traits` |
| 2 | Open species creation | All 58 traits visible in picker |
| 3 | Check trait count | Exactly 58 traits listed |
| 4 | Check archetype limits | BIOLOGICAL: 7 max, 2 points; MACHINE: 7 max, 1 point; ROBOT: 6 max, 0 points |

### 2. Trait verification test

Verify each trait's properties.

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Create Biological species | All custom traits appear with correct names |
| 2 | Hover each trait | Correct description shown with `§L...§!` formatting |
| 3 | Select `ui_trait_very_strong` | Cost shows 6, icon shows correctly |
| 4 | Verify modifiers | Console: `species_modifiers <id>` shows expected values |
| 5 | Verify opposites | Selecting `ui_trait_agile` disables `ui_trait_sluggish`, etc. |
| 6 | Verify archetype filtering | Machine species does not show physical/social traits |

### 3. Tier scaling test

Verify Very and Extremely tiers scale correctly.

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Select `ui_trait_very_strong` | Cost = 6, modifiers = 1.5× base |
| 2 | Select `ui_trait_extremely_strong` | Cost = 8, modifiers = 2.0× base |
| 3 | Verify Extremely > Very | Effect values confirm +20% over Very |
| 4 | Verify Very > Base | Effect values confirm 1.8–3.0× base |
| 5 | Verify cost progression | Base(2) → Very(6) → Extremely(8): 3× then 4× |

### 4. Archetype limit test

Verify archetype trait point and max trait limits.

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Create Biological species | Trait point pool = 2 |
| 2 | Select 7 traits | 8th rejected with "max traits" message |
| 3 | Create Machine species | Trait point pool = 1 |
| 4 | Create Robot species | Trait point pool = 0, max 6 traits |
| 5 | Verify HOVERING inherits from MACHINE | Same as MACHINE |
| 6 | Verify PSIONIC inherits from BIOLOGICAL | Same as BIOLOGICAL |

### 5. Localisation test

Verify all trait names and descriptions display correctly.

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Create Biological species | All 38 custom trait names shown (not raw IDs) |
| 2 | Create Biological species | All 20 tier trait names shown |
| 3 | Hover trait | Description with `§L...§!` formatting renders correctly |
| 4 | Verify no raw IDs | No `ui_trait_agile` displayed instead of "Agile" |
| 5 | Verify BOM encoding | File opens correctly in Stellaris (no encoding errors) |

### 6. Icon test

Verify all icons load correctly.

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Create Biological species | All 58 icons render in trait picker |
| 2 | Verify tier icons | Very/Extremely icons differ from base icons |
| 3 | Verify no placeholders | No generic question mark icons |
| 4 | Verify DDS format | Icons open in GIMP without corruption |

### 7. Save game compatibility test

Verify existing save games work with mod updates.

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Load existing save with mod v1.x | Species traits intact |
| 2 | Update mod to v1.y | Traits still functional |
| 3 | Disable mod, load save | Traits show as missing, species weakened |
| 4 | Re-enable mod, load save | Traits restored |

### 8. AI behaviour test

Verify AI picks reasonable traits.

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Generate AI empire | AI selects traits within budget |
| 2 | Verify AI doesn't pick Extremely | Extremely too expensive for AI |
| 3 | Verify AI picks focused traits | Militarist AI picks strong traits |
| 4 | Verify AI respects opposites | No conflicting traits selected |

### 9. Edge case test

| Step | Action | Expected result |
|------|--------|-----------------|
| 1 | Create species with 0 traits | Valid (no traits = default species) |
| 2 | Create species with all negatives | Valid (exceeds budget but allowed) |
| 3 | Gene modify to add trait | Modifiers update correctly |
| 4 | Event grants trait | Trait added, opposites removed |
| 5 | Robot assembly | Only machine traits available |

## Error log verification

After any test session, check:

```bash
# Path to error log
~/Documents/Paradox Interactive/Stellaris/logs/error.log

# Search for mod-related errors
grep -i "ui_trait\|ui-species-traits\|ui_trait_" error.log

# Expected: no output (clean load)
# If errors found: investigate and fix before release
```

Common error patterns:

| Pattern | Meaning | Fix |
|---------|---------|-----|
| `unknown trait id: ui_trait_xxx` | Missing localisation key | Add key to `.yml` |
| `missing icon for trait: ui_trait_xxx` | Missing DDS file | Add `.dds` file |
| `archetype not found` | Invalid archetype name | Check `allowed_archetypes` |
| `modifier not found` | Invalid modifier key | Check modifier syntax |
| `parse error` | Syntax error in `.txt` | Review Clausewitz script syntax |

## Release testing checklist

Before tagging a release:

- [ ] All 9 test categories pass
- [ ] `error.log` clean (no mod-related errors)
- [ ] Both descriptors have correct `supported_version`
- [ ] README version badge matches release version
- [ ] Package ZIP contains all files (verify structure)
- [ ] Steam Workshop upload succeeds
- [ ] Nexus Mods upload succeeds
- [ ] Paradox Mods manually uploaded (if applicable)
- [ ] Changelog updated (if exists)

## Known limitations

| Limitation | Impact | Mitigation |
|------------|--------|-----------|
| No automated tests | Manual verification required | Documented testing procedures |
| No CI pipeline | No automated validation | Manual release checklist |
| Single language | No non-English testing | English-only localisation |
| No regression suite | New changes untested against old | Version-tagged releases for comparison |
| Scaling policy not enforced | Inconsistent tier values possible | Manual review per family |

## Related documentation

| Document | Scope |
|----------|-------|
| [Architecture](../architecture.md) | Design constraints, testing requirements |
| [Components/trait-definitions.md](../components/trait-definitions.md) | Trait field verification |
| [Components/archetype-config.md](../components/archetype-config.md) | Archetype limit verification |
| [Operational/build-and-deployment.md](./build-and-deployment.md) | Release testing |
| Root [README.md](../README.md) | Installation instructions |