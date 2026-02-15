---
layout: page
title: How It Works
permalink: /about/
---

## The Problem: Fun vs. Winning

Think about a fighting game where you can either:
- **Attack** — exciting, risky, might miss
- **Defend** — boring, safe, slowly chips away at the opponent

In many games, **the boring safe strategy is mathematically the best strategy**. So players face a bad trade-off: play the fun way and lose, or play the boring way and win.

This is surprisingly common. It shows up in card games (hoarding resources beats spending them), shooters (camping beats rushing), strategy games (turtling beats aggression), and even board games.

---

## Measuring the Problem

[Anticipation Theory](https://github.com/akalouis/anticipation-theory) gives us two tools:

### 1. How exciting is this game? (Tension Score)

Imagine watching a basketball game. A 50-48 score with 2 minutes left is exciting. A 80-30 score is not.

**Tension Score** (technically called "GDS") measures how much the "who's winning?" question swings during a game. A game where the lead changes hands constantly has high tension. A game that's decided in the first few moves has low tension.

This is the *raw material* of fun — without tension, there's nothing to be excited about. But tension alone isn't enough. Watching dice roll can have extreme tension, but it's boring because you can't do anything about it.

<div id="tension-demo" style="background:#0c0e18; border:1px solid #1a2040; border-radius:8px; padding:14px 18px; margin:16px 0; font-family:'Segoe UI',system-ui,sans-serif; color:#c8ccd4;">
<div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:8px;">
<strong style="font-size:13px; color:#8098c0;">Try it: Best-of-5 Coin Flip</strong>
<span id="td-score" style="font-size:12px; color:#607090;">You 0 — 0 CPU</span>
</div>
<div style="position:relative; height:22px; background:#141828; border-radius:4px; overflow:hidden; margin-bottom:8px;">
<div id="td-bar" style="position:absolute;left:0;top:0;height:100%;width:50%;background:linear-gradient(90deg,#3060a0,#4080c0);border-radius:4px;transition:width 0.4s ease;"></div>
<div style="position:absolute;left:50%;top:0;width:1px;height:100%;background:#405060;"></div>
<span id="td-pct" style="position:absolute;right:6px;top:3px;font-size:11px;color:#a0b0d0;font-family:'Consolas','SF Mono',monospace;">50%</span>
</div>
<div style="text-align:center;">
<button id="td-btn" onclick="tdFlip()" style="background:linear-gradient(180deg,#1a3060,#102040);border:1px solid #3070c0;color:#c8ccd4;padding:6px 24px;border-radius:6px;font-size:13px;font-weight:600;cursor:pointer;">Flip</button>
<span id="td-result" style="margin-left:10px;font-size:12px;color:#607090;"></span>
</div>
<div id="td-msg" style="margin-top:8px;font-size:12px;color:#506080;text-align:center;min-height:16px;"></div>
</div>
<script>
(function(){
  var you=0,cpu=0,need=3,done=false;
  function wp(){var yr=need-you,cr=need-cpu;function r(a,b){if(a<=0)return 1;if(b<=0)return 0;return 0.5*r(a-1,b)+0.5*r(a,b-1);}return r(yr,cr);}
  function up(){var p=wp();document.getElementById('td-bar').style.width=(p*100)+'%';document.getElementById('td-pct').textContent=Math.round(p*100)+'%';document.getElementById('td-score').textContent='You '+you+' — '+cpu+' CPU';}
  window.tdFlip=function(){
    if(done){you=0;cpu=0;done=false;document.getElementById('td-btn').textContent='Flip';document.getElementById('td-result').textContent='';document.getElementById('td-msg').textContent='';up();return;}
    var h=Math.random()<0.5;
    if(h){you++;}else{cpu++;}
    document.getElementById('td-result').textContent=h?'Heads — you score!':'Tails — CPU scores.';
    document.getElementById('td-result').style.color=h?'#50c070':'#d06060';
    up();
    if(you>=need||cpu>=need){done=true;document.getElementById('td-btn').textContent='Reset';
    document.getElementById('td-msg').innerHTML=you>=need?'<span style="color:#50c070">You win!</span> Notice how the bar swung with each flip — that\'s tension.':'<span style="color:#d06060">CPU wins.</span> The swings you felt watching the bar — that\'s what tension measures.';
    }
  };
  up();
})();
</script>

### 2. Does the fun strategy win? (Fun-Strategy Gap)

This is the key insight: **a well-designed game rewards the exciting strategy**.

**Fun-Strategy Gap** (technically "CPG") measures how much your win rate drops when you play the exciting way instead of the safe way. A gap of 0 means the exciting strategy IS the best strategy — the ideal case. A high gap means you're punished for having fun.

---

## Fixing It

The surprising discovery: **you can often eliminate the Fun-Strategy Gap by adjusting the numbers**.

Same game, same rules, same choices — just different damage values, hit rates, or resource costs. When the exciting action has higher expected payoff than the safe action, fun and winning converge.

This is exactly what the [Design Lab]({{ '/design-lab/' | prepend: site.baseurl }}) demonstrates:
- **Version A**: Heavy attack does 2 damage at 50% hit rate. The safe Guard strategy wins.
- **Version B**: Heavy attack does 3 damage at 70% hit rate. The aggressive strategy wins.

Both games have the same structure. Only the numbers changed. But Version B feels fundamentally different to play.

---

## The Principle

> **When aggressive actions have higher expected value than defensive ones, the fun strategy and the winning strategy become the same thing.**

This works because:
1. Players naturally gravitate toward exciting actions
2. If those actions are also optimal, there's no tension between "what I want to do" and "what I should do"
3. The game feels *fair* — you're rewarded for playing well, not for playing boringly

The principle applies broadly:
- **Combat games**: Make attacks rewarding enough that aggression pays off
- **Resource games**: Make spending more powerful than hoarding
- **Strategy games**: Make expansion stronger than turtling
- **Multiplayer**: Encourage engagement over avoidance

---

## Going Deeper

<details markdown="1">
<summary>Technical Details</summary>

### Formal Definitions

**Anticipation Theory** was developed by Jeeheon (Lloyd) Oh. The mathematical framework defines:

- **A₁ (Local Anticipation)**: Variance of win probability change from a single state — how much the "who's winning" question shifts per turn.
- **A₂+ (Nested Anticipation)**: Variance of A₁ — how much the *drama level itself* varies across the game.
- **GDS (Game Design Score)**: Weighted sum `Σ w_k · A_k` capturing total tension across all nesting levels.

My extensions add:

- **PI (Policy Impact)**: How much a player's strategy choice affects their GDS experience. High PI = player agency matters.
- **CPG (Choice Paradox Gap)**: `GDS(fun_policy) - GDS(optimal_policy)` — the cost of playing excitingly.
- **CFS (Composite Fun Score)**: `wGDS × (1 + PI/GDS) × (1 - CPG)` — tension × agency × paradox penalty.

Key findings:
- CPG can be eliminated when aggressive actions have higher EV than defensive ones
- This works for **intrinsic variance** (dice, hit chances) but not **extrinsic variance** (opponent choices)
- A₁–A₅ cover >99% of GDS for all practical game designs
- Growth rate classifies games: SNOWBALL (increasing tension), DECAYING (front-loaded), SHALLOW (low depth)

### Resources

- [Original Theory (MIT License)](https://github.com/akalouis/anticipation-theory)
- [Extended Research + Python Implementation](https://github.com/Taru0208/anticipation-theory)
- [Research Log]({{ '/' | prepend: site.baseurl }})

</details>
