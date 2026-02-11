---
layout: post
title: "Perspective Desire ≡ Naive Desire: A Proof"
date: 2026-02-12
tags: [theory, proof]
description: "The code uses a different desire formulation than the paper. Do they produce different results? A short proof shows they're mathematically equivalent."
---

While porting ToA to Python, I noticed a discrepancy between the paper and the code.

## The Discrepancy

**Paper (Naive Desire):** The desirability of a transition from state s to s' is simply D(s') — the absolute value of the destination.

**Code (Perspective Desire):** The desirability is D(s') - D(s) — the *change* in value relative to the current state.

Volume 2 of the theory introduces Perspective Desire as a distinct concept, suggesting it captures something different about player experience. But do the two formulations actually produce different GDS scores?

## The Proof

Anticipation is computed as:

```
A(s) = √( Σ P(s→s') · (D(s') - μ)² )
```

where μ = Σ P(s→s') · D(s').

For Perspective Desire, replace D(s') with D(s') - D(s):

```
A_perspective(s) = √( Σ P(s→s') · ((D(s') - D(s)) - μ')² )
```

where μ' = Σ P(s→s') · (D(s') - D(s)) = μ - D(s).

Expanding the squared term:

```
(D(s') - D(s)) - μ' = D(s') - D(s) - μ + D(s) = D(s') - μ
```

The D(s) terms cancel. We get:

```
A_perspective(s) = √( Σ P(s→s') · (D(s') - μ)² ) = A_naive(s)
```

This is a consequence of a basic property of variance: **Var(X - c) = Var(X)** for any constant c. Subtracting a constant from all values shifts the mean but doesn't change the spread.

## Empirical Verification

Just to be thorough, I computed both formulations across all 8 game models, all states, all anticipation components (A₁ through A₅):

**Perfect match in every case.** Zero numerical difference.

## What This Means

Perspective Desire is a useful *interpretive* framework — it helps think about player experience in terms of relative gains/losses rather than absolute positions. But it doesn't change any numerical results. The two formulations are mathematically identical for all ToA computations.

This simplifies the theory: there's no need to choose between formulations. Use whichever is more intuitive for the context.
