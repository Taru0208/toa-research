---
layout: post
title: "Breaking the Deterministic Dominance Trap"
date: 2026-02-17
description: "How to resolve the Choice Paradox in card draft games by injecting intrinsic variance into battle resolution."
tags: [agency, cpg, design-principle]
featured: false
---

The [Choice Paradox](/toa-research/2026/02/12/the-choice-paradox/) says that in many games, the fun strategy and the winning strategy are different. We showed that for **combat games** (like Strike/Heavy/Guard), the paradox can be completely eliminated by making aggressive actions have higher expected value.

But what about **card draft games** — games where players take turns selecting cards, then battle with what they've drafted?

## The Problem: DraftWars CPG = 0.249

DraftWars is a 6-card sequential draft with automatic battle resolution. Players alternate picking cards (3 each), and the winner is determined by comparing total attack minus opponent's defense.

Three meta-strategies: Aggressive (pick highest attack), Defensive (pick highest defense), Balanced (pick highest total).

| Strategy | GDS | Win Rate |
|----------|------|----------|
| Aggressive | 0.136 | 60% |
| Defensive | 0.113 | 80% |
| Balanced | **0.000** | **100%** |

The Balanced strategy always wins — and has zero tension. CPG = 0.249.

## The Deterministic Dominance Trap

Here's why this happens and why combat games don't have this problem:

In **combat**, even the best action (Heavy with 70% hit) has uncertain outcomes. You might swing for 3 damage, or you might miss. The dominance is in *expected value*, not in *certainty*.

In **DraftWars**, the Balanced strategy always picks the highest total-power card. Since the battle compares totals deterministically, better cards always win. The dominance is in *certainty*.

This creates an inescapable trap:
1. Dominant strategy → certain outcomes → GDS = 0
2. Non-dominant strategies → uncertain outcomes → GDS > 0
3. Fun-optimal ≠ win-optimal → CPG > 0

The structural difference: combat has **intrinsic variance** (randomness within the action), while DraftWars has **extrinsic variance** (randomness comes from the opponent's picks).

## The Fix: Variance Injection

What if we add intrinsic variance to the battle resolution? Instead of deterministically comparing totals, each card has a probability of "activating" (like a hit chance). Combined with an attack advantage (attack values weighted higher), this creates:

1. Aggressive drafting becomes EV-dominant (more attack = more expected damage)
2. But individual battles are uncertain (cards might not activate)
3. The dominant strategy now has both high win rate AND high GDS

### Results

| Configuration | CPG | Reduction |
|---------------|------|-----------|
| Original (deterministic) | 0.249 | — |
| Random battle only | 0.228 | 8% |
| Attack advantage only | 0.412 | worse |
| **Both combined** (AtkW=1.6, ActP=0.65) | **0.017** | **93%** |

Neither ingredient works alone. Random battle alone barely helps (the defensive strategy becomes dominant instead). Attack advantage alone makes it worse (aggressive wins deterministically → same trap). But combined: 93% CPG reduction.

## The Universal Principle

CPG → 0 requires **two conditions**:

1. The risky action has higher expected value than the safe action
2. The winning strategy produces non-deterministic outcomes

For **intrinsic variance games** (Combat, CoinDuel), condition 2 is automatic — hit/miss is inherent. Only need condition 1.

For **extrinsic variance games** (DraftWars, card games), condition 2 fails — dominant strategy → certain outcomes. The fix: inject intrinsic variance into outcome resolution. Then condition 1 applies as usual.

### Verification Across All Game Types

| Game | Variance Type | CPG Before | CPG After | Reduction |
|------|:---:|:---:|:---:|:---:|
| Combat | Intrinsic | 0.346 | 0.000 | 100% |
| CoinDuel | Intrinsic | 0.213 | 0.000 | 100% |
| DraftWars | Extrinsic | 0.249 | 0.017 | 93% |

All three game structures achieve >90% CPG reduction. The principle generalizes.

## Design Implication

Many card games have this problem. In any game where players assemble a "deck" or "hand" and then resolve battles deterministically:

- The optimal strategy will produce predictable outcomes
- Playing suboptimally is more exciting but loses
- Players face the Choice Paradox

The fix: add randomness to battle resolution. Card activation chances, critical hits, random modifiers — anything that prevents the optimal deck from winning with certainty. This isn't about making the game "more random" overall; it's about injecting just enough intrinsic variance so that optimal play remains exciting.

*30 tests validate these findings. Code: [variance_injection.py](https://github.com/Taru0208/anticipation-theory/blob/main/python/experiments/variance_injection.py)*
