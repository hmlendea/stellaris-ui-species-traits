# Component: Archetype configuration

This document details the species archetype configuration, variable system, and trait limit enforcement.

## File

**Path:** `ui-species-traits/common/species_archetypes/00_species_archetypes.txt`

## Variable definitions

The file defines variables at the top for maintainability:

```clausewitz
@species_trait_points = 2
@species_max_traits = 7
@robot_trait_points = 0
@robot_max_traits = 6
@machine_trait_points = 1
@machine_max_traits = 7
```

These variables are referenced by archetype blocks below.

## Archetype blocks

### BIOLOGICAL

```clausewitz
species_archetype = {
    name = "BIOLOGICAL"
    trait_points = @species_trait_points
    max_traits = @species_max_traits
}
```

- **Trait points:** 2
- **Max traits:** 7
- **Inherits from:** None (base archetype)
- **Applies to:** Standard biological species, including Lithoid (via inheritance)

### LITHOID

```clausewitz
species_archetype = {
    name = "LITHOID"
    inherit_trait_points_from = "BIOLOGICAL"
    trait_points = @species_trait_points
    max_traits = @species_max_traits
}
```

- **Trait points:** 2 (inherited from BIOLOGICAL)
- **Max traits:** 7 (inherited from BIOLOGICAL)
- **Inherits from:** BIOLOGICAL via `inherit_trait_points_from`
- **Applies to:** Lithoid species (rock-based life)

### MACHINE

```clausewitz
species_archetype = {
    name = "MACHINE"
    trait_points = @machine_trait_points
    max_traits = @machine_max_traits
}
```

- **Trait points:** 1
- **Max traits:** 7
- **Inherits from:** None
- **Applies to:** Machine empires (synthetic)

### ROBOT

```clausewitz
species_archetype = {
    name = "ROBOT"
    trait_points = @robot_trait_points
    max_traits = @robot_max_traits
}
```

- **Trait points:** 0
- **Max traits:** 6
- **Inherits from:** None
- **Applies to:** Robot pops (constructed, not grown)

### HOVERING

```clausewitz
species_archetype = {
    name = "HOVERING"
    inherit_trait_points_from = "MACHINE"
    trait_points = @machine_trait_points
    max_traits = @machine_max_traits
}
```

- **Trait points:** 1 (inherited from MACHINE)
- **Max traits:** 7 (inherited from MACHINE)
- **Inherits from:** MACHINE via `inherit_trait_points_from`
- **Applies to:** Hovering machine variants

### PSIONIC

```clausewitz
species_archetype = {
    name = "PSIONIC"
    inherit_trait_points_from = "BIOLOGICAL"
    trait_points = @species_trait_points
    max_traits = @species_max_traits
}
```

- **Trait points:** 2 (inherited from BIOLOGICAL)
- **Max traits:** 7 (inherited from BIOLOGICAL)
- **Inherits from:** BIOLOGICAL via `inherit_trait_points_from`
- **Applies to:** Psionically ascended species

### PRESAPIENT

```clausewitz
species_archetype = {
    name = "PRESAPIENT"
    uses_modifiers = no
}
```

- **Trait points:** N/A
- **Max traits:** N/A
- **Modifiers:** Disabled via `uses_modifiers = no`
- **Applies to:** Pre-sapient species (animals, primitives)

### OTHER

```clausewitz
species_archetype = {
    name = "OTHER"
    uses_modifiers = no
}
```

- **Trait points:** N/A
- **Max traits:** N/A
- **Modifiers:** Disabled via `uses_modifiers = no`
- **Applies to:** Fallback/other species types

## Summary table

| Archetype | Trait points | Max traits | Inherits from | Modifiers |
|-----------|--------------|------------|---------------|-----------|
| BIOLOGICAL | 2 | 7 | — | Yes |
| LITHOID | 2 | 7 | BIOLOGICAL | Yes |
| MACHINE | 1 | 7 | — | Yes |
| ROBOT | 0 | 6 | — | Yes |
| HOVERING | 1 | 7 | MACHINE | Yes |
| PSIONIC | 2 | 7 | BIOLOGICAL | Yes |
| PRESAPIENT | N/A | N/A | — | No |
| OTHER | N/A | N/A | — | No |

## Variable modification procedure

To change archetype limits globally:

1. Open `ui-species-traits/common/species_archetypes/00_species_archetypes.txt`
2. Modify the variable values at the top:
   ```clausewitz
   @species_trait_points = <new_value>
   @species_max_traits = <new_value>
   @robot_trait_points = <new_value>
   @robot_max_traits = <new_value>
   @machine_trait_points = <new_value>
   @machine_max_traits = <new_value>
   ```
3. All archetypes referencing those variables update automatically.
4. Test in-game: create species of each archetype and verify trait point pool and max trait count.

## Trait selection enforcement

The Stellaris engine enforces limits at species creation time:

1. Player selects archetype (implicitly by species portrait/type)
2. Engine reads `trait_points` and `max_traits` for that archetype
3. Trait picker UI shows available traits filtered by `allowed_archetypes` in trait definitions
4. Player cannot exceed `max_traits` count
5. Player cannot exceed `trait_points` budget (sum of trait `cost` values)
6. Negative traits (negative cost) increase available points

## Interaction with trait definitions

| Trait definition field | Archetype interaction |
|------------------------|----------------------|
| `allowed_archetypes` | Filters which traits appear for each archetype |
| `cost` | Consumes/grants trait points from archetype's `trait_points` pool |
| `category` | UI grouping only; no archetype restriction |

Traits with `allowed_archetypes = { BIOLOGICAL LITHOID }` do not appear for MACHINE/ROBOT archetypes.

## Vanilla compatibility

The mod increases vanilla defaults:
- Vanilla BIOLOGICAL: 2 trait points, 5 max traits → Mod: 2 trait points, 7 max traits
- Vanilla MACHINE: 1 trait point, 5 max traits → Mod: 1 trait point, 7 max traits
- Vanilla ROBOT: 0 trait points, 5 max traits → Mod: 0 trait points, 6 max traits

This is intentional to accommodate the larger trait pool (58 custom traits).

## Testing verification

Manual verification steps:

1. Start Stellaris with mod enabled
2. Create new empire → Biological species
   - Verify trait point pool shows 2
   - Verify max 7 traits selectable
   - Verify all custom traits with `allowed_archetypes = { BIOLOGICAL }` appear
3. Create new empire → Lithoid species
   - Verify trait point pool shows 2
   - Verify max 7 traits selectable
4. Create new empire → Machine empire
   - Verify trait point pool shows 1
   - Verify max 7 traits selectable
   - Verify only traits with `allowed_archetypes` including `MACHINE` appear
5. Create new empire → Robot species (via robot assembly)
   - Verify trait point pool shows 0
   - Verify max 6 traits selectable
6. Check `error.log` for archetype parsing errors

## Related documentation

| Document | Scope |
|----------|-------|
| [Data model](./data-model.md) | Variable definitions, archetype table |
| [Architecture](./architecture.md) | Archetype area, interfaces |
| [Components/trait-definitions.md](./trait-definitions.md) | Trait `allowed_archetypes` field |
| Root [ARCHITECTURE.md](../ARCHITECTURE.md) | High-level archetype configuration |