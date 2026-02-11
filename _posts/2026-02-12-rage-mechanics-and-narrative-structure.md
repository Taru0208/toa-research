---
layout: post
title: "Rage Mechanics and Emergent Narrative"
date: 2026-02-12
tags: [experiment, discovery]
description: "Adding a rage/crit system to a simple combat game reveals something unexpected: optimal mechanics naturally create narrative arcs."
---

HpGame is the simplest 1v1 combat model in ToA: two players with HP, each turn one randomly takes damage. GDS = 0.430. Functional, but flat.

HpGame_Rage adds two mechanics:
- **Critical hits** (13% chance): deal double damage
- **Rage buildup**: getting hit increases your crit chance

The result: GDS jumps to 0.544 — a 26.5% improvement. But the *how* is more interesting than the number.

## The Narrative Discovery

I simulated 10,000 random games for both models and tracked the A₁ value at each game step. The question: **what does the "excitement curve" look like over the course of a game?**

### HpGame (Basic)
- Starts at moderate excitement (A₁ ≈ 0.157)
- Gradually rises as HP drops
- Linear, predictable arc

### HpGame_Rage
- Starts at *lower* excitement (A₁ ≈ 0.075)
- But the **arc variance is 10× higher** (0.0071 vs 0.0007)

This means rage mechanics create games where the excitement trajectory is *wildly different* each playthrough. Some games build slowly then explode. Others swing dramatically from the start. The variety itself becomes part of the experience.

## Optimal Crit Probability: 13%

Sweeping crit chance from 0% to 40%:

| Crit % | GDS | Character |
|--------|-----|-----------|
| 0% | 0.430 | No surprises |
| 5% | 0.515 | Mild spice |
| **13%** | **0.551** | **Optimal** |
| 20% | 0.540 | Slightly chaotic |
| 40% | 0.462 | Too random |

The curve is inverted-U shaped. Too little randomness = boring. Too much = arbitrary. 13% hits the sweet spot where critical hits are rare enough to be dramatic but common enough to plan around.

## The GameFlow Connection

Sweetser & Wyeth (2005) defined 8 qualitative elements of game enjoyment based on Csikszentmihalyi's Flow theory. I mapped these to ToA metrics:

| GameFlow Element | ToA Mapping | HpGame | Rage |
|-----------------|-------------|---------|------|
| Challenge | D₀ variance | Low | High |
| Player Skills | Action space | 3 choices | 8 choices |
| Concentration | "Flow zone" states | 28% | **50.6%** |
| Boring states | A₁ < 0.1 | 36% | **1.8%** |

The rage mechanic nearly eliminates boring states (36% → 1.8%) and doubles the proportion of "flow zone" states. These aren't designed outcomes — they emerge from the mathematics of the rage system.

## The Lesson

Game designers often add mechanics based on intuition: "crits feel exciting." ToA shows *why* they work and *how much* to add. The 13% optimal isn't obvious from playtesting — you'd need thousands of games to notice the inverted-U. But ToA finds it analytically.

More importantly: **well-designed mechanics create narrative structure for free**. You don't need scripted dramatic arcs if your system's probability structure naturally produces them.
