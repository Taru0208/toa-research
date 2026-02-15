---
layout: post
featured: true
title: "What is Anticipation Theory?"
date: 2026-02-12
tags: [theory, overview]
description: "A mathematical framework that measures the structural tension of games — the raw material of engagement."
---

Can you measure the *tension* in a game? Not whether players enjoy it — that depends on taste, context, and a hundred other things. But the structural tension: how much the question "who's winning?" swings from moment to moment?

Anticipation Theory says yes. And that tension turns out to be a surprisingly good foundation for understanding engagement.

## The Core Idea

Developed by Jeeheon (Lloyd) Oh at Farseer Co., the theory starts from a simple observation: **fun comes from uncertainty**. Not chaos — *structured* uncertainty. The coin flip before a critical hit. The fog of war hiding enemy positions. The split-second decision between aggressive and defensive play.

ToA formalizes this into measurable quantities.

## Local Anticipation (A₁)

At any game state, there are possible outcomes. Each outcome has a probability and a value. A₁ measures the *spread* of these possibilities — technically, the probability-weighted standard deviation of outcome values:

```
A₁(s) = √( Σ P(s→s') · (D(s') - μ)² )
```

Where `D(s')` is the desirability of reaching state s', and μ is the expected value.

A fair coin toss gives A₁ = 0.500 — the theoretical maximum for a binary outcome (win or lose, 50/50). Rock-Paper-Scissors achieves A₁ = 0.471 because its three outcomes (win/draw/lose at 1/3 each) spread probability more evenly, reducing the variance compared to a sharp 50/50 split.

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
| HpGame + Rage (optimized) | 0.654 | +52%, crit=13%, rage mult=2 |
| MOBA Lane Model | 0.540 | High depth ratio |
| GA-Optimized (symmetric) | **0.979** | Maximum symmetric GDS |
| GA + Accumulation | **1.429** | Best discovered so far |

Higher GDS means more structural tension — the game's state creates wider swings in "who's winning?" over time. But more interesting than the number itself is *which components contribute* — A₁-heavy games feel like gambling; A₂+-heavy games feel strategic.

**An important caveat:** GDS measures *tension potential*, not fun directly. A dice-rolling game with no player decisions can have high GDS — plenty of dramatic swings, but no way to influence them. Tension is the *raw material* of engagement; what turns it into fun is player agency. A game needs both.

Why does GA + Accumulation score so much higher than the symmetric optimum? Symmetric games have a ceiling: when both sides are identical, the initial win probability is always 50%, limiting how far outcomes can swing. Accumulation mechanics (resources that carry between turns) break this symmetry — they create states where one player has built an advantage, producing a wider range of win probabilities across the game and therefore higher GDS.

## Why This Matters

Most game design is intuition-driven. Designers playtest, iterate, and rely on experience to know what "feels right." ToA offers a complementary approach: **compute the engagement structure before building the game**.

You could:
- Compare two ability designs mathematically before implementing either
- Identify exactly which mechanic creates depth vs. noise
- Optimize parameters (damage values, cooldowns, probabilities) with a concrete objective

This research log documents my exploration of these ideas — porting the theory to Python, reproducing all reference games, and pushing into new territory with genetic algorithms and novel game models.

## Resources

- [Original C++ Implementation](https://github.com/akalouis/anticipation-theory) (MIT License)
- [Extended Research Fork](https://github.com/Taru0208/anticipation-theory) with Python port and experiments
