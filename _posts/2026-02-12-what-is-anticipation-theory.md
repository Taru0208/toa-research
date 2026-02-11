---
layout: post
title: "What is Anticipation Theory?"
date: 2026-02-12
tags: [theory, overview]
description: "A mathematical framework that answers the question: can we measure fun?"
---

Can you measure fun? Not satisfaction surveys or retention metrics — the raw, moment-to-moment *engagement* of playing a game?

Anticipation Theory says yes.

## The Core Idea

Developed by Jeeheon (Lloyd) Oh at Farseer Co., the theory starts from a simple observation: **fun comes from uncertainty**. Not chaos — *structured* uncertainty. The coin flip before a critical hit. The fog of war hiding enemy positions. The split-second decision between aggressive and defensive play.

ToA formalizes this into measurable quantities.

## Local Anticipation (A₁)

At any game state, there are possible outcomes. Each outcome has a probability and a value. A₁ measures the *spread* of these possibilities — technically, the probability-weighted standard deviation of outcome values:

```
A₁(s) = √( Σ P(s→s') · (D(s') - μ)² )
```

Where `D(s')` is the desirability of reaching state s', and μ is the expected value.

A fair coin toss gives A₁ = 0.500 — the theoretical maximum for a binary outcome. Rock-Paper-Scissors achieves A₁ = 0.471 (94.2% of the maximum) because even a perfectly random game isn't a pure coin flip once you have three outcomes.

The insight: **A₁ captures immediate excitement**. High A₁ means anything could happen right now.

## But A₁ Isn't Enough

A game that maximizes A₁ at every state is a slot machine — each pull is exciting, but there's no depth. ToA solves this with **nested anticipation**.

A₂ measures the variance of *A₁ values* across possible futures. It captures whether current decisions lead to a *range* of different excitement levels. High A₂ means: "my choices now determine whether the game becomes tense or boring later."

A₃ does the same for A₂. And so on recursively.

This nesting captures **strategic depth** — the feeling that what you do now *matters* for the shape of the game to come.

## Game Design Score (GDS)

The GDS combines all anticipation components into a single metric. It's the weighted sum of A₁ through Aₙ, averaged across all reachable states weighted by their probability of being reached:

| Game | GDS | Character |
|------|-----|-----------|
| HpGame (basic 1v1) | 0.430 | Simple, readable |
| HpGame + Rage | 0.544 | +26%, more dramatic |
| MOBA Lane Model | 0.540 | Highest depth ratio |
| GA-Optimized | **0.979** | Maximum symmetric GDS |
| GA + Accumulation | **1.429** | Best discovered so far |

Higher GDS correlates with the intuitive sense that a game is more engaging. But more interesting than the number itself is *which components contribute* — A₁-heavy games feel like gambling; A₂+-heavy games feel strategic.

## Why This Matters

Most game design is intuition-driven. Designers playtest, iterate, and rely on experience to know what "feels right." ToA offers a complementary approach: **compute the engagement structure before building the game**.

You could:
- Compare two ability designs mathematically before implementing either
- Identify exactly which mechanic creates depth vs. noise
- Optimize parameters (damage values, cooldowns, probabilities) with a concrete objective

This research log documents my exploration of these ideas — porting the theory to Python, reproducing all reference games, and pushing into new territory with genetic algorithms and novel game models.

## Resources

- [Original C++ Implementation](https://github.com/jeeheonoh/anticipation_theory) (MIT License)
- [Extended Research Fork](https://github.com/Taru0208/anticipation-theory) with Python port and experiments
