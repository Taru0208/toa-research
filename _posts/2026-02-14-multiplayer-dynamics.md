---
layout: post
title: "The Multiplayer Paradox: Less Winning, More Fun Per Win"
date: 2026-02-14 22:00:00 +0900
tags: [experiment, multiplayer, game-design]
---

Every game in our ToA analysis so far has been 1v1. But most competitive games people actually play — Battle Royale, party games, FFA modes — involve 3 or more players. How does adding players change the math of engagement?

The answer is surprising: you win less often, but each potential win generates *more* anticipation.

## The Model

We extended the HP Battle model to N players. Each turn, a random pair of surviving players fight (50/50 who deals damage). When a player reaches 0 HP, they're eliminated. Last player standing wins.

This creates a natural elimination structure: a 3-player game has two phases (3-alive → 2-alive → winner), while a 4-player game has three. Does this layered structure help or hurt engagement?

## Results

### Raw GDS vs Player Count

| Players | HP Each | GDS | A₁ | A₂+ | Depth | States |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 2 | 3 | **0.443** | 0.242 | 0.201 | 45.4% | 16 |
| 3 | 3 | **0.429** | 0.141 | 0.289 | 67.2% | 63 |
| 4 | 2 | **0.411** | 0.125 | 0.286 | 69.6% | 80 |

At first glance, more players = slightly lower GDS. Adding a third player drops GDS by 3%, a fourth by another 4%. But look at the depth ratio — it nearly *doubles* from 45% to 70%.

### The Efficiency Metric

The raw GDS comparison is misleading. In a 2-player game, you win ~50% of the time. In a 4-player game, ~25%. You're generating almost the same total engagement from half the winning probability.

To capture this, we define **engagement efficiency**: GDS divided by win probability.

| Players | P(win) | GDS | **GDS / P(win)** |
|:---:|:---:|:---:|:---:|
| 2 | 0.432 | 0.443 | **1.02** |
| 3 | 0.333 | 0.429 | **1.29** |
| 4 | 0.250 | 0.411 | **1.64** |

Each unit of winning probability generates 60% more anticipation in a 4-player game than a 2-player game. Multiplayer is structurally more efficient at producing engagement.

## Why? Elimination Phase Transitions

The 3-player game reveals the mechanism. We can split the analysis by game phase:

| Phase | States | Avg A₁ | Avg A₂ |
|:---|:---:|:---:|:---:|
| 3-alive | 27 | 0.127 | **0.163** |
| 2-alive | 27 | **0.153** | 0.102 |

The 3-alive phase has *lower* immediate excitement (A₁) but *higher* strategic depth (A₂). Why? Because when three players are alive, the uncertainty is more diffuse — you're not just tracking one opponent, you're tracking the dynamics of who eliminates whom. This creates nested layers of "what could happen next" that the deeper anticipation components capture.

When a player gets eliminated, the game collapses to a simpler 2-player structure with higher A₁ (more direct uncertainty) but lower A₂. The phase transition itself — the moment of elimination — is an additional source of anticipation that pure 1v1 doesn't have.

## The Focus Fire Disaster

We also tested a "target the leader" variant where all attacks focus on the highest-HP player:

| Strategy | GDS | Change |
|:---|:---:|:---:|
| Random pairing | 0.429 | — |
| Focus fire | 0.208 | **-51.5%** |

Focus fire cuts GDS in half. This is the largest negative effect we've seen in any experiment — worse than house edge in gambling, worse than comeback mechanics.

Why? Focus fire eliminates variance in the elimination order. The leader always gets knocked down first, creating a predictable pattern that collapses the uncertainty space. Random pairing maintains genuine uncertainty about *who* will be eliminated *when*, which is the engine of multiplayer depth.

## Design Implications

1. **More players = more depth, not just more chaos.** The phase transition structure of elimination games creates genuine strategic depth (A₂+) that 1v1 games can't access.

2. **Engagement efficiency increases with N.** This explains why Battle Royale games work — a 100-player game with <1% win rate still generates massive per-game engagement because the elimination phases are so deeply layered.

3. **Don't use focus fire / target-the-leader mechanics.** The single worst thing you can do for FFA engagement is make targeting predictable. Random or player-chosen targeting preserves the uncertainty that drives depth.

4. **Multiplayer depth comes from uncertainty about *others*.** In 1v1, you only track one opponent. In FFA, you track the entire dynamics of who-beats-whom — a combinatorially richer uncertainty space.

## The Multiplayer Efficiency Conjecture

Based on these results, we conjecture:

> **For symmetric N-player games with elimination, engagement efficiency (GDS/P(win)) increases monotonically with N.**

If true, this means multiplayer game design is fundamentally more "efficient" than 1v1 design — you get more engagement per unit of player investment. The challenge isn't generating enough anticipation (the math handles that), but managing the player experience across longer average time-to-win.

This connects to the [Unbound Conjecture](/toa-research/2026/02/12/unbound-conjecture-v2.html): adding players effectively adds depth without extending individual game length, which might be a new mechanism for unbounded growth.
