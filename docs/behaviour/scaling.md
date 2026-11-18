# Behaviour: Scaling policy

This document details the tier scaling policy recorded in the trait file headers and how the recorded values relate to it.

## Policy statement

Both trait files begin with the same header comment describing the intended scaling for additional tiers:

| Tier | Effect target | Positive cost | Negative cost |
|------|---------------|---------------|---------------|
| Base | — | 1× | — |
| Very | 11/6 of the base effect, approximately 1.833 | 3× the base cost | Follows the effect factor |
| Extremely | 20 per cent above the corresponding Very effect | 4× the base cost | Follows the effect factor, with refunds truncated toward zero and raised by at least one point per stronger tier |

## Basis recorded in the header

- The mean effect factor is derived from vanilla Strong to Very Strong: 2.0 for army damage, 2.0 for worker production, and 1.5 for soldier production, giving (2.0 + 2.0 + 1.5) / 3 = 11/6.
- The cost factor is 3/1 = 3.
- Positive Extremely costs rise by 33.333333 per cent over Very, from 3× to 4× the base cost.
- Negative tier costs use the effect factor, because vanilla inverse pairs retain unit cost elasticity.
- The 20 per cent uplift sits above the 9.090909 per cent minimum implied by the median x2 vanilla upgrade and below the premium x3 and x4 vanilla outliers.
- Further successive tiers would compound the same uplift.
- Where vanilla already supplies an Extremely tier, its fixed values take precedence and the custom Very tier is back-calculated.
- Slave prices, assembly scores, AI weights, trigger thresholds, and crossbreeding chances are intentionally excluded.
- Discrete count modifiers round away from zero.

## Recorded values

### Multiplicative families

| Family | Modifier | Base | Very | Extremely |
|--------|----------|------|------|-----------|
| Agility | `weapon_type_*_weapon_fire_rate_mult` | `ui_trait_agile`, 2 at `0.05` | `ui_trait_very_agile`, 6 at `0.091667` | `ui_trait_extremely_agile`, 8 at `0.11` |
| Unity and amenities | `planet_jobs_unity_produces_mult`, `planet_amenities_mult` | `ui_trait_creative`, 2 at `0.10` | `ui_trait_very_creative`, 6 at `0.183333` | `ui_trait_extremely_creative`, 8 at `0.22` |
| Food upkeep | `planet_pops_organics_food_upkeep_mult` | `ui_trait_gluttonous`, -1 at `0.10` | `ui_trait_very_gluttonous`, -2 at `0.183333` | `ui_trait_extremely_gluttonous`, -3 at `0.22` |
| Habitability | `pop_environment_tolerance` | vanilla base | `ui_trait_very_adaptive`, 3 at `0.166667` | `ui_trait_extremely_nonadaptive`, -4 at `-0.22` |
| Breeding | `logistic_growth_mult` | vanilla base | `ui_trait_very_rapid_breeders`, 6 at `0.183333` | `ui_trait_extremely_rapid_breeders`, 8 at `0.22` |
| Army strength | `army_damage_mult` | `trait_strong`, 2 | `ui_trait_very_weak`, -2 at `-0.366667` (weak side) | `ui_trait_extremely_strong`, 4 at `0.48`; `ui_trait_extremely_weak`, -3 at `-0.44` |

The ratios in these families cluster around 1.833 for Very and 2.2 for Extremely relative to base, which matches the header target.

### Additive families

| Family | Modifier | Recorded tiers |
|--------|----------|----------------|
| Lifespan | `leader_lifespan_add`, in years | `ui_trait_long_lived` 3 years at `36.666667`; `ui_trait_very_long_lived` 4 years at `44`; `ui_trait_momentary` -4 years at `-26.4`; `ui_trait_short_lived` -2 years at `-18.333333`; `ui_trait_very_short_lived` -3 years at `-22` |
| Talent | `negative_leader_traits_species`, `leaders_upkeep_mult` | `ui_trait_very_talented` 3 points at `-2` and `-0.183333` |
| Empire size | `species_empire_size_mult` | `ui_trait_very_docile` 6 at `-0.183333`; `ui_trait_very_unruly` -3 at `0.183333` |
| Bureaucrat workforce | `bureaucrat_jobs_bonus_workforce_mult` | `ui_trait_very_traditional` 3 at `0.183333`; `ui_trait_very_quarrelsome` -2 at `-0.183333` |

