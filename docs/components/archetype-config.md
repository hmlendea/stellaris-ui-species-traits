# Component: Archetype configuration

This document details the species archetype configuration, the shared variables, and how trait limits are enforced.

## File

**Path:** `ui-species-traits/common/species_archetypes/00_species_archetypes.txt`

## Variable definitions

The file declares six variables before any archetype block:

```clausewitz
@robot_trait_points = 0
@robot_max_traits = 6
@machine_trait_points = 1
@machine_max_traits = 7
@species_trait_points = 2
@species_max_traits = 7
```

## Archetype blocks

The file contains six blocks. Each is a top-level named block rather than a `species_archetype = { name = "..." }` wrapper.

### BIOLOGICAL

```clausewitz
BIOLOGICAL = {
    species_trait_points = @species_trait_points
    species_max_traits = @species_max_traits
}
```

- **Trait points:** 2
- **Maximum traits:** 7
- **Inheritance:** none; this is the base archetype

### ROBOT

```clausewitz
ROBOT = {
    species_trait_points = @robot_trait_points
    species_max_traits = @robot_max_traits
}
```

- **Trait points:** 0
- **Maximum traits:** 6

### MACHINE

```clausewitz
MACHINE = {
    species_trait_points = @machine_trait_points
    species_max_traits = @machine_max_traits
    robotic = yes
}
```

- **Trait points:** 1
- **Maximum traits:** 7
- **Flag:** `robotic = yes`

### PRESAPIENT

```clausewitz
PRESAPIENT = {
    species_trait_points = @species_trait_points
    species_max_traits = @species_max_traits
    uses_modifiers = no
}
```

- **Trait points:** 2
- **Maximum traits:** 7
- **Flag:** `uses_modifiers = no`

### LITHOID

```clausewitz
LITHOID = {
    inherit_trait_points_from = BIOLOGICAL
}
```

- **Trait points:** inherited from `BIOLOGICAL` (2)
- **Maximum traits:** inherited from `BIOLOGICAL` (7)

### OTHER

```clausewitz
OTHER = {
    uses_modifiers = no
}
```

- **Trait points:** not declared
- **Maximum traits:** not declared
- **Flag:** `uses_modifiers = no`

## Summary

| Archetype | Trait points | Maximum traits | Inheritance or flags |
|-----------|--------------|----------------|----------------------|
| `BIOLOGICAL` | 2 | 7 | — |
| `ROBOT` | 0 | 6 | — |
| `MACHINE` | 1 | 7 | `robotic = yes` |
| `PRESAPIENT` | 2 | 7 | `uses_modifiers = no` |
| `LITHOID` | 2 | 7 | inherits from `BIOLOGICAL` |
| `OTHER` | not declared | not declared | `uses_modifiers = no` |

## Variable modification procedure

1. Open `ui-species-traits/common/species_archetypes/00_species_archetypes.txt`.
2. Change the variable value, for example `@species_max_traits = 8`.
3. Every archetype referencing that variable updates with it; `LITHOID` updates through its inheritance.
4. Verify in species creation that the new limits apply to each archetype.

## Enforcement

The engine enforces the limits during species creation:

1. The species portrait and empire type determine the archetype.
2. The engine reads that archetype's trait points and maximum traits.
3. The trait selector offers only traits whose `allowed_archetypes` contains the archetype.
4. The sum of selected `cost` values must stay within the trait point budget; negative costs raise the available budget.
5. The selected count cannot exceed the maximum traits.

## Interaction with trait definitions

| Trait definition field | Interaction |
|------------------------|-------------|
| `allowed_archetypes` | Filters the traits offered for the archetype |
| `cost` | Consumes or grants trait points from the archetype budget |
| `opposites` | Removes conflicting traits from the selectable pool |

All 58 traits declare `BIOLOGICAL` in `allowed_archetypes`; 41 of them also declare `LITHOID`. No trait declares `MACHINE` or `ROBOT`, so machine and robot species gain trait capacity from this file without gaining mod traits.

## Relationship to vanilla limits

The mod raises the vanilla maximum trait counts while keeping the vanilla point budgets. The repository does not contain the vanilla values, so a direct comparison can only be made in game.

## Verification

1. Start Stellaris with the mod active.
2. Create a biological species and confirm two trait points and seven selectable traits.
3. Create a lithoid species and confirm the inherited limits.
4. Create a machine empire and confirm one trait point and seven traits, with no mod traits offered.
5. Create a robot species and confirm zero trait points and six traits.
6. Inspect `error.log` for archetype parse failures.

## Related documentation

| Document | Scope |
|----------|-------|
| [Data model](../data-model.md) | Variable definitions, archetype table |
| [Architecture](../architecture.md) | Archetype area, interfaces |
| [Components/trait-definitions.md](./trait-definitions.md) | The `allowed_archetypes` field |