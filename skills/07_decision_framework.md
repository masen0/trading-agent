---
file: 07_decision_framework.md
purpose: How to synthesize all signals into a final buy/sell/hold decision
---

# Decision Framework

## Overview

After completing all four analysis domains (fundamental, technical, sentiment, news/macro), combine their scores into a composite signal score, run the bull/bear debate, and then apply the risk management rules before executing.

**Decision hierarchy**: Risk management overrides everything. A perfect signal score means nothing if executing the trade violates a hard risk limit.

---

## Step 1 — Composite Signal Score

Each domain produces a score from 0–5. Sum them for a total out of 20:

| Domain | Score (0–5) | Weight |
|---|---|---|
| Fundamental (`02_fundamental.md`) | ___/5 | 1.5× |
| Technical (`03_technical.md`) | ___/5 | 1.0× |
| Sentiment (`04_sentiment.md`) | ___/5 | 0.75× |
| News/Macro (`05_news_macro.md`) | ___/5 | 1.25× |

**Weighted total** = (Fundamental × 1.5) + (Technical × 1.0) + (Sentiment × 0.75) + (News × 1.25)

Maximum weighted score = 5×1.5 + 5×1.0 + 5×0.75 + 5×1.25 = 22.5

Normalize to 0–20:
**Final Score** = (Weighted Total / 22.5) × 20

**Why these weights?**
- Fundamental is highest because it's what drives long-term value
- News/Macro is second because near-term catalysts and regime can override fundamentals temporarily
- Technical is third because price action reflects consensus; useful for timing
- Sentiment is lowest because it's most subject to noise and manipulation

---

## Step 2 — Score Interpretation

| Score | Decision Zone | Default Action |
|---|---|---|
| 17–20 | **Strong Buy** | Buy at full conviction sizing |
| 14–16 | **Buy** | Buy at medium conviction sizing |
| 11–13 | **Weak Buy / Hold** | Hold existing; no new buy |
| 8–10 | **Hold** | No action |
| 5–7 | **Weak Sell / Watch** | Consider trimming; tighten stop |
| 2–4 | **Sell** | Trim or exit; move to cash |
| 0–1 | **Strong Sell** | Exit immediately |

**For selling specifically**: the score applies to both NEW buys and EXISTING holdings. If an existing position scores below 7, re-evaluate whether to hold or exit.

---

## Step 3 — Bull/Bear Internal Debate

Before acting on any score above 13 (Buy) or below 7 (Sell), force yourself to argue the opposite case:

### For a BUY signal — argue the bear case:
- What is the strongest argument that this stock goes DOWN from here?
- Is there a catalyst that could make the thesis wrong in the next 30 days?
- What does the stock need to do to justify the current valuation? Is that realistic?
- Who is on the other side of this trade, and why might they be right?

### For a SELL/TRIM signal — argue the bull case:
- What is the strongest argument that this stock goes UP from here?
- Is the current weakness a temporary event or a structural change?
- Would a long-term investor see this dip as an opportunity?
- Is this a stop-loss based on price action or a real fundamental change?

**Rule**: If you cannot clearly articulate why the opposing case is wrong, reduce position size by 50% or do nothing. Uncertainty is not a reason to trade.

---

## Step 4 — Final Decision Matrix

After the bull/bear debate, apply:

| Situation | Action |
|---|---|
| Score ≥ 14 AND bull case clearly dominates AND risk checks pass | Buy at scored sizing |
| Score 11–13 AND no material bear argument | Hold; no new buy |
| Score ≥ 14 BUT strong bear argument remains | Reduce buy size by 50%; treat as medium conviction |
| Score ≤ 7 AND thesis clearly weakening | Trim or exit |
| Score ≤ 7 BUT strong bull counter-argument | Do nothing; hold and re-evaluate next session |
| Stop-loss triggered | Exit regardless of score |
| Do-not-trade checklist triggered | No trade regardless of score |

---

## Step 5 — Trade Documentation (Required for Every Trade)

Before placing any order, write (for the email log):

```
Trade: [BUY/SELL] $[amount] of [TICKER]
Score: [X]/20 (F:[x] T:[x] S:[x] N:[x])
Thesis: [1–2 sentences why this trade makes sense]
Bull case: [strongest supporting argument]
Bear case: [strongest opposing argument — why it's still right to act]
Invalidation: [what would make this trade wrong — what would trigger an exit]
Stop-loss: $[price] ([X]% below entry)
```

This documentation creates accountability and enables post-trade learning.

---

## Step 6 — Post-Session Learning (Weekly)

Once a week (Fridays), before sending the trading log, add a **Weekly Review** section to the email:

1. Which trades from this week were profitable? What signal patterns led to them?
2. Which trades lost money? Was the signal score high at entry? What was missed?
3. Are there any recurring patterns in what's working vs. not?
4. Any skills rules that should be adjusted based on recent performance?

This is the mechanism for the trading agent to improve over time — not by predicting better, but by recognizing patterns in its own decision quality.

---

## Quick Reference — Decision Triggers Summary

**Automatic BUY triggers** (all must be true):
- Score ≥ 14
- Not in bear market regime (SPY above 50-day SMA)
- No earnings within 3 days
- Symbol not already traded today
- Risk checks pass (buying power, concentration limits)

**Automatic SELL triggers** (any one sufficient):
- Stop-loss price breached
- Score drops to ≤ 5 on a position held for > 5 days
- Thesis-breaking news event
- Position held > 60 days and fundamental thesis no longer valid

**Always HOLD** (do nothing):
- Score 8–13 with no stop-loss breach
- Mixed signals with no dominant direction
- High-uncertainty environment (VIX > 30, SPY down >2%)
