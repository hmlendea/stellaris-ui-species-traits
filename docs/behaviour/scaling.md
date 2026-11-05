# Behaviour: Scaling policy

This document details the tier scaling policy, its mathematical basis, application across trait families, and verification methods.

## Policy statement

The mod extends 11 vanilla trait families with **Very** and **Extremely** tiers using a consistent scaling policy:

| Tier | Cost multiplier (vs base) | Effect multiplier (vs base) | Effect multiplier (vs previous tier) |
|------|---------------------------|----------------------------|--------------------------------------|
| Base (vanilla) | 1× | 1× | — |
| **Very** | **3×** | **11/6 ≈ 1.833×** | — |
| **Extremely** | **4×** | **11/6 × 1.2 = 2.2×** | **+20%** |

## Mathematical basis

### Very tier: 11/6 multiplier

The 11/6 ratio (≈1.833) was chosen because:
- It approximates a "strong but not double" increase
- It's a rational fraction, avoiding floating-point precision issues in modifier math
- It scales cleanly: base 2 → Very 6 (3× cost), base effect × 11/6

### Extremely tier: +20% over Very

The +20% increment was chosen because:
- It provides a noticeable but not overwhelming step up
- It maintains the 3×/4× cost progression (base→Very→Extremely)
- It keeps Extremely traits viable but expensive (cost 8 vs base 2)

## Application per family

### Strength family

| Trait | Cost | `army_damage_mult` | `planet_jobs_minerals_produces_mult` |
|-------|------|-------------------|-------------------------------------|
| `trait_strong` (base) | 2 | 0.2 | 0.1 |
| `ui_trait_very_strong` | 6 (3×) | 0.3 (×1.5) | 0.15 (×1.5) |
| `ui_trait_extremely_strong` | 8 (4×) | 0.4 (×2.0) | 0.2 (×2.0) |

**Note:** Actual multipliers in file are 1.5× and 2.0× vs base, not exactly 11/6 and 2.2. The 11/6 is the design target; implementation uses clean decimals.

### Agility family

| Trait | Cost | `army_damage_mult` | `ship_evasion_mult` |
|-------|------|-------------------|---------------------|
| `trait_agile` (base) | 2 | 0.1 | 0.05 |
| `ui_trait_very_agile` | 6 | 0.18 | 0.09 |
| `ui_trait_extremely_agile` | 8 | 0.24 | 0.12 |

Multipliers: Very = 1.8× base, Extremely = 2.4× base

### Intelligence family

| Trait | Cost | `research_speed_mult` |
|-------|------|----------------------|
| `trait_intelligent` (base) | 2 | 0.1 |
| `ui_trait_very_intelligent` | 6 | 0.18 |
| `ui_trait_extremely_intelligent` | 8 | 0.24 |

Multipliers: Very = 1.8×, Extremely = 2.4×

### Breeding family

| Trait | Cost | `pop_growth_speed_mult` |
|-------|------|------------------------|
| `trait_rapid_breeders` (base) | 2 | 0.1 |
| `ui_trait_very_rapid_breeders` | 6 | 0.3 |
| `ui_trait_extremely_rapid_breeders` | 8 | 0.4 |

Multipliers: Very = 3.0×, Extremely = 4.0× (higher due to growth compounding)

### Adaptability family

| Trait | Cost | `pop_environment_tolerance` |
|-------|------|----------------------------|
| `trait_adaptive` (base) | 2 | 0.1 |
| `ui_trait_very_adaptive` | 6 | 0.3 |
| `ui_trait_extremely_adaptive` | 8 | 0.4 |

Multipliers: Very = 3.0×, Extremely = 4.0×

### Lifespan family

| Trait | Cost | `leader_lifespan_mult` |
|-------|------|------------------------|
| `trait_long_lived` (base) | 2 | 0.2 |
| `ui_trait_very_long_lived` | 6 | 0.6 |
| `ui_trait_extremely_long_lived` | 8 | 0.8 |

Multipliers: Very = 3.0×, Extremely = 4.0×

### Thrift family

| Trait | Cost | `pop_jobs_energy_produces_mult` |
|-------|------|--------------------------------|
| `trait_thrifty` (base) | 2 | 0.05 |
| `ui_trait_very_thrifty` | 6 | 0.15 |
| `ui_trait_extremely_thrifty` | 8 | 0.2 |

Multipliers: Very = 3.0×, Extremely = 4.0×

### Charisma family

| Trait | Cost | `trade_value_mult` | `monthly_unity_mult` |
|-------|------|-------------------|---------------------|
| `trait_charismatic` (base) | 2 | 0.1 | 0.05 |
| `ui_trait_very_charismatic` | 6 | 0.15 | 0.1 |
| `ui_trait_extremely_charismatic` | 8 | 0.2 | 0.15 |

Multipliers: Very = 1.5–2.0×, Extremely = 2.0–3.0×

### Engineering/Physics/Sociology families

| Trait | Cost | Research speed mult |
|-------|------|---------------------|
| `trait_natural_engineers` (base) | 2 | 0.1 |
| `ui_trait_very_natural_engineers` | 6 | 0.18 |
| `ui_trait_extremely_natural_engineers` | 8 | 0.24 |

Multipliers: Very = 1.8×, Extremely = 2.4×

## Negative trait scaling

Negative traits mirror positive scaling with inverted signs:

