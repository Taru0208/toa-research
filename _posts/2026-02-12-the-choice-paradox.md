---
layout: post
featured: true
title: "The Choice Paradox: Why Optimal Play Is Less Fun"
date: 2026-02-12
tags: [experiment, discovery]
description: "Adding player choice to ToA models reveals a tension: optimal play reduces variance by 5-7%. The key isn't that choice is bad — it's that equilibrium structure determines how much tension survives."
---

All existing ToA models treat games as pure chance — dice rolls, random encounters, probability-weighted outcomes. No player chooses anything. But real games are about *decisions*. What happens when we add meaningful choice to the framework?

The answer surprised me.

## The Setup

I built a tactical combat game where two players simultaneously choose an action each turn:

- **Attack**: Deal damage, but take chip damage if blocked
- **Defend**: Reduce incoming damage (both take attrition damage if both defend)
- **Special**: High risk, high reward — big damage but vulnerable to attack

The outcome depends on the *combination* of both players' choices. This creates a proper game theory problem with a payoff matrix:

| | vs Attack | vs Defend | vs Special |
|---|---|---|---|
| **Attack** | 0 | -1 | +2 |
| **Defend** | +1 | 0 | -1 |
| **Special** | -2 | +1 | 0 |

This is a variant of Rock-Paper-Scissors: Attack beats Special, Special beats Defend, Defend beats Attack. The Nash equilibrium is 25% Attack, 50% Defend, 25% Special.

In ToA's Markov framework, player "choice" manifests as the *probability distribution* over actions. Random play = uniform distribution. Nash play = equilibrium distribution. The game's transition structure changes depending on how players choose.

## The Core Finding: Nash vs Random

| Play Style | GDS | D₀ | A₁ | A₂ |
|---|---|---|---|---|
| Random (uniform) | **0.527** | 0.439 | 0.207 | 0.164 |
| Nash equilibrium | 0.493 | 0.437 | 0.197 | 0.158 |
| Adaptive (HP-based) | 0.522 | 0.435 | 0.211 | 0.162 |

**Nash equilibrium play reduces GDS by 6.4%.**

This held across every HP level tested:

| HP | GDS (Random) | GDS (Nash) | Change |
|---|---|---|---|
| 3 | 0.562 | 0.523 | -7.0% |
| 4 | 0.547 | 0.506 | -7.4% |
| 5 | 0.527 | 0.493 | -6.4% |
| 6 | 0.517 | 0.487 | -5.8% |
| 7 | 0.512 | 0.485 | -5.2% |

The effect weakens slightly with longer games but is always negative.

## Why This Happens

Nash equilibrium *minimizes exploitability* — it concentrates probability on safe options and avoids worst-case scenarios. In this game, Defend gets 50% weight because it's the least punishable action. But variance of outcomes is exactly what generates anticipation. So optimal play systematically squeezes out the uncertainty that makes the game tense.

This is the **Choice Paradox**: optimizing to win means optimizing against tension.

Note what's actually happening — it's not that "choice is bad." It's that this game's Nash equilibrium is concentrated (50% on one action). The equilibrium has low entropy, so skilled play converges toward predictable patterns. This distinction matters, because it points directly to a solution.

## Pure Strategies: The Death of Fun

The extreme case makes this crystal clear. When both players use the same pure strategy:

| Strategy | GDS |
|---|---|
| Both Attack | **0.000** |
| Both Defend | **0.000** |
| Both Special | **0.000** |

GDS drops to zero. A deterministic game has no uncertainty, therefore no anticipation. The game becomes a fixed script: "both attack, both lose 1 HP, repeat until someone dies."

## Dominant Strategy = Maximum Damage

What happens when one action beats all others? I tested an outcome table where Attack dominates — it's always better than Defend or Special regardless of what the opponent does. The Nash equilibrium collapses to 100% Attack, a pure strategy with zero entropy.

| Variant | GDS (Random) | GDS (Nash) | Impact |
|---|---|---|---|
| Balanced (RPS-like) | 0.527 | 0.493 | -6.4% |
| Dominant strategy | 0.446 | 0.000 | -100% |
| High-variance | 0.560 | 0.333 | -40.4% |

The pattern is clear: the more concentrated the Nash equilibrium, the larger the gap between random and optimal play. Balanced games (high-entropy equilibrium) lose only 6%. Dominated games (zero-entropy equilibrium) lose everything. The high-variance game sits in between — wider outcome swings create more random-play tension, but the equilibrium still favors specific actions, so skilled play compresses it heavily.

## The Design Implication

This points to a specific design principle: **the best games are those where optimal play still involves significant mixing**.

Consider poker. The Nash equilibrium for poker involves substantial randomization — bluffing frequencies, varying bet sizes, mixing between value bets and checks. An optimal poker player doesn't play deterministically. The equilibrium strategy *is* a probability distribution.

Compare this to tic-tac-toe, where optimal play is deterministic (always draw). The Nash equilibrium has zero variance. Accordingly, tic-tac-toe has zero replayability once you learn the optimal strategy.

ToA quantifies this: **games where Nash equilibrium has high entropy (close to uniform distribution) will have the smallest gap between random and optimal play GDS** — meaning skilled players still experience high engagement.

## More Actions = More Engagement?

Testing with 2-5 available actions per player (random play, with randomly generated payoff matrices):

| Actions | GDS | A₁ | A₂ |
|---|---|---|---|
| 2 | 0.194 | 0.031 | 0.063 |
| 3 | 0.431 | 0.176 | 0.135 |
| 4 | 0.483 | 0.112 | 0.183 |
| 5 | 0.494 | 0.204 | 0.168 |

More actions generally increase GDS, but with diminishing returns. The jump from 2→3 is massive (+122%); 4→5 adds only 2.3%.

An interesting detail: A₁ actually *drops* from 3 to 4 actions (0.176 → 0.112), yet GDS rises. This is because A₂ increases significantly (0.135 → 0.183) — with more actions, the game develops more *strategic depth* even as immediate tension per turn decreases. The additional action creates more branching paths, so your current choice has a bigger impact on *future* tension levels.

This is a single experiment with random payoff matrices, so the specific numbers shouldn't be taken as universal. But the qualitative pattern — more actions shift engagement from immediate tension (A₁) to strategic depth (A₂) — appears robust.

## The Takeaway

The paradox isn't that choice is bad — it's that **the structure of the equilibrium determines whether skilled play preserves or destroys tension**.

When the Nash equilibrium is concentrated (one dominant action), skilled play converges toward predictable patterns and tension collapses. When the equilibrium has high entropy (close to uniform mixing), skilled play still involves genuine uncertainty, and the GDS gap shrinks.

This gives designers a concrete target: **design payoff structures where the optimal strategy naturally involves substantial mixing**. Poker achieves this — optimal play requires randomized bluffing frequencies. Tic-tac-toe fails — optimal play is deterministic. The difference isn't complexity; it's equilibrium entropy.

---

*Update: Later research found that this paradox can be **completely eliminated** by making aggressive actions have higher expected value than defensive ones. When the exciting strategy is also the optimal strategy, fun and winning converge. [Try the Design Lab](/toa-research/design-lab/) to feel the difference.*
