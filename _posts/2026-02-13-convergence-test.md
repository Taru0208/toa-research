---
layout: post
title: "Why Games Get More Fun But Quizzes Don't"
date: 2026-02-13 07:30:00 +0900
tags: [theory, experiment]
---

The [Unbound Conjecture]({% post_url 2026-02-12-unbound-conjecture-evidence %}) says GDS grows with depth — deeper games are more engaging. But the [education model]({% post_url 2026-02-13-education-model %}) found the opposite: longer quizzes are *less* engaging. What determines which direction?

## The Experiment

Three quiz models, identical structure, different probability rules:

**Model 1 — Normal Quiz**: Knowledge accumulates, increases P(correct). Standard learning.

**Model 2 — Chaotic Quiz**: Every question is a fair coin flip (50/50). Knowledge tracked but irrelevant to probability.

**Model 3 — Volatile Quiz**: Knowledge accumulates but has a 30% chance of loss each turn. Partial convergence.

## Results

| Model | GDS at 3q | GDS at 19q | Growth Rate | Direction |
|:---|:---:|:---:|:---:|:---|
| Normal | 0.241 | 0.189 | -0.003/q | **Shrinking** |
| Chaotic | 0.333 | 0.573 | **+0.015/q** | **Growing** |
| Volatile | 0.174 | 0.097 | -0.005/q | **Shrinking** |

The chaotic quiz — where each question is an independent coin flip — shows clear Unbound growth, just like games. The normal quiz shrinks. The volatile quiz, despite 30% knowledge loss disrupting convergence, *still shrinks*.

## The Structural Property

The dividing line is clean:

**Unbound** (GDS grows with depth): When each step's probability distribution is **independent of accumulated state**. Examples:
- Best-of-N coin tosses (each flip is 50/50)
- HP combat games (damage probabilities are constant regardless of current HP)
- The chaotic quiz (P(correct) = 50% always)

**Anti-Unbound** (GDS shrinks with depth): When accumulated state **converges** probability toward certainty. Examples:
- Standard quizzes (more knowledge → higher P(correct) → outcomes predictable)
- Skill-based sequences (practice → mastery → determinism)
- Even volatile systems (partial convergence still dominates)

## Why This Happens

In an independent-trial system, adding more trials creates new independent sources of uncertainty. The total trajectory has exponentially more possible paths. Higher-order anticipation components (A₃, A₄, ...) capture this structural complexity and grow without bound.

In a convergent system, adding more steps reduces uncertainty via the **law of large numbers**. After enough questions, the student's knowledge — and therefore their outcome — is nearly determined. New questions add no fresh uncertainty.

The critical insight: **even 30% volatility isn't enough to overcome convergence**. The volatile quiz still shrinks because knowledge-dependent probabilities still dominate the dynamics. Only *complete* independence (chaotic model) produces growth.

## Implications

This explains a real-world observation: **games get more engaging as they get longer, but tests don't**.

A chess game becomes more tense as it progresses — each move matters more as the position sharpens. But a 200-question exam is less engaging per question than a 10-question pop quiz, because after 50 questions, the outcome is effectively decided.

The principle generalizes: any system where accumulated state reduces future uncertainty will show Anti-Unbound behavior. This includes:
- Job performance reviews (more data → more predictable)
- Sports tournaments with many games (law of large numbers)
- Stock market analysis over long horizons

Systems that maintain independence — poker hands, individual combat rounds, daily stock returns — stay engaging.

## A Conjecture

> **Entropy Preservation Conjecture**: GDS(depth) → ∞ if and only if the conditional entropy of each step's outcome, given accumulated state, remains bounded away from zero as depth increases.

In other words: engagement grows without bound exactly when the system never "figures itself out."

## Code

Full experiment: [`python/experiments/convergence_test.py`](https://github.com/Taru0208/anticipation-theory/tree/main/python/experiments/convergence_test.py)

Three models, parameter sweeps across quiz lengths 3-19, with regression tests.