A ratio is inapplicable to an additive modifier, so these families follow the target magnitude while remaining additive. The header acknowledges this by describing 11/6 as a reference factor.

### Incomplete families

Several families declare only some tiers:

| Family | Tiers present |
|--------|---------------|
| Adaptability | very positive, very negative, extremely negative |
| Breeding | very positive, extremely positive, very negative, extremely negative |
| Civic Behaviour | very positive, very negative |
| Lifespan | base positive, very positive, base negative, very negative, momentary |
| Social Cohesion | very positive, very negative |
| Strength | extremely positive, very negative, extremely negative |
| Talent | very positive |

The missing variants reflect vanilla traits that already supply a tier, which the header states takes precedence.

## Negative tier costs

| Tier | Costs observed |
|------|----------------|
| Very negative | `-2` (`ui_trait_very_weak`, `ui_trait_very_quarrelsome`), `-3` (`ui_trait_very_nonadaptive`, `ui_trait_very_slow_breeders`, `ui_trait_very_short_lived`, `ui_trait_very_sluggish`, `ui_trait_very_uncreative`, `ui_trait_very_gluttonous`, `ui_trait_very_demanding`) |
| Extremely negative | `-3` (`ui_trait_extremely_weak`, `ui_trait_extremely_gluttonous`, `ui_trait_extremely_demanding`), `-4` (`ui_trait_extremely_nonadaptive`, `ui_trait_extremely_slow_breeders`, `ui_trait_extremely_sluggish`, `ui_trait_extremely_uncreative`) |

Negative refunds consequently increase by exactly one point per tier in most families, matching the truncation-plus-one rule in the header.

## Positive tier costs

| Tier | Costs observed |
|------|----------------|
| Very positive | `3` (adaptability, food, talent, housing, social cohesion), `6` (agility, unity, breeding, civic behaviour) |
| Extremely positive | `4` (adaptability, strength, food), `8` (agility, unity, breeding) |

The cost depends on the base cost of the vanilla family the tier extends, so a family whose base costs 1 point reaches Very at 3 and Extremely at 4, while a family whose base costs 2 points reaches Very at 6 and Extremely at 8.

## Verification

Per family:

- [ ] The base identifier exists in the targeted game version.
- [ ] Very cost equals three times the base cost.
- [ ] Extremely cost equals four times the base cost, or the recorded vanilla value where vanilla supplies that tier.
- [ ] Very effects approximate 1.833× the base effect for multiplicative families.
- [ ] Extremely effects approximate 1.2× the Very effect.
- [ ] `opposites` lists every family member.
- [ ] Localisation keys and icons exist for every tier.
- [ ] AI weights do not cause the AI to select tiers it is unable to afford.

No automated verification exists. The repository contains no script that parses the trait files or compares values.

## Modification procedure

1. Update the header comment in both trait files so the recorded policy stays current.
2. Recalculate the affected tier values.
3. Update the localisation only if a description no longer fits; the descriptions carry no numeric values.
4. Verify in game that the point economy still permits the intended selections.
5. Update this document.

## Point economy

| Archetype | Trait points | Maximum traits | Can afford an Extremely positive tier alone? |
|-----------|--------------|----------------|----------------------------------------------|
| `BIOLOGICAL` | 2 | 7 | No, because the cheapest Extremely positive tier costs 4 |
| `LITHOID` | 2 | 7 | No |
| `PRESAPIENT` | 2 | 7 | No |
| `MACHINE` | 1 | 7 | No, and no mod trait is offered |
| `ROBOT` | 0 | 6 | No, and no mod trait is offered |

Affording an Extremely positive tier consequently requires negative traits to raise the budget, which is the recorded design intent.

## Related documentation

| Document | Scope |
|----------|-------|
| [Data model](../data-model.md) | Scaling summary, catalogue |
| [Components/trait-definitions.md](../components/trait-definitions.md) | Modifier values |
| [Behaviour/trait-effects.md](./trait-effects.md) | Modifier application, stacking |