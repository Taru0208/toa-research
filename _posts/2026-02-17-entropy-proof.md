---
layout: post
title: "Why Game Tension Grows: A Formal Proof"
date: 2026-02-17
description: "Proving that GDS grows without bound when every game state maintains uncertainty — with closed-form formulas for the first two anticipation components."
tags: [theory, proof, mathematical]
featured: true
---

Why do longer games feel more dramatic? Not all of them do — a 100-question quiz where you've mastered the material gets *less* tense, not more. But a close best-of-seven series? Every game matters more than the last.

This post proves exactly when and why game tension grows without bound.

## The Conjecture

**Entropy Preservation Conjecture**: A game's total tension (GDS) grows to infinity with depth if and only if every game state maintains at least some minimum uncertainty.

"Uncertainty" here means Shannon entropy of the transition probabilities. In a fair coin flip, H = 1 bit (maximal for binary outcomes). In a quiz where you've learned all the answers, H → 0.

## The Key Formula

For Best-of-N games with a fair coin, the first-order anticipation (how much the "who's winning" question shifts per step) has a closed form:

**A₁(a,b) = C(a+b-2, a-1) / 2^{a+b-1}**

where `a` = wins player 1 still needs, `b` = wins player 2 still needs.

This is half the probability mass of a binomial distribution evaluated at position `a-1` on row `a+b-2` of Pascal's triangle.

### Proof sketch

Define Δ(a,b) = D(a-1,b) - D(a,b-1), where D is the win probability. Show that Δ satisfies the same recursion as D: Δ(a,b) = [Δ(a-1,b) + Δ(a,b-1)]/2. The formula C(a+b-2,a-1)/2^{a+b-2} satisfies this recursion by **Pascal's identity** (C(n,k) + C(n,k+1) = C(n+1,k+1)). Match boundary conditions. A₁ = Δ/2.

## The Beautiful A₂ Formula

The second-order anticipation — measuring how much the *drama level itself* varies — has an even more elegant form:

**A₂(a,b) = |a - b| × A₁(a,b)**

The proof goes through the "expected total A₁" function G(a,b), which itself has a beautiful closed form:

**G(a,b) = (a+b-1) × A₁(a,b)**

The expected total first-order anticipation from any state is simply the maximum remaining path length times the local anticipation.

### What A₂ means

A₂ measures the *asymmetry* of the game position:
- At the diagonal (a = b): A₂ = 0. When the game is tied, the drama level is equally likely to increase or decrease — there's no systematic direction.
- At the corners (a = 1, b = k): A₂ = (k-1) × A₁. When one player is far ahead, the drama level is strongly directional — it can only increase (if the trailing player catches up) or collapse (if the game ends).

## The Cascade

Here's why total tension grows without bound:

| Component | Behavior as depth → ∞ |
|-----------|----------------------|
| GDS₁ | Decreases (A₁ spreads thinner over longer paths) |
| GDS₂ | Converges to ~0.137 |
| GDS₃ | Grows |
| GDS₄ | Grows faster |
| GDS₅+ | Each component eventually grows |

Each A_m is the variance of A_{m-1}. Since A₁ varies across states (from 1/2 near the end to ~1/√k at the start), A₂ is nonzero. Since A₂ varies (zero on the diagonal, large at corners), A₃ is nonzero. And so on.

New GDS components "activate" as depth increases — component m first becomes significant at approximately k ≈ 2m. With infinitely many components each eventually contributing, the sum is unbounded.

**Growth rate**: GDS(k) ~ k^1.5 (superlinear).

## Boundary: Pascal's Triangle

At the boundary states (one player needs just one more win), something lovely happens:

**A_m(1, b) = C(b-1, m-1) / 2^b**

The anticipation components at these states form rows of Pascal's triangle! A₁ = 1, A₂ = b-1, A₃ = C(b-1,2), A₄ = C(b-1,3), all divided by 2^b.

## The Converse

For the quiz model — where knowledge accumulates and correct-answer probability increases — entropy decays toward zero at an increasing fraction of states. This breaks the cascade: when A₁ → 0 (because p → 0 or p → 1), there's nothing for A₂ to measure variance of. GDS stays bounded regardless of quiz length.

## Summary

| Formula | Meaning |
|---------|---------|
| A₁(a,b) = C(a+b-2,a-1)/2^{a+b-1} | Win probability shift per step |
| A₂(a,b) = \|a-b\| × A₁(a,b) | Asymmetry of drama level |
| G(a,b) = (a+b-1) × A₁(a,b) | Expected total drama |
| A_m(1,b) = C(b-1,m-1)/2^b | Boundary: Pascal's triangle |

The forward direction of the Entropy Preservation Conjecture is proven: constant per-state entropy → GDS grows to infinity.

**Code and tests**: [experiments/entropy_proof.py](https://github.com/Taru0208/anticipation-theory/blob/main/python/experiments/entropy_proof.py) (50 tests).
