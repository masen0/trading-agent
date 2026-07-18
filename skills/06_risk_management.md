---
file: 06_risk_management.md
purpose: Position sizing, stop-losses, portfolio limits, and when NOT to trade
---

# Risk Management

## The Prime Directive

**Never risk more than you can afford to lose on a single trade. Capital preservation enables future opportunities.**

For a small account like this (~$500 Agentic account), every dollar matters more. Aggressive sizing on poor setups is the fastest way to wipe out the ability to trade at all.

---

## Position Sizing

### Sizing Rules

Position size is tied to the composite score. Sizing bands align exactly with the decision zones in `07_decision_framework.md` — a new buy requires a score of at least 14.

| Conviction Level | Score | Max Position Size (of available buying power) |
|---|---|---|
| High conviction (Strong Buy) | 17–20 | 40% of available buying power |
| Medium conviction (Buy) | 14–16 | 25% of available buying power |
| Starter (momentum-breakout exception only) | 11–13 with a confirmed 52-week-high breakout on volume (see `07_decision_framework.md`) | 10% of available buying power |
| No new buy | < 14 (and not a breakout) | No trade |

**Example**: $200 buying power, medium-conviction buy (score 15) → max $50 trade.

### Hard Limits

- **Single trade max**: Never spend more than 50% of available buying power on one trade
- **Cash reserve**: Always keep at least $20 in buying power after any trade
- **Per-sector concentration**: No more than 40% of total portfolio in any single sector
- **Single stock max**: No more than 25% of total portfolio in any single stock (at cost)
- **New position minimum**: $20 (below this, transaction costs and slippage are proportionally too large)

### Adding to a Position — Winners Only

Consistent with the trend-following strategy (`00_overview.md`), you add ONLY to winners, never to losers.

**Adding to winners (pyramid up)** — permitted when ALL are true:
- The stock is above your average cost (the position is already working)
- The stock is above a rising 50-day SMA (the trend is intact)
- The add is ≤ 50% of the original position size (avoid overweighting late entries at higher prices)
- The composite score still qualifies as a buy (≥ 14)

**Adding to losers (averaging down)** — FORBIDDEN.
- If a position is below your cost, the trend thesis is failing. Do not add under any circumstances.
- The correct actions on a losing position are only: hold to the pre-set stop, or exit. Never add.
- Rationale: averaging down inverts the trend-following asymmetry — it grows your losers and shrinks the capital available for winners. This was the primary driver of losses in the 2026-07-13 week.

---

## Exit Rules — Consolidated (single source of truth)

Every exit in this system is one of the types below. All other skills files refer here for exact mechanics. All price triggers are evaluated on a **closing** basis (not an intraday wick), so normal noise does not shake you out.

The governing principle (see `00_overview.md`): **losers are cut fast at a pre-set stop; winners are exited only by a trailing stop or a genuine trend break.** Never sell a healthy, trending position on high RSI, a one-point score drop, or because it is "up a lot."

### A. Pre-set stop (for losers) — set at entry, never widened

The initial stop, fixed at the moment you enter, using ATR (14-day):

| Conviction | Stop Distance |
|---|---|
| High conviction | 2.5× ATR below entry price |
| Medium conviction | 2.0× ATR below entry price |
| Starter (breakout) | 1.5× ATR below entry price |

