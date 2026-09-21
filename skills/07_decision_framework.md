---
file: 07_decision_framework.md
purpose: How to synthesize all signals into a final buy/sell/hold decision
---

# Decision Framework

## Overview

After applying the hard eligibility, regime, data-quality, and risk gates, combine the four analysis domains into a ranking score, run the bull/bear debate, and execute only through the controlled order workflow below.

**Decision hierarchy**: Risk management overrides everything. A perfect signal score means nothing if executing the trade violates a hard risk limit.

## Step 0 — Hard Gates Before Scoring

A candidate is ineligible for a new long position or an addition to an existing position unless all are true:

1. The selected account and current account state are verified.
2. Instrument eligibility and liquidity checks in `01_universe.md` pass.
3. Data-quality and timestamp checks pass.
4. Market regime permits new long exposure.
5. Price is above a rising SMA50 and above SMA200.
6. Earnings are not within three trading days.
7. No re-entry lockout, duplicate-order condition, or operational kill switch applies.
8. A valid initial logical stop and fixed-risk position size can be computed within every portfolio limit, and the logical stop can be durably persisted and read back. Fractional quantity is allowed.

If a hard gate fails, record the reason and stop. Do not manipulate a score to compensate.

For an addition, these gates are mandatory **in addition to** every winner-only condition in `06_risk_management.md`. Passing the add-specific conditions never waives a Step 0 gate. An add counts as a new entry and as that symbol's one permitted trade for the day.

---

## Step 1 — Composite Signal Score

Each domain produces a score from 0–5. The composite is a **ranking aid**, not a probability forecast and not a substitute for risk sizing:

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

These weights are provisional. A fact may contribute to only one domain—particularly earnings, guidance, analyst reactions, and macro news. When reliable sentiment data is unavailable, use the neutral score defined in `04_sentiment.md` and lower confidence; do not invent evidence.

---

## Step 2 — Score Interpretation

| Score | Decision Zone | Default Action |
|---|---|---|
| 17–20 | **Strong Buy Candidate** | Eligible for the 0.75%-risk band, subject to every hard gate |
| 14–16 | **Buy Candidate** | Eligible for the 0.50%-risk band, subject to every hard gate |
| 11–13 | **Weak Buy / Hold** | Hold existing; no new buy |
| 8–10 | **Hold** | No action |
| 5–7 | **Weak / Review** | Review thesis and trend; score alone cannot change a stop or force a sale |
| 2–4 | **Exit Candidate** | Seek a verified mechanical or thesis exit trigger |
| 0–1 | **Thesis Failure Candidate** | Exit only after verifying the invalidating evidence or another hard exit trigger |

For existing positions, the score is diagnostic. Exits are governed by the logical stop, confirmed trend break, verified thesis invalidation, earnings-gap rule, or portfolio-risk controls in `06_risk_management.md`. This prevents a subjective score change from overriding a healthy trend.

---

## Step 3 — Bull/Bear Internal Debate

Before acting on any score above 13 (buy candidate) or below 7 (exit review), force yourself to argue the opposite case:

### For a BUY signal — argue the bear case:
- What is the strongest argument that this stock goes DOWN from here?
- Is there a catalyst that could make the thesis wrong in the next 30 days?
- What does the stock need to do to justify the current valuation? Is that realistic?
- Who is on the other side of this trade, and why might they be right?

### For a SELL/TRIM signal — argue the bull case:
- What is the strongest argument that this stock goes UP from here?
- Is the current weakness a temporary pullback within an intact uptrend, or a genuine trend break (price closed below a rising 50-day SMA)?
- Is the trend still structurally up despite the noise?
- Is this a mechanical stop/trend-break exit, or an emotional reaction to a red day?

(Note: this debate does not authorize *buying* weakness — the trend filter still forbids new entries below the 50-day SMA. It only governs whether to exit an existing position.)

**Rule**: If a material opposing case remains unresolved, do nothing. Uncertainty is not repaired by arbitrary half-sizing.

---

## Step 4 — Final Decision Matrix

After the bull/bear debate, apply:

| Situation | Action |
|---|---|
| Score ≥ 14 AND bull case clearly dominates AND all hard gates pass | Size from fixed dollar risk and proceed to order preview |
| Score 11–13 AND no material bear argument | Hold; no new buy |
| Score ≥ 14 BUT strong bear argument remains | Do nothing; unresolved material risk is not repaired by arbitrary half-sizing |
| Score ≤ 7 AND a verified exit trigger exists | Trim or exit according to `06_risk_management.md` |
| Score ≤ 7 BUT strong bull counter-argument | Do nothing; hold and re-evaluate next session |
| Stop-loss triggered | Exit regardless of score |
| Do-not-trade checklist triggered | No trade regardless of score |

---

## Step 5 — Trade Documentation (Required for Every Trade)

Before placing any order, write (for the email log):

```
Trade: [BUY/SELL] $[amount] of [TICKER]
Score: [X]/20 (F:[x] T:[x] S:[x] N:[x])
Skills commit / model: [commit] / [model]
Data cutoff: [timestamp and market session]
Thesis: [1–2 sentences why this trade makes sense]
Bull case: [strongest supporting argument]
Bear case: [strongest opposing argument — why it's still right to act]
Invalidation: [what would make this trade wrong — what would trigger an exit]
Stop-loss: $[price] ([X]% below entry — computed via the ATR method in skills/06, NOT a rounded number; show the ATR value used)
Risk budget: $[amount] ([X]% of equity)
Portfolio heat before/after: [X]% / [Y]%
Order type/session: [type] / [market session]
```

This documentation must be written to the append-only ledger before order placement. Update it afterward with preflight results, idempotency reference, broker order ID, fills, fees/slippage, the active logical-stop value, and verified final state.

