---
layout: post
title: "Porting ToA to Python: 8 Game Models Verified"
date: 2026-02-12
tags: [experiment, implementation]
description: "Reimplementing the C++17 Anticipation Theory engine in Python, with Monte Carlo verification against all 8 reference games."
---

The original ToA implementation is written in C++17 using the module system (`.ixx` files) — which means MSVC-only, no Linux/ARM support. To experiment freely, I needed a portable implementation.

## The Port

The Python engine (`toa/engine.py`, 186 lines) implements the full analysis pipeline:

1. **State graph construction** from game model transitions
2. **Terminal value assignment** (win = 1, loss = 0, draw = 0.5)
3. **Backward propagation** of desire values (D₀)
4. **Recursive anticipation computation** (A₁ through Aₙ)
5. **Probability-weighted GDS calculation**

The key data structures:

```python
@dataclass
class StateNode:
    state: Any
    desire: float           # D₀ — expected win probability
    anticipation: list[float]  # [A₁, A₂, A₃, ...]
    transitions: list[tuple]   # [(prob, next_state), ...]

@dataclass
class GameAnalysis:
    gds: float              # Game Design Score
    components: list[float] # [GDS_A₁, GDS_A₂, ...]
    states: dict            # state → StateNode
```

## The 8 Reference Games

Each game model implements a simple protocol: define states, transitions, and terminal conditions.

| Game | States | GDS | Key Metric |
|------|--------|-----|------------|
| CoinToss | 3 | 0.500 | A₁ theoretical max |
| RPS | 4 | 0.471 | Three-outcome ceiling |
| HpGame | 25 | 0.430 | Baseline 1v1 combat |
| HpGame_Rage | 75 | 0.544 | +26% with crit/rage |
| GoldGame | 14 | 0.369 | Economic competition |
| GoldGame_Critical | 20 | 0.348 | Steal mechanic (lower!) |
| TwoTurnGame | 4 | varies | Parameter optimization |
| LaneGame | 50+ | 0.540 | MOBA simulation |

Every value matches the C++ reference implementation to 3+ decimal places.

## Monte Carlo Verification

Beyond matching the C++ numbers, I built an independent verification system: simulate 10,000 random games and compute GDS empirically.

```python
def simulate_gds(game, n_games=10000):
    """Play n random games, track A₁ at each visited state."""
    # For each game: random walk from start to terminal
    # Record: which states visited, with what probability
    # Compute: empirical anticipation at each state
    # Average: weighted by visit frequency
```

Results for CoinToss, HpGame, and HpGameRage all converge to the exact values within ±0.01, confirming the engine is correct.

## Surprising Finding: GoldGame_Critical

Adding a "steal" mechanic to GoldGame (critical hits steal opponent's gold) *decreases* GDS from 0.369 to 0.348.

This is counterintuitive — more mechanics should mean more engagement, right? Not necessarily. The steal mechanic creates catch-up dynamics that *reduce* the variance of future trajectories. In ToA terms: it compresses A₂+ components.

The lesson: **not all complexity adds engagement**. Mechanics that reduce outcome variance can actually make a game less interesting, even if they "feel" more interactive. ToA catches this before playtesting would.

## What the Port Enables

With a portable Python implementation, I can now:

- Run experiments on any machine (no MSVC required)
- Iterate on game models in minutes instead of hours
- Use genetic algorithms and optimization libraries
- Generate data for visualization and analysis

The next posts cover what happened when I started pushing beyond the reference games.

## Code

The full implementation is at [Taru0208/anticipation-theory](https://github.com/Taru0208/anticipation-theory) — `python/` directory. 35 tests, all passing.
