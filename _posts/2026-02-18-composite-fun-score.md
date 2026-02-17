---
layout: post
featured: true
title: "One Formula to Measure Fun: The Composite Fun Score"
date: 2026-02-18
tags: [synthesis, design]
description: "Combining tension, player agency, and choice paradox into a single game quality metric. CFS = wGDS × (1 + PI/GDS) × (1 - CPG). The dominant factor isn't tension or agency — it's whether fun and winning align."
---

After five phases of research — from basic tension measurement through agency integration, perceptual weighting, and variance injection — everything converges to one formula.

## The Problem

We had three separate metrics:
- **GDS** (Game Dramatic Score): how much win probability fluctuates — "tension potential"
- **PI** (Policy Impact): how much a player's strategy choice affects their experience — "agency"
- **CPG** (Choice Paradox Gap): how far apart the most fun strategy is from the winning strategy — "alignment"

Each captures something real. A game with high GDS but zero PI is just watching dice. A game with high PI but massive CPG punishes players for having fun. We needed a single number that rewards all three simultaneously.

## The Formula

$$CFS(\alpha) = wGDS(\alpha) \times (1 + \frac{PI}{GDS}) \times (1 - CPG)$$

Where:
- **wGDS(α)** = weighted GDS with perceptual decay factor α. This captures *perceived* tension, discounting anticipation levels that humans can't detect. For casual games, α ≈ 0.5. For strategy games, α ≈ 0.7.
- **(1 + PI/GDS)** = agency multiplier. Scales from 1× (no agency, pure chance) to 2× (all tension is player-controlled). This is the "agency fraction" — what percentage of the game's engagement the player actually controls.
- **(1 - CPG)** = paradox penalty. Ranges from 1 (no penalty — fun and winning align) to 0 (complete misalignment — the fun strategy always loses). This is the critical factor.

## What We Found

Testing across 45 parameter configurations:

| Metric | Baseline Game | Optimized Game | Change |
|---|---|---|---|
| CFS (α=0.5) | **0.257** | **0.604** | +135% |
| wGDS | 0.296 | 0.361 | +22% |
| PI | 0.161 | 0.368 | +129% |
| CPG | 0.346 | 0.000 | -100% |

The optimized game scores 135% higher — not because it has much more tension, but because it *eliminates the paradox entirely*.

## The Dominant Factor: CPG

Factor analysis reveals something unexpected. Among the three components:

| Factor | Coefficient of Variation | Correlation with CFS |
|---|---|---|
| Paradox Penalty (1-CPG) | 0.239 | **0.921** |
| wGDS (tension) | 0.091 | 0.484 |
| Agency (1+PI/GDS) | 0.073 | 0.312 |

**CPG explains 92% of the variance in CFS.** Tension and agency barely matter in comparison.

This means: the single most impactful thing a designer can do is ensure that the fun strategy and the winning strategy are the same strategy. Everything else — more tension, more choices, more complexity — is secondary.

## How to Eliminate CPG

We found three structural archetypes:

### 1. Direct Variance Games (e.g., Combat)

Games where actions directly control outcome variance. CPG elimination: make the aggressive action's expected value higher than the defensive action's.

```
Before: Guard (safe, boring) has EV 0.493 > Heavy (risky, exciting) EV 0.440
After:  Heavy EV 0.524 > Guard EV 0.484
Result: CPG drops from 0.346 → 0.000
```

### 2. Resource Allocation Games (e.g., CoinDuel)

Games where players choose exposure levels to random processes. CPG elimination: make high-risk bets clearly superior.

```
Before: min wager (safe) dominates → CPG 0.213
After:  max wager range increased, refill faster → CPG 0.000
```

### 3. Sequential Information Games (e.g., DraftWars)

Games where variance comes from opponent interaction. These are the hardest — deterministic resolution creates what we call the **Deterministic Dominance Trap**: the best strategy produces certain outcomes, killing all tension.

The fix: **inject intrinsic variance** into outcome resolution. Probabilistic card activation, critical hits, random modifiers. This ensures that even the optimal strategy has uncertain results.

```
Before: deterministic battle → dominant strategy → certain outcome → GDS=0 → CPG 0.249
After:  probabilistic activation (65%) + attack weight (1.6×) → CPG 0.017
```

## The Universal Principle

CPG → 0 requires two conditions simultaneously:

1. **The risky action must have higher expected value** than the safe action
2. **The winning strategy must produce non-deterministic outcomes** (GDS > 0 for that strategy)

Both conditions are necessary. Attack advantage alone doesn't work if outcomes are deterministic. Randomness alone doesn't work if the safe strategy still wins.

## Practical Takeaway

If you're designing a game and want it to be engaging:

1. First, check if the winning strategy is also the exciting one. If not, that's your biggest problem.
2. Make aggressive play slightly more rewarding than defensive play.
3. Ensure every strategy — even the optimal one — has uncertain outcomes.
4. Only *after* alignment is fixed should you worry about adding more tension or more choices.

The CFS formula makes this measurable. You can compute it for your game and see exactly where the score is being lost.

Try it yourself: [Game Design Calculator](/toa-research/calculator/)

---

*This post synthesizes results from Phases 1-5 of the ToA research. The Composite Fun Score integrates Policy Impact (Phase 1), perceptual weighting (Phase 2), and the CPG minimization principle (Phase 1, 3, 4). Full experimental results and code are in the [anticipation-theory repository](https://github.com/Taru0208/anticipation-theory).*
