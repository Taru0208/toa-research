---
layout: post
title: "Genetic Algorithms Find the Optimal Game"
date: 2026-02-12
tags: [experiment, discovery]
description: "Using evolutionary optimization to discover game mechanics that maximize GDS. The best game found scores 1.429 — 159% above HpGame_Rage."
---

Instead of designing games by hand and measuring their GDS, what if we let an algorithm *search* for the best game?

## The Setup

A genetic algorithm (GA) that evolves game transition tables:

- **Genome**: A complete game definition — states, transitions, probabilities
- **Fitness**: GDS score from the ToA engine
- **Constraints**: Symmetric, fair games (D₀ = 0.5), valid probability distributions

Each generation: evaluate 200 games, keep the best, mutate and crossbreed.

## Round 1: Symmetric Games

Constraining to symmetric games (both players have identical action spaces), the GA converges reliably:

**Best symmetric game: GDS = 0.979**

8 out of 8 multi-seed runs converged to essentially the same design:

| Property | Value |
|----------|-------|
| HP | 3 (per player) |
| Outcomes per turn | 8 |
| Instant kill possible | Yes (-3 damage) |
| Healing | Minimal (+1, rare) |
| Boring states | 0% |

The structure: every turn, someone takes meaningful damage. Games are short (3-5 turns) but every turn matters. There's a ~50% chance of instant kill from any state — not as randomness, but as a consequence of high-variance outcomes.

This matches an important principle: **appropriate lethality creates both immediate drama (A₁) and multi-turn depth (A₂+)**. If you might die any turn, every survival is meaningful.

The score 0.979 appears to be a hard ceiling for symmetric games. The GA finds it from any starting point.

## Round 2: Asymmetric Games

Removing the symmetry constraint, the GA finds higher scores — up to GDS = 1.30 — but with D₀ = 0.80 (one player has 80% expected win rate). These are mathematically interesting but not fair games.

## Round 3: Accumulation Mechanics

The breakthrough came from expanding the genome to include *resource accumulation*:

**Best game found: GDS = 1.429**

| Property | Value | Significance |
|----------|-------|--------------|
| GDS | 1.429 | 159% > HpGame_Rage |
| States | 98 | Rich state space |
| D₀ | 0.500 | Perfectly fair |
| A₁ contribution | 0.29 | Immediate excitement |
| A₂ contribution | 0.34 | Strategic depth (higher!) |

The optimal mechanic: **accumulate a resource when dealing damage**. At 4 stacks, gain +3 bonus damage. The resource doesn't deplete on use.

This creates a natural game arc:
1. **Early game**: Cautious. Low stacks, low damage. Building power.
2. **Mid game**: Divergent. Players race to accumulate. One pulls ahead.
3. **Late game**: Explosive. High-stack players deal devastating damage.

The crucial feature: A₂ > A₁. The game's strategic depth exceeds its immediate excitement. Your choices about when to be aggressive (building stacks) vs. defensive (preserving HP) create more engagement than any single random outcome.

## What the GA Reveals

Three principles emerge from the optimization:

**1. Lethality matters.** The best games have instant-kill potential. Not because randomness is fun, but because high stakes make every state meaningful.

**2. Accumulation beats simplicity.** Resource mechanics that compound over time create richer decision spaces than static combat. The 46% jump from 0.979 to 1.429 comes entirely from adding a power meter.

**3. Depth should exceed drama.** In the best games, A₂ > A₁ — the *anticipation of anticipation* matters more than raw excitement. This is why simple gambling feels hollow compared to strategic games: the higher-order structure is missing.

## Open Question

Is GDS = 1.429 the theoretical maximum for fair two-player games? Or can more complex accumulation mechanics push even higher?

The GA has only explored a small corner of the design space. More complex genomes (multiple resources, conditional mechanics, asymmetric-but-fair designs) might find games with even higher engagement. But 1.429 is already nearly 3× the score of professionally designed game mechanics (HpGame_Rage: 0.544).