## Step 6 — Controlled Order Workflow

For any real order:

1. Re-read account, position, buying power, open orders, and current quote.
2. Recompute size from the fresh executable quote and active logical stop; round down to the supported fractional increment and verify that the stop can be persisted.
3. In pre-production `shadow`, an order-review tool may be used because no placement follows. In production `autonomous`, do not call a review tool whose contract requires a new interactive confirmation; instead complete all documented preflight checks and abort if any hard gate or risk limit fails.
4. Generate one unique idempotency reference for the logical order and reuse it only for retries of that same order.
5. In production `autonomous`, place the order without per-trade human approval only when the approved skills commit matches and all gates pass.
6. Re-read the broker order until its actual state is known. "Accepted" is not "filled."
7. After an entry fill, recompute the logical stop from the actual average fill, persist it, and read it back as required by `06_risk_management.md`.
8. Reconcile positions, buying power, open orders, and the ledger. Any mismatch triggers the operational kill switch.

---

## Step 7 — Post-Session Learning and Change Governance

Once a week (Fridays), before sending the trading log, add a **Weekly Review** section to the email:

1. Which trades from this week were profitable? What signal patterns led to them?
2. Which trades lost money? Was the signal score high at entry? What was missed?
3. Are there any recurring patterns in what's working vs. not?
4. Which outcomes came from signal quality, sizing, execution, market regime, or randomness?
5. Did the strategy outperform SPY, QQQ, and an equal-weight buy-and-hold benchmark after estimated slippage?
6. Are any rule changes worth proposing for research?

Track at minimum: time-weighted return; benchmark-relative return; maximum drawdown; win rate; average win/loss in `R`; expectancy; exposure; turnover; slippage; gap losses; and results by regime and entry type.

The live agent may **propose** a skills change but may never edit, approve, or deploy its own live rules. Each change requires:

1. a written hypothesis and the exact rule diff;
2. enough relevant observations to justify testing—normally at least 30 closed trades, not one week of anecdotes;
3. backtesting with point-in-time data, delisted names where applicable, splits, realistic spreads/slippage, and no look-ahead leakage;
4. an untouched out-of-sample or walk-forward evaluation;
5. shadow operation before live promotion; and
6. explicit human approval of the versioned skills commit before it is promoted to autonomous production.

Never optimize a rule on the same trades used to discover the pattern and then report that result as validation.

---

## Quick Reference — Decision Triggers Summary

**New-position eligibility** (all must be true):
- Score ≥ 14
- **Trend filter (non-negotiable)**: the stock itself is above its own *rising* 50-day SMA. Never buy a stock trading below its 50-day SMA, regardless of score — that is a downtrend, and buying it is the falling-knife / averaging-down mistake. A strong fundamental story does NOT override a broken trend.
- **Re-entry lockout**: if this symbol was sold at a loss within the last 5 trading days, do not re-buy unless price has reclaimed the 50-day SMA on above-average volume OR shows a confirmed bullish RSI divergence.
- The deterministic market-regime rules permit new long exposure
- No earnings within 3 days
- Symbol not already traded today
- Risk checks pass (buying power, concentration limits)
- **Exception — momentum breakout**: if a completed daily close exceeds the highest adjusted close of the prior 252 completed sessions and completed-day volume is ≥1.5× the median of the prior 30 sessions, the score threshold drops to ≥11 and risk is capped at 0.25% of equity. All other hard gates still apply.

**Exit triggers** (any one sufficient after its required evidence is verified):

*Exit discipline: losers are cut fast at a pre-set stop; winners are exited only by a trailing stop or a genuine trend break. Do NOT sell a healthy, trending position because RSI is high, it's "up a lot," or the score slipped a point — that is cutting a winner early and inverts the strategy's asymmetry. The exact mechanics of every exit type below are defined in one place — see "Exit Rules — Consolidated" in `06_risk_management.md`.*

- **Stop-loss breached**: initial stop set at entry using ATR method in `06_risk_management.md`
- **Trailing stops** (see `06_risk_management.md`): initial stop below +2R, at least breakeven from +2R, and a non-decreasing 3×ATR trail from +3R
- **Earnings gap down**: position gaps down >8% on earnings day on above-average volume — trim or exit before the next session; do not hold through the subsequent drift expecting recovery
- **Combined underwater death-cross trigger**: apply the exact verified conditions and minimum 50% trim defined in `06_risk_management.md`; a death cross alone is not an exit
- **Sector ETF breakdown**: the stock's sector ETF (SMH for semis/memory, XLK for software/mega-cap, XLC for comms) closes below its 50-day SMA on volume >1.5× average — reduce exposure to all holdings in that sector; this signals a sustained rotation, not a one-day event
- Thesis-breaking news event (earnings miss + guidance cut, major competitive loss, regulatory action)
- Position held > 60 days and fundamental thesis no longer valid
- **Rebalancing trim**: sector concentration exceeds the 35% hard limit — reduce the lowest-ranked exposure only after verifying tax, order, and trend context; a new opportunity is not required to correct a breach

**Review triggers** (not sufficient by themselves to sell):

- Score drops to ≤5 on a position held for more than 5 days
- A current holding scores ≤10 while an eligible alternative scores ≥16

For either review, sell only when the holding also has a verified exit or portfolio-risk trigger.

**Default HOLD / no new action**:
- Score 8–13 with no stop-loss breach
- Mixed signals with no dominant direction
- High-uncertainty environment (VIX > 30, SPY down >2%)
- **A winning position in an intact uptrend** — hold it; do not trim on high RSI or a minor score dip
- **Trendless / choppy market** (SPY oscillating around its 50-day SMA) — default to no action; trend-following has no edge without a trend, and forcing trades here produces whipsaw losses