| Family | Base cost | Very cost | Extremely cost |
|--------|-----------|-----------|----------------|
| Strong/Weak | -2 | -6 | -8 |
| Agile/Clumsy | -2 | -6 | -8 |
| Intelligent/Slow Learners | -2 | -6 | -8 |
| Rapid/Slow Breeders | -2 | -6 | -8 |
| Adaptive/Nonadaptive | -2 | -6 | -8 |
| Long/Short Lived | -2 | -6 | -8 |
| Thrifty/Wasteful | -2 | -6 | -8 |
| Charismatic/Repugnant | -2 | -6 | -8 |
| Natural Engineers/Poor Engineers | -2 | -6 | -8 |
| Natural Physicists/Poor Physicists | -2 | -6 | -8 |
| Natural Sociologists/Poor Sociologists | -2 | -6 | -8 |

Effect magnitudes match positive counterparts (negative values).

## Custom trait scaling (ui-traits_species.txt)

The 38 custom traits in `ui-traits_species.txt` include their own Very/Extremely tiers that follow the same policy:

| Custom family | Base | Very | Extremely |
|---------------|------|------|-----------|
| Agile/Sluggish | `ui_trait_agile` (2) | `ui_trait_very_agile` (6) | `ui_trait_extremely_agile` (8) |
| Creative/Uncreative | `ui_trait_creative` (2) | `ui_trait_very_creative` (6) | `ui_trait_extremely_creative` (8) |
| Demanding/Undemanding | `ui_trait_demanding` (-2) | `ui_trait_very_demanding` (-6) | `ui_trait_extremely_demanding` (-8) |
| Gluttonous/Light Eaters | `ui_trait_gluttonous` (-2) | `ui_trait_very_gluttonous` (-6) | `ui_trait_extremely_gluttonous` (-8) |
| Long/Short Lived | `ui_trait_long_lived` (2) | `ui_trait_very_long_lived` (6) | `ui_trait_extremely_long_lived` (8) |
| Rapid/Slow Breeders | (vanilla base) | `ui_trait_very_rapid_breeders` (6) | `ui_trait_extremely_rapid_breeders` (8) |

## Verification checklist

### Manual verification (per trait family)

For each of the 11 vanilla families + 6 custom families:

- [ ] Base trait exists in vanilla (verify ID matches)
- [ ] Very tier cost = 3× base cost
- [ ] Extremely tier cost = 4× base cost
- [ ] Very tier effects ≈ 1.8–3.0× base (varies by family)
- [ ] Extremely tier effects ≈ 2.2–4.0× base (varies by family)
- [ ] Extremely tier effects ≈ 1.2× Very tier effects
- [ ] Opposites list includes all family members (base, Very, Extremely, positive, negative)
- [ ] Localisation keys exist for all three tiers
- [ ] Icon files exist for all three tiers
- [ ] AI weights scale appropriately (higher for higher tiers)

### Automated verification (not implemented)

No automated verification exists. Potential future implementation:

```python
# Pseudocode for verification script
def verify_scaling():
    for family in TRAIT_FAMILIES:
        base = load_trait(family.base_id)
        very = load_trait(family.very_id)
        extremely = load_trait(family.extremely_id)

        assert very.cost == base.cost * 3
        assert extremely.cost == base.cost * 4

        for modifier in base.modifiers:
            very_ratio = very.modifiers[modifier] / base.modifiers[modifier]
            extremely_ratio = extremely.modifiers[modifier] / base.modifiers[modifier]
            tier_ratio = extremely.modifiers[modifier] / very.modifiers[modifier]

            assert 1.5 <= very_ratio <= 3.0  # family-dependent
            assert 2.0 <= extremely_ratio <= 4.0
            assert 1.15 <= tier_ratio <= 1.25  # ~20%
```

## Policy exceptions

| Family | Exception | Reason |
|--------|-----------|--------|
| Breeding | Very = 3×, Extremely = 4× base effect | Growth compounds exponentially; linear scaling too weak |
| Adaptability | Very = 3×, Extremely = 4× base effect | Habitability caps at 100%; higher tiers need stronger base |
| Lifespan | Very = 3×, Extremely = 4× base effect | Leader lifespan has diminishing returns |
| Thrift | Very = 3×, Extremely = 4× base effect | Energy production scales linearly |

## Modification procedure

To change the scaling policy:

1. **Update file headers** in `ui-traits_species.txt` and `ui-traits_species_newtiers.txt` with new policy
2. **Recalculate all tier values** for all 17 families (11 vanilla + 6 custom)
3. **Update trait definitions** with new costs and modifier values
4. **Update localisation** descriptions to match new values
5. **Test in-game** for balance across all archetypes
6. **Update this document** with new policy

## Balance considerations

### Trait point economy

| Archetype | Trait points | Max traits | Can afford Extremely? |
|-----------|--------------|------------|----------------------|
| BIOLOGICAL | 2 | 7 | No (cost 8 > 2) — needs negatives |
| LITHOID | 2 | 7 | No — needs negatives |
| MACHINE | 1 | 7 | No |
| ROBOT | 0 | 6 | No |

**Design intent:** Extremely tiers require stacking negative traits to afford. This is intentional — they are "endgame" picks for specialised builds.

### AI accessibility

AI rarely picks Extremely tiers due to cost. Very tiers are occasionally picked by focused AI (Militarist → Very Strong, Materialist → Very Intelligent).

### Multiplayer balance

Extremely tiers are expensive enough that they don't dominate; they represent specialised all-in strategies.

## Related documentation

| Document | Scope |
|----------|-------|
| [Data model](../data-model.md) | Scaling policy summary, trait catalogue |
| [Components/trait-definitions.md](../components/trait-definitions.md) | Trait modifier values |
| [Behaviour/trait-effects.md](./trait-effects.md) | Modifier application, stacking |
| [Flows/trait-selection.md](../flows/trait-selection.md) | Cost constraints in selection |