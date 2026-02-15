---
layout: post
featured: true
title: "The Comeback Paradox: Why Designed Reversals Kill Fun"
date: 2026-02-14 15:00:00 +0900
tags: [experiment, game-design, paradox]
---

Game designers love comeback mechanics — rubber-banding in racing games, revenge meters in fighting games, blue shells in Mario Kart. The intuition seems obvious: reversals are exciting, so making them happen more often should make games more engaging.

ToA says otherwise.

## The Experiment

We built **CoinDuel**, a turn-based resource management game where two players wager coins each round (1-3 from a bank of 5), flip them, and whoever gets more heads wins the round. First to 3 round wins takes the game.

Then we added a "desperation bonus" — when you lose a round, you get extra coins in your bank. This should create more comebacks. More drama. More fun.

**It didn't.**

## Results

| Variant | GDS | A₁ | A₂+ | Depth Ratio |
|:---|:---:|:---:|:---:|:---:|
| CoinDuel (base) | **0.404** | 0.218 | 0.187 | 46% |
| + Desperation bonus 1 | 0.385 | 0.215 | 0.170 | 44% |
| + Desperation bonus 2 | 0.380 | 0.211 | 0.168 | 44% |
| + Desperation bonus 3 | 0.379 | — | — | 44% |
| + Desperation bonus 4 | 0.380 | — | — | 44% |

Every level of comeback bonus *reduced* GDS. The base game with zero artificial comebacks is the most engaging version.

## Why This Happens

The math reveals the mechanism. GDS comes from **uncertainty about outcomes** — the probability swings between winning and losing states. Comeback mechanics reduce GDS by:

1. **Flattening consequences.** When losing is softened (you get bonus coins), the gap between "ahead" and "behind" narrows. The d_global values across states become more uniform.

2. **Reducing A₂+ anticipation.** The higher-order anticipation components (A₂, A₃, ...) depend on *cascading uncertainty* — how today's uncertainty affects tomorrow's uncertainty. When the loser gets compensated, future states become more predictable, dampening the cascade.

3. **Compressing the probability space.** In the base game, wagering 3 coins while your opponent has 1 gives you a significant edge (71/29 after draw resolution). The desperation bonus narrows this asymmetry — the losing player quickly rebuilds their bank, making every round feel similar.

Put differently: the drama of a comeback requires the *real possibility of not coming back*. When you guarantee the comeback, you eliminate the anticipation that made it exciting.

## The General Principle

This connects to a deeper pattern we've seen across domains:

| System | Artificial Intervention | Effect on Engagement |
|:---|:---|:---|
| CoinDuel | Desperation bonus | **-6% GDS** |
| Education (Goldilocks) | Forced easy questions | Anti-Unbound |
| Gambling (slots) | Payout asymmetry | 2-5x lower than games |
| Trading (stop-loss) | Capping downside | +132% GDS (!) |

Wait — trading stop-losses *increase* GDS? Yes, but there's a crucial difference. A stop-loss doesn't soften the blow of losing; it **gives the player agency** to choose when to accept a loss. The player's *decision to stop* is itself a strategic layer that adds depth. Comeback mechanics, by contrast, remove agency — the bonus happens automatically.

**The distinction: agency-adding interventions increase GDS. Consequence-reducing interventions decrease it.**

## Implications for Game Design

1. **Don't rubber-band.** If your racing game speeds up trailing players, you're compressing the probability space. Let them catch up through skill or strategy instead.

2. **Make comebacks possible, not guaranteed.** CoinDuel's base version allows comebacks through resource management (save coins early, bet big later). This is earned, not given.

3. **If you must help the loser, add options, not bonuses.** Give them a new *choice* they can make (like trading's stop option) rather than automatically softening their position.

4. **Test with ToA.** The difference between "feels like it should work" and "actually works" is measurable. CoinDuel's desperation bonus *seemed* like a good idea until the math showed it wasn't.

## The CoinDuel Model

State: `(score1, score2, bank1, bank2)` where scores range 0-3 and banks 0-8.

Each turn, both players choose a wager (1 to min(3, bank)). All wagered coins are flipped. More heads wins the round. Draws re-roll.

Banks deplete by wager amount and refill by 1 per turn. The tension between aggressive wagering (high win chance now, empty bank later) and conservative play (save coins for when they matter) generates the game's strategic depth.

319 reachable states. Perfectly symmetric (d_global = 0.500 from initial state). 46% of GDS comes from A₂+ — comparable to HpGame, the reference multi-turn combat model.

The desperation variant gives the losing player extra coins after each lost round, expanding to 431 states but compressing the d_global distribution, producing a consistently lower GDS across all bonus levels tested.

## Code

The full model is available in the [anticipation-theory repository](https://github.com/Taru0208/anticipation-theory):

```python
from toa.engine import analyze
from toa.games.coin_duel import CoinDuel

result = analyze(
    initial_state=CoinDuel.initial_state(),
    is_terminal=CoinDuel.is_terminal,
    get_transitions=CoinDuel.get_transitions,
    compute_intrinsic_desire=CoinDuel.compute_intrinsic_desire,
    nest_level=10,
)
print(f"GDS: {result.game_design_score:.3f}")  # 0.404
```

All 152 tests pass, including 9 new tests specific to CoinDuel.

---

*Next: Taking CoinDuel from mathematical model to playable game. The design is validated — now we build.*
