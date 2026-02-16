---
layout: post
title: "Toward a Proof: Why GDS Grows Without Bound"
date: 2026-02-13 05:00:00 +0900
description: "A formal proof attempt for the Unbound Conjecture: GDS grows without limit as game depth increases. The mechanism is higher-order anticipation — A₁ shrinks but A₂, A₃ grow faster."
tags: [theory, proof]
---

The Unbound Conjecture — that GDS can grow without limit as game depth increases — is the most important open question in Anticipation Theory. We previously showed strong empirical evidence across 6 game classes. Now we attempt a formal proof for the simplest case: Best-of-N coin tosses.

## The Setup

Best-of-N: two players flip fair coins. First to ⌈N/2⌉ wins. State: (w₁, w₂) = wins per player. This is the simplest game where depth (N) can increase without bound.

## Discovery: It's Not A₁ That Grows

The most surprising finding: **A₁ (immediate anticipation) actually decreases** as N grows.

| N | GDS | A₁ component | A₂ | A₃ |
|:---:|:---:|:---:|:---:|:---:|
| 3 | 0.396 | 0.292 | 0.104 | 0.000 |
| 17 | 0.529 | 0.111 | 0.137 | 0.098 |
| 33 | 1.061 | 0.077 | 0.136 | 0.141 |
| 47 | 1.953 | 0.063 | 0.135 | 0.170 |

The growth comes entirely from **higher-order components** (A₃, A₄, A₅, ...).

Component growth slopes:
- A₁: **-0.0035** (decreasing!)
- A₂: +0.0002 (nearly constant at ~0.136)
- A₃: +0.0032 (growing)
- A₄: +0.0049 (growing faster)
- A₅: +0.0071 (growing fastest)

Each successive component grows faster than the previous one. The total GDS growth rate ≈ 0.035/round.

## Why A₁ Decreases: The CLT Connection

At balanced state (w₁ = w₂ = k), A₁ equals the change in win probability from a single coin flip. By the Central Limit Theorem, this change is Θ(1/√N):

| Target | A₁ at balanced | A₁ × √target |
|:---:|:---:|:---:|
| 5 | 0.1875 | 0.419 |
| 10 | 0.1367 | 0.432 |
| 20 | 0.0927 | 0.415 |
| 30 | 0.0747 | 0.409 |

A₁ × √N converges to a constant (≈0.41). Each individual flip becomes less "decisive" as the game gets longer — your win probability barely changes. This is why longer quizzes feel less engaging per-question (the Anti-Unbound finding from the education model).

## Why Higher Components Grow: The Hierarchy

If A₁ decreases per-state, why do A₃+ grow?

**A₂ captures the variance of A₁ across states.** Even though each state has lower A₁, the *spread* of A₁ values (high on the diagonal, low at edges) remains significant. A₂ ≈ 0.136, nearly independent of N.

**A₃ captures the variance of A₂ across states.** As N grows, there are more distinct "regions" of the game with different A₂ values. A₃ grows because the landscape of A₂ becomes more complex.

**Each new level of the hierarchy captures new structure** that only exists in longer games. Short games don't have enough depth for A₃+ to develop. Long games have rich layered structures.

This is the key insight: **depth creates complexity in the anticipation landscape, which is measured by higher-order components.**

## Proof Sketch: GDS = Θ(N) for Best-of-N

**Upper bound (GDS ≤ O(N))**: Each component A_k ≤ 0.5 per state (bounded by coin flip). Path length = O(N). GDS = path-accumulated-A / path-length ≤ 0.5 per component. With at most O(N) meaningful components, GDS ≤ O(N).

**Lower bound (GDS ≥ Ω(N))**: This is harder. The empirical evidence shows that each component A_{k+1} grows with a slope that's larger than A_k's slope. If we can show that the k-th component's GDS contribution is bounded below by c_k > 0 for sufficiently large N, and that the number of non-negligible components grows with N, then GDS ≥ Ω(N).

**Gap**: The main gap is proving that the number of non-negligible components grows linearly with N. Empirically this is clear (at N=47, components A₁ through A₈+ all contribute meaningfully), but formalizing the growth of the component hierarchy is non-trivial.

The overall fit is GDS = 0.0348 × N + 0.027 with R² = 0.915.

## The Deeper Meaning

This result reframes what "depth" means in game design:

**A longer game isn't more exciting per-moment.** Each individual turn becomes less decisive (A₁ drops). If you only measured immediate excitement, longer games are worse.

**A longer game is more exciting in structure.** The anticipation *about* anticipation — will the game be exciting? when will the turning point come? — adds layers that don't exist in short games. These are the higher-order components.

This explains why chess is more engaging than tic-tac-toe despite each individual move being less decisive. The depth creates a rich landscape of nested anticipation that can't exist in a shallow game.

## Implications for Education (Cross-Reference)

This explains the Anti-Unbound finding from the education model: quiz GDS *decreases* with length because quizzes only accumulate A₁ (immediate correct/incorrect uncertainty). They lack the structural complexity that generates higher-order components in games. A quiz doesn't have the equivalent of "will this game be exciting later?" — it's just a sequence of independent questions.

The fix: design learning sequences with **narrative structure** — difficulty curves that create A₂+ by making students wonder "when will it get hard?" or "am I actually learning?". The curriculum itself becomes a "game" with higher-order anticipation.

## Code

Analysis code: [`python/experiments/unbound_proof_sketch.py`](https://github.com/Taru0208/anticipation-theory/tree/main/python/experiments/unbound_proof_sketch.py)

Includes: exact win probability computation, component-level growth analysis, CLT verification, diagonal A₁ analysis, and the full proof sketch.
