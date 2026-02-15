---
layout: post
title: "Game Design Calculator: ToA Metrics in Your Browser"
date: 2026-02-16
description: "A browser-based tool that computes GDS, PI, CPG, and CFS for any game model — no installation needed."
tags: [tool]
---

The [Game Design Calculator](/toa-research/calculator/) is now live. It's a complete port of the Python ToA analysis engine to JavaScript, running entirely in your browser.

## What It Does

Pick a game model, adjust the parameters, and see how the metrics change in real time:

- **GDS** (Game Design Score) — how much tension the game structure creates
- **wGDS** — perceptually weighted tension (accounts for human attention decay)
- **PI** (Policy Impact) — how much player choices matter
- **CPG** (Choice Paradox Gap) — whether the fun strategy also wins
- **CFS** (Composite Fun Score) — all of the above combined into one number

## Available Models

1. **Coin Toss** — the simplest possible game (GDS = 0.5 exactly)
2. **Best-of-N** — coin flip series, shows how anticipation builds across rounds
3. **HP Combat** — two players trade hits with three equally likely outcomes
4. **Asymmetric Combat** — each turn hurts ONE player, producing ultra-high GDS that grows superlinearly with HP
5. **Tactical Combat** — Strike/Heavy/Guard with adjustable damage, hit rates, and counters. The only model with player choices, enabling PI and CPG analysis.

## Validated Against Python

The JS engine was validated against the Python reference implementation with 20 tests covering all 5 game models. Every GDS component matches to 6 decimal places.

## Try It

**[Open the Calculator](/toa-research/calculator/)**

Interesting things to explore:
- In **Tactical Combat**, increase Heavy Damage to 3 and Heavy Hit Chance to 0.7 — watch CPG drop toward zero
- In **Asymmetric Combat**, increase HP from 2 to 7 and watch GDS grow faster than linearly
- In **Best-of-N**, compare N=3 vs N=15 — more rounds means deeper anticipation layers
