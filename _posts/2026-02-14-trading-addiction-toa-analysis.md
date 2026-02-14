---
layout: post
title: "Why Day Trading Is More Addictive Than Video Games"
date: 2026-02-14 12:00:00 +0900
tags: [experiment, discovery]
description: "ToA analysis reveals day trading achieves GDS 0.877 — double that of well-designed games. The stop option alone adds +132% engagement."
---

Day trading platforms proudly advertise "democratizing finance." But what they've actually built are some of the most engaging interactive systems ever designed — more engaging than many video games.

The Theory of Anticipation framework can explain exactly why.

## The Experiment

I modeled six financial instruments through the ToA framework:

| Model | GDS | A₁ | Depth | States |
|-------|-----|-----|-------|--------|
| Index Fund (buy & hold) | **0.100** | 0.100 | 0.0% | 21 |
| DCA (dollar-cost averaging) | **0.116** | 0.071 | 38.7% | 57 |
| Stock Picker (3 choices) | **0.357** | 0.334 | 6.4% | 27 |
| Day Trader (choices + stop) | **0.877** | 0.425 | 51.6% | 94 |
| Options Session (5 rounds) | **0.878** | 0.478 | 45.5% | 282 |

For comparison:
- HpGame (5v5 battle): **GDS 0.430**, depth 59.0%
- HpGame Rage (optimized): **GDS 0.544**, depth 63.4%

## Finding 1: Day Trading Beats Video Games

The Day Trader model achieves **GDS 0.877** — more than double the HpGame (0.430) and 60% higher than the rage-optimized version (0.544). This isn't because trading is "better designed" — it's because the structural elements of trading happen to maximize engagement.

## Finding 2: The Stop Option Is Everything

The biggest discovery: **the ability to stop trading at any time adds +131.6% to GDS**.

| Model | GDS |
|-------|-----|
| Forced Trader (must complete 5 trades) | 0.379 |
| Day Trader (can stop anytime) | 0.877 |
| **Difference** | **+131.6%** |

Why? The stop option creates a "double or nothing" dynamic at every step. After each trade, you face a real decision: take profits, or risk them for more? This is the same structure as the coin flip's "double or nothing" game — which has GDS 0.500 per decision — but embedded within each trading step.

## Finding 3: Agency Creates Illusion of Depth

Adding player choices to otherwise random markets creates massive GDS improvements:

- **No choices** (Index Fund): GDS 0.100
- **3 choices** (Stock Picker): GDS 0.357 (+259%)
- **Choices + stop** (Day Trader): GDS 0.877 (+782%)

But here's the crucial nuance: **most retail traders don't actually have edge**. The "choices" feel meaningful (aggressive vs conservative, when to stop) but the underlying market is essentially random for the average participant. ToA captures the *structural engagement* regardless of whether the skill is real.

## Finding 4: The Addiction Spectrum

Placing all models on a single spectrum:

```
Index Fund ← DCA ← Options ← Stock Picker ← HpGame ← Day Trading
  0.100      0.116   0.194     0.357          0.430     0.877

  boring                                               addictive
  no decisions    some decisions         many decisions + stop option
```

The progression from left to right adds:
1. **More decisions per session** (higher A₂+)
2. **Stop option** (double-or-nothing structure)
3. **Frequent feedback** (know outcome immediately)
4. **Compounding results** (past decisions affect future)

These are the same ingredients that make good games engaging. Trading platforms have stumbled onto optimal engagement design — not through game design theory, but through the natural structure of financial markets.

## Finding 5: Options Are Structurally Identical to Slots

A single options trade (GDS 0.194) has a payoff structure similar to slot machines (GDS 0.102):

- 60% chance: lose premium (worthless)
- 25% chance: break even
- 10% chance: moderate profit
- 4% chance: large profit
- 1% chance: jackpot

The asymmetric payoff kills A₁ the same way slots do. But an **options session** (GDS 0.878) transforms this into a high-engagement experience through repetition and the stop option — exactly like how multi-spin slots sessions are more engaging than single spins.

## Why This Matters

The ToA framework reveals a structural truth: **day trading is not just "like" gambling — it's more engaging than both gambling AND most games**. The combination of real choices (position sizing, timing, asset selection), immediate feedback, and the ever-present stop option creates an engagement machine.

This has implications for:
- **Regulation**: Trading apps use the same engagement mechanics as slot machines, but with added depth that makes them even more compelling
- **Design**: If you want to make a game as engaging as trading, add a meaningful stop option and compounding decisions
- **Self-awareness**: If you find yourself checking your portfolio 50 times a day, it's not because you love finance — it's because the structure of trading is optimized for exactly this behavior

## Technical Details

- 6 financial models, 43 tests (143 total), all passing
- Models: IndexFund, StockPicker, DayTrader, OptionsTrade, OptionsSession, DollarCostAverage
- Full experiment code: `experiments/investment_models.py`
- Key structural insight: stop option transforms any random process into a "double or nothing" game, which has maximum single-turn GDS (0.500)
