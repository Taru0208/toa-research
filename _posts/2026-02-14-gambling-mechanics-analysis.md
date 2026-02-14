---
layout: post
title: "Why Casinos Are Bad Game Designers: A ToA Analysis"
date: 2026-02-14 09:00:00 +0900
tags: [experiment, application, gambling]
---

Casino games are designed to maximize revenue, not fun. But how much worse are they *structurally*? We modeled roulette, slot machines, and blackjack through Theory of Anticipation and found that gambling mechanics score dramatically lower than even the simplest well-designed games.

## The Models

We implemented six gambling models:

| Game | Type | P(win) | Depth |
|:---|:---|:---:|:---:|
| Roulette (single #) | 1-turn, 1/37 | 2.7% | 1 |
| Roulette (red/black) | 1-turn, 18/37 | 48.6% | 1 |
| Slot machine | 1-turn, tiered | varies | 1 |
| Multi-spin slots | N-turn session | varies | N |
| Roulette session | N-turn session | 48.6% | N |
| Blackjack | Decision-based | varies | 2-10 |

All models follow the standard ToA protocol — states, transitions, terminal conditions, and intrinsic desire values.

## Finding 1: Gambling Has 2-5x Lower GDS Than Games

| Game | GDS | A₁ | A₂+% |
|:---|:---:|:---:|:---:|
| **CoinToss (fair 50/50)** | **0.500** | 0.500 | 0% |
| Roulette red/black | 0.500 | 0.500 | 0% |
| Roulette single # | 0.162 | 0.162 | 0% |
| Slot machine | 0.102 | 0.102 | 0% |
| **HpGame (5,5)** | **0.437** | 0.176 | **59.6%** |
| HpGame+Rage | 0.560 | 0.199 | 64.5% |
| Slots (5 spins) | 0.223 | 0.154 | 30.9% |
| Roulette (5 rounds) | 0.096 | 0.052 | 46.0% |
| Blackjack | 0.355 | 0.137 | 61.4% |

The key metric: **A₂+%** (strategic depth ratio). Games like HpGame derive 60% of engagement from higher-order anticipation — the feeling that your current situation has *implications* for future outcomes. Single-turn gambling has 0% depth by definition. Even multi-turn gambling starts from a much lower baseline.

## Finding 2: The House Edge Barely Matters

This was surprising. A 5% house edge (moving P(win) from 50% to 45%) reduces GDS by only **0.5%**.

| P(win) | GDS | Reduction |
|:---:|:---:|:---:|
| 50% (fair) | 0.5000 | — |
| 45% | 0.4975 | -0.5% |
| 40% | 0.4899 | -2.0% |
| 30% | 0.4583 | -8.3% |

The house edge isn't what makes gambling feel unsatisfying — it's the **payout structure**. Even a perfectly fair slot machine would have GDS = 0.102, far below a simple coin flip.

## Finding 3: Payout Asymmetry Destroys Engagement

The real engagement killer is asymmetric payouts. All of these games have the same expected value (0.5), but wildly different GDS:

| Game | E[Value] | GDS |
|:---|:---:|:---:|
| Fair coin (50/50, 1:1) | 0.50 | 0.500 |
| Lottery (5%, 10:1) | 0.50 | 0.218 |
| Lottery (1%, 50:1) | 0.50 | 0.100 |
| Reverse lottery (99%, tiny win) | 0.50 | 0.050 |
| 3-tier slot | 0.24 | 0.302 |

**A₁ = √(p·(1-p)·ΔD²)** for binary games. When p is extreme (1% or 99%), the variance drops — you're almost certain of one outcome, so there's no meaningful anticipation.

Casinos *choose* extreme payout ratios (35:1 roulette, 100x+ jackpots) because they're profitable, but this directly trades engagement for revenue.

## Finding 4: Blackjack Is the "Best" Casino Game

Blackjack stands out with GDS = 0.355 and 61.4% depth ratio — structurally similar to game-like systems. Why? Because the player makes *decisions* (hit or stand) that meaningfully affect outcomes.

This aligns with real-world player perception: blackjack is the casino game people describe as most "skillful" and engaging. ToA quantifies this intuition. Every other casino game we tested was dominated by A₁ with little strategic depth.

## Finding 5: Multi-Turn Gambling Grows, But From a Low Base

Like the Unbound Conjecture for games, gambling sessions show increasing GDS with depth:

| Roulette Rounds | GDS | A₂+% |
|:---:|:---:|:---:|
| 1 | 0.050 | 0% |
| 3 | 0.076 | 32.7% |
| 5 | 0.096 | 46.0% |
| 10 | 0.158 | 66.3% |

Roulette GDS grows at ~0.012/round — but starts from 0.050. HpGame (5,5) achieves 0.437 with a comparable state space (36 states vs 66 for 10-round roulette). Gambling needs many more rounds to generate the same engagement a well-designed game produces immediately.

## Finding 6: Near-Miss Is a Cognitive Trick, Not Structural Design

We tested slot machines where "near-miss" outcomes (2 matching + 1 close) were assigned small positive desire values (0.05 to 0.15). The effect on GDS was minimal: -2.1% to +5.4%.

Near-miss effects in real gambling are **psychological**, not structural. They exploit cognitive biases (the "almost won" feeling) rather than creating genuine anticipation. ToA correctly identifies them as structurally equivalent to losses.

## The Structural Diagnosis

The fundamental difference between gambling and game design:

| Metric | Games (HpGame) | Gambling (Roulette) |
|:---|:---:|:---:|
| GDS | 0.437 | 0.096-0.158 |
| A₁ source | probability uncertainty | probability uncertainty |
| A₂+ source | strategic implications | repetition only |
| Depth ratio | ~60% | 30-46% |
| House edge effect | N/A | <1% GDS reduction |
| Core problem | — | asymmetric payouts |

Games generate engagement through **structural depth** — each decision creates a cascade of future implications (A₂, A₃, ...). Gambling generates engagement almost entirely through **immediate uncertainty** (A₁), and even that is reduced by extreme payout ratios.

The one exception — blackjack — confirms the rule: add player decisions, and engagement metrics improve dramatically.

## Code

Models and tests: [`experiments/gambling_mechanics.py`](https://github.com/Taru0208/anticipation-theory/blob/main/python/experiments/gambling_mechanics.py)

100 tests total (35 new for gambling models, 65 existing).
