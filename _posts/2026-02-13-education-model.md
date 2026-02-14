---
layout: post
title: "Beyond Games: ToA Predicts Learning Engagement"
date: 2026-02-13 04:00:00 +0900
tags: [experiment, application]
---

Can Theory of Anticipation predict which quiz designs are most engaging? We modeled learning sequences as "games" and found surprising parallels — and divergences — with game design.

## The Model

A quiz becomes a Markov game:
- **State**: `(knowledge_level, question_index)`
- **Transitions**: correct answer (P = sigmoid(knowledge - difficulty)) → knowledge +1; wrong → stays
- **Terminal**: all questions answered
- **Desire**: reaching mastery threshold = 1.0

This maps directly to ToA's framework. Each question is a "turn," knowledge level tracks progress like HP, and passing the quiz is "winning."

## Finding 1: The Goldilocks Zone (P ≈ 50-80%)

Peak engagement occurs when the probability of passing is **50-80%**. Too easy (P > 95%) → no uncertainty → GDS drops. Too hard (P < 10%) → desire signal barely propagates → GDS collapses.

| Difficulty | P(pass) | GDS |
|:---:|:---:|:---:|
| 0 (trivial) | 99.6% | 0.125 |
| **1 (easy)** | **80.0%** | **0.526** |
| 2 (moderate) | 32.2% | 0.340 |
| 3 (hard) | 8.5% | 0.141 |
| 5 (impossible) | 0.4% | 0.025 |

**The sweet spot**: difficulty just below the student's level. Questions should feel achievable but not certain — exactly the "zone of proximal development" that Vygotsky described in 1978. ToA arrives at the same conclusion from pure mathematics.

## Finding 2: Binary Pass/Fail Crushes Partial Credit

Binary scoring (pass/fail at threshold) generates **42-65% more engagement** than graduated scoring (proportional to knowledge gained).

| Mode | Binary GDS | Graduated GDS | Difference |
|:---|:---:|:---:|:---:|
| Ascending difficulty | 0.173 | 0.100 | -42% |
| Flat difficulty | 0.050 | 0.041 | -18% |
| Adaptive difficulty | 0.363 | 0.128 | **-65%** |

Why? Binary creates a **cliff**: states near the threshold have maximum variance in outcomes (pass vs fail). Graduated scoring smooths the cliff into a ramp — less dramatic, less engaging.

This maps to a real design tension: partial credit feels fairer, but pass/fail is more motivating. Certification exams, bar exams, driving tests — they're all binary for a reason.

## Finding 3: Forgetting Kills Engagement (-40%)

Adding knowledge decay (lose 1 level on wrong answers) **reduces GDS by 40%**. This surprised us — in game design, setbacks often add drama (rage mechanics increased HpGame GDS by 26-52% depending on configuration).

The difference: in games, setbacks create new possible paths (HP yo-yos). In learning, forgetting just extends the path without adding meaningful branching — the student needs to re-learn the same material, reducing net progress.

**Implication**: spaced repetition's value isn't in the forgetting itself — it's in the re-testing. The engagement comes from the uncertainty of "will I remember?", not from actually forgetting.

## Finding 4: Longer Quizzes Are Less Engaging (Anti-Unbound)

Unlike games where GDS grows linearly with depth (the Unbound Conjecture), quiz GDS **decreases** with length:

| Questions | GDS |
|:---:|:---:|
| 3 | 0.257 |
| 5 | 0.253 |
| 8 | 0.189 |
| 15 | 0.163 |
| 20 | 0.159 |

Why the reversal? In games, each turn adds independent uncertainty. In quizzes, more questions → **probability concentrates** around the expected outcome (law of large numbers). With 20 questions, the outcome is nearly predetermined by question 10.

**Design implication**: short, high-stakes assessments (3-5 questions) maximize engagement. This explains why pop quizzes feel more intense than final exams — it's not just anxiety, it's mathematically more uncertain.

## Finding 5: The Excitement Landscape

Mapping A₁ (immediate anticipation) across all states reveals a diagonal band of excitement:

```
Knowledge × Question:
         Q0    Q1    Q2    Q3    Q4    Q5    Q6    Q7
K=0    0.292 0.200 0.091 0.026    .     .     .     .
K=1      -   0.219 0.304 0.274 0.155 0.046    .     .
K=2      -     -   0.047 0.142 0.273 0.331 0.224    .
K=3      -     -     -   0.002 0.018 0.080 0.241 0.500
K=4 ←mastery  -     -     -     .     .     .     .
```

The peak excitement state: **K=3, Q=7** (A₁ = 0.500) — one question away from the end, one level below mastery. This is the education equivalent of (HP=1, HP=1) in combat games: everything rides on the next outcome.

The band follows the mastery threshold diagonally, confirming that **proximity to the pass/fail boundary** is what drives engagement.

## The Adaptive Difficulty Paradox (Mild)

Adaptive difficulty (d = student's current knowledge) gives each question P(correct) = 50% — maximum per-question uncertainty. But it only barely beats fixed easy difficulty (GDS 0.363 vs 0.363).

The paradox is subtle: adaptive maximizes micro-uncertainty (each question) but makes the macro-outcome predictable. Over 8 coin-flip questions, you'll almost certainly get ~4 right. There's no path to dramatically beating or missing expectations.

This parallels the Choice Paradox in games (Nash equilibrium reduces GDS by 5-7%), but the effect is much weaker because the quiz model lacks the zero-sum structure.

## Connections to Game Design

| Game Finding | Education Parallel |
|:---|:---|
| HP (1,1) is most exciting | Knowledge near threshold, final questions most exciting |
| Nash equilibrium reduces GDS | Adaptive difficulty slightly reduces GDS |
| Rage mechanics add +26-52% GDS | Forgetting mechanics reduce GDS by -40% |
| GDS grows with depth (Unbound) | GDS shrinks with length (Anti-Unbound) |
| Binary win/lose | Pass/fail > partial credit |

The core insight: **anticipation requires consequential uncertainty**. In games, depth creates new sources of uncertainty. In learning, length dilutes it. The optimal learning experience is short, high-stakes, and binary.

## Practical Implications

1. **Quiz design**: 3-5 questions, pass/fail, difficulty slightly below student level
2. **Gamification**: Binary achievements ("passed!") > progress bars
3. **Adaptive systems**: Don't perfectly match difficulty — keep it slightly easier than the student's level for maximum engagement
4. **Assessment length**: Shorter is more engaging. If you need comprehensive coverage, break into multiple short assessments rather than one long exam

## Code

Full experiment: [`python/experiments/education_model.py`](https://github.com/Taru0208/anticipation-theory/tree/main/python/experiments/education_model.py)

8 experiments, parameter sweeps across difficulty curves, adaptive modes, forgetting rates, success criteria, and quiz lengths.