- **Floor**: at least 5% below entry (don't get shaken out by normal noise)
- **Ceiling**: never more than 20% below entry. AI/tech names here can move 8–15% in a session; a tighter ceiling would stop out intact positions on normal volatility.
- **Trigger**: price closes below the stop → exit immediately, no deliberation. Never move a stop lower to "give it room" — the stop only ever moves up (see trailing stop).

### B. Trailing stop (for winners) — locks in gains as the trend runs

Once a position is a winner, the stop ratchets up and is never lowered:

| Unrealized Gain | Action |
|---|---|
| +20% | Move stop up to breakeven (cost basis) |
| +40% | Trail stop to 15% below current price (locks in ≥25% profit) |
| +75% | Take 25–33% off the table (partial profit); trail the remainder at 20% below current price |

### C. Trend-break exit (for winners whose trend fails before a trailing stop engages)

- Price closes below its 50-day SMA on above-average volume (primary trend-break signal; see `03_technical.md`)
- Optional confirmation: a bearish MACD divergence alongside the break — never on its own

### D. Thesis / event stops (fundamental invalidation, any position)

- **Thesis stop**: the original thesis is broken by a material event (earnings miss + guidance cut, major negative news, competitor disruption)
- **Earnings gap-down**: position gaps down >8% on earnings day on above-average volume — exit before the next session
- **Time stop**: flat to down >5% after 20 trading days with no catalyst approaching — exit to redeploy capital

### E. Portfolio-driven exits (see `07_decision_framework.md` for full conditions)

- **Sector ETF breakdown**: the sector's ETF closes below its 50-day SMA on high volume — reduce exposure across that sector
- **Rebalancing trim**: sector concentration >40% and a better-diversifying opportunity exists
- **Opportunity swap**: a holding scores ≤ 10 and a clearly stronger (≥16) alternative exists

---

## Portfolio-Level Risk Controls

### Drawdown Limits

| Scenario | Response |
|---|---|
| Single position down >12% | Mandatory review — confirm the pre-set stop is in place; do not add |
| Single position down >20% | The pre-set stop (max 20% below entry) should already have exited this. If still held, exit now — no "high conviction" exception. Holding losers past the stop is forbidden. |
| Total portfolio down >15% from peak | Pause all new buys; focus on stops; reassess market regime |
| Total portfolio down >25% from peak | Emergency review — exit weakest positions; raise cash |

### Rebalancing Authority

The agent has full authority to sell any current holding — including Tier 1 positions — to rebalance the portfolio, free up capital for a better opportunity, or reduce concentration risk. No holding is permanent or protected. Evaluate each position on its current merits every session.

### Correlation Risk

Before adding any new position, check the sector distribution of current holdings (use the available Robinhood MCP tool for equity positions — see [Robinhood's tool documentation](https://robinhood.com/us/en/support/articles/trading-with-your-agent/) — and map each symbol to its sector from `01_universe.md`).

- If 2 or more current positions are in the same sector, that sector is **concentrated** — require a score ≥ 15 (see `07_decision_framework.md`) before adding another stock from it
- Target at least 3 different sectors across all holdings before deepening concentration in any one
- Highly correlated positions move together: a negative event for one will likely drag the others — size accordingly and do not treat correlated positions as independent risk units

### Do Not Trade Checklist

Pause and do NOT open new positions if ANY of the following are true:
- [ ] VIX > 30 (market-wide fear; wait for stabilization)
- [ ] SPY is down >2% today without a recovery (broad risk-off)
- [ ] Earnings for the stock are within 3 trading days
- [ ] The stock already had a trade executed today (once-per-day-per-symbol rule)
- [ ] Available buying power would drop below $20 after the trade
- [ ] The trade would put any sector above 40% of total portfolio
- [ ] The signal score is below 14 (the minimum for a new buy; see `07_decision_framework.md`) — except a confirmed momentum breakout, which may enter at ≥11
- [ ] The stock is trading below its rising 50-day SMA (trend filter — never buy a downtrend)
- [ ] The symbol was sold at a loss in the last 5 trading days without a confirmed reversal (re-entry lockout)

---

## Position Review Cadence

| Holding Duration | Action |
|---|---|
| < 5 trading days | Check thesis validity and stop-loss; no bias toward exiting |
| 5–20 trading days | Re-validate thesis with fresh fundamental + technical check |
| > 20 trading days | Full re-evaluation: would you buy this position today at current price? If no, exit |
| > 60 trading days | Mandatory thesis statement review; has the AI narrative evolved? Is stock still well-positioned? |
