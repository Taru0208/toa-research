---
layout: post
title: "Rage Mechanics: The Optimal Configuration"
date: 2026-02-12
tags: [experiment, discovery]
description: "Systematic optimization of rage/crit mechanics reveals crit=13%, damage multiplier=2, and non-spending rage as the optimal combat configuration — a 52% improvement over baseline."
---

HpGame is the simplest 1v1 combat model in ToA: two players with HP=5, each turn producing one of three random outcomes (P1 hits, both hit, P2 hits), each with 1/3 probability. GDS = 0.430. Functional, but flat.

HpGame_Rage adds two mechanics:
- **Critical hits** (configurable chance): deal bonus damage scaled by accumulated rage (`1 + rage × multiplier`)
- **Rage buildup**: rage counter increases when dealing or receiving damage

Neither player makes choices — all outcomes are probabilistic. The game tests whether structural mechanics alone can improve engagement.

## Systematic Configuration Search

The original C++ repo compared 8 different configurations of the rage system. We reproduced this in Python:

| Configuration | GDS | Change |
|:---|:---:|:---:|
| Baseline (no rage, crit=13%) | 0.427 | — |
| Rage mult=1, spend, on_dmg | 0.522 | +22% |
| Rage mult=1, nospend, on_dmg | 0.541 | +27% |
| Rage mult=1, spend, on_recv | 0.428 | +0.3% |
| Rage mult=1, nospend, on_recv | 0.432 | +1% |
| Rage mult=1, nospend, on_both | 0.551 | +29% |
| Rage mult=2, spend, on_both | 0.648 | +52% |
| **Rage mult=2, nospend, on_both** | **0.654** | **+53%** |

Three clear insights emerge:

**1. Damage multiplier matters most.** Going from mult=1 to mult=2 produces the biggest jump (0.551 → 0.654). Higher rage means more devastating crits, which increases the variance of future outcomes — exactly what A₂+ captures.

**2. Non-spending rage > spending.** When rage resets to zero after a crit (`spend`), the mechanic is one-shot. When rage accumulates (`nospend`), it compounds — creating accelerating tension as the game progresses. This persistent accumulation is what creates the narrative arc.

**3. Attack triggers > receive triggers.** Gaining rage from dealing damage (0.541) helps far more than gaining from receiving damage (0.432). The "on_both" configuration is slightly better still (0.551), but the asymmetry shows that offensive momentum is structurally more engaging.

## Optimal Critical Hit Probability

Sweeping crit chance from 0% to 40% with the optimized config (mult=2, nospend, on_both):

| Crit % | GDS |
|:---:|:---:|
| 0% | 0.430 |
| 5% | 0.562 |
| 10% | 0.639 |
| **13-14%** | **0.654-0.655** |
| 20% | 0.635 |
| 30% | 0.558 |
| 40% | 0.481 |

The curve is inverted-U shaped. The peak at 13-14% means crits are rare enough to be dramatic but common enough to influence strategy. Below 10%, rage barely activates. Above 20%, every turn is a crit and the mechanic loses its specialness.

The original C++ repo identified 13% as the optimum with a grid search — our Python reproduction confirms this, with the peak spanning 13-14% (the difference between them is less than 0.002).

## The 52% Improvement

The fully optimized configuration achieves GDS = 0.654 compared to the baseline HpGame's 0.430 — a **52% improvement**. This matches the original C++ result (GDS 0.653, +51.8%) with less than 0.2% numerical difference.

For reference:

| Game | GDS | States |
|:---|:---:|:---:|
| HpGame (baseline) | 0.430 | 36 |
| HpGame_Rage (default config, crit=10%, mult=1) | 0.544 | 318 |
| HpGame_Rage (optimized, crit=13%, mult=2) | **0.654** | 318 |
| CoinToss (theoretical max per-turn) | 0.500 | 3 |

The optimized rage game exceeds the single-turn theoretical maximum — demonstrating that multi-turn structural mechanics generate more total engagement than any single moment of perfect uncertainty.

## Code

All results are reproducible with the [Python port](https://github.com/Taru0208/anticipation-theory):

```python
from toa.engine import analyze
from toa.games.hpgame_rage import HpGameRage

config = HpGameRage.Config(
    critical_chance=0.13,
    rage_spendable=False,
    rage_dmg_multiplier=2,
    rage_increase_on_attack_dmg=True,
    rage_increase_on_received_dmg=True,
)
result = analyze(
    initial_state=HpGameRage.initial_state(),
    is_terminal=HpGameRage.is_terminal,
    get_transitions=HpGameRage.get_transitions,
    compute_intrinsic_desire=HpGameRage.compute_intrinsic_desire,
    config=config,
    nest_level=5,
)
print(f"GDS: {result.game_design_score:.3f}")  # 0.654
```
