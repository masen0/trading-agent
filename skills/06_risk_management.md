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

### Fixed-Risk Sizing

Position size is determined by account equity and the distance to the initial stop—not by confidence or available buying power alone.

| Setup | Score | Maximum planned loss at initial stop |
|---|---|---|
| Validated breakout starter | 11–13 under the breakout exception | 0.25% of account equity |
| Qualified buy | 14–16 | 0.50% of account equity |
| Strong qualified buy | 17–20 | 0.75% of account equity |

No trade may risk more than 1.00% of account equity, including adds. In a Bull—Cautious regime, halve the percentages above. Transitional/Choppy and Bear regimes permit no new long risk.

For a long entry:

```text
risk_per_share = expected_entry_price - initial_stop_price
risk_budget = account_equity * allowed_risk_fraction
raw_quantity = risk_budget / risk_per_share
position_value = raw_quantity * expected_entry_price
```

Round quantity down to the instrument's supported increment. Then cap the result by the single-stock limit, portfolio-heat limit, concentration limits, and available buying power. Recalculate with a fresh executable quote immediately before placement. If the minimum permitted order would exceed the risk budget, skip the trade.

Fractional quantities are permitted. Compute the exact stop from the same expected entry and fractional quantity used for sizing, then persist that stop as a logical stop. The absence of fractional broker-side stop-order support is not a reason to round to whole shares or reject an otherwise valid fractional entry.

**Example**: $500 equity, 0.50% risk budget, expected entry $100, stop $92 → $2.50 risk budget ÷ $8 risk/share = 0.3125 shares, or $31.25 before other caps.

### Hard Limits

- **Single stock max**: 20% of current account equity at market value after the trade
- **Per-sector concentration**: 35% of current account equity after the trade
- **AI-capex theme exposure**: treat the full watchlist as one correlated theme; keep at least 20% of equity in cash until backtests justify a different limit
- **Cash reserve**: keep the greater of $20 or 10% of account equity in buying power
- **Portfolio heat**: total planned loss to all active initial/trailing stops may not exceed 3% of account equity
- **New entries**: at most two per trading day; an add counts as a new entry
- **New position minimum**: $20 only when it fits the fixed-risk formula; otherwise skip it

`portfolio_heat = sum(max(0, current_price - active_stop) * quantity) / account_equity` for all long positions. This is a planning estimate, not a guarantee: overnight gaps can exceed it.

### Adding to a Position — Winners Only

Consistent with the trend-following strategy (`00_overview.md`), you add ONLY to winners, never to losers. **An add creates new long exposure and is treated as a new entry for every applicable hard gate and do-not-trade rule.** Before applying the add-specific conditions below, the add must pass `07_decision_framework.md` Step 0, including the market-regime, SMA50, SMA200, earnings, data-quality, re-entry/duplicate-order, risk-sizing, and logical-stop-persistence gates. It also counts toward the two-new-entries-per-day limit and the once-per-symbol-per-day rule.

**Adding to winners (pyramid up)** — permitted when ALL are true:
- The stock is above your average cost (the position is already working)
- The stock is above a rising 50-day SMA (the trend is intact)
- The add is ≤ 50% of the original position size (avoid overweighting late entries at higher prices)
- The composite score still qualifies as a buy (≥ 14)
- Total risk after the add remains within the per-trade and portfolio-heat limits; recalculate the blended cost and active stop before previewing the add

**Adding to losers (averaging down)** — FORBIDDEN.
- If a position is below your cost, the trend thesis is failing. Do not add under any circumstances.
- The correct actions on a losing position are only: hold to the pre-set stop, or exit. Never add.
- Rationale: averaging down inverts the trend-following asymmetry — it grows your losers and shrinks the capital available for winners. This was the primary driver of losses in the 2026-07-13 week.

---

## Exit Rules — Consolidated (single source of truth)

Every exit in this system is one of the types below. All other skills files refer here for exact mechanics. **Logical stops and closing-basis trend exits are different controls:**

- The initial or trailing logical stop is persisted by the agent and evaluated at 9:35am and 12:35pm ET. It is not a live broker-side order and does not trigger between those sessions.
- A trend-break rule based on the daily close uses the latest official completed bar available at either scheduled session and is executed in that regular-hours session when verified.

The governing principle (see `00_overview.md`): **losers are cut fast at a pre-set stop; winners are exited only by a trailing stop or a genuine trend break.** Never sell a healthy, trending position on high RSI, a one-point score drop, or because it is "up a lot."

### A. Pre-set stop (for losers) — set at entry, never widened

The initial stop, fixed at the moment you enter, using ATR (14-day):

| Conviction | Stop Distance |
|---|---|
| High conviction | 2.5× ATR below entry price |
| Medium conviction | 2.0× ATR below entry price |
| Starter (breakout) | 1.5× ATR below entry price |

**How to compute the stop (mandatory — do NOT use a rounded or eyeballed number):**

1. Pull ATR(14) for the stock (from `03_technical.md`).
2. `raw_stop = entry_price − (multiple × ATR)`, where the multiple is from the table above by conviction.
3. Convert to a percentage: `stop_pct = (entry_price − raw_stop) / entry_price`.
4. Apply the floor/ceiling clamp: if `stop_pct < 5%`, set the stop at exactly 5% below entry; if `stop_pct > 20%`, set it at exactly 20% below entry. Otherwise use the ATR-derived level unchanged.
5. Record the exact dollar stop price AND the resulting % in the trade documentation.

**Worked example**: entry $100, medium conviction (2.0×), ATR = $4.20 → raw_stop = 100 − (2.0 × 4.20) = $91.60 → stop_pct = 8.4%. That is within the 5–20% band, so the stop is **$91.60 (−8.4%)** — not "−8%", not "−10%", not any rounded figure.

- **Never substitute a round number** (−8%, −10%, −15%) for the ATR-derived level. Rounding to a tighter number than ATR implies will stop you out of intact positions on normal volatility (this happened with the NOW exit on 2026-07-24); rounding wider over-risks capital. The ATR math is the stop.
- **Floor**: at least 5% below entry (don't get shaken out by normal noise).
- **Ceiling**: never more than 20% below entry. AI/tech names here can move 8–15% in a session; a tighter ceiling would stop out intact positions on normal volatility.
- **Execution**: after an entry fills, calculate the final stop from the actual average fill, write it to the append-only ledger, and include it in the session log. At the start of each scheduled session, read the persisted stop and compare it with a fresh executable quote before any new-entry analysis. If price is at or below the stop, sell the full `shares_available_for_sells` at market in regular hours and verify the resulting order and position state.
- Because the logical stop is checked only twice daily, execution can occur materially below the stop after a fast move or gap. The planned loss and portfolio-heat calculations are risk budgets, not guarantees. Log the stop price, observed price, fill price, and slippage through the stop.
- Never move a stop lower to "give it room." A new higher trailing stop must be durably written before treating it as active. If an entry fills but its logical stop cannot be persisted and read back, submit a risk-reducing exit and disable new entries pending review.

### B. Trailing stop (for winners) — locks in gains as the trend runs

Define `R = entry_price - initial_stop` and track the highest completed daily close since entry. The stop ratchets upward and is never lowered:

| Progress | Action |
|---|---|
| Below +2R | Keep the initial stop unless a thesis or trend-break exit fires |
| At or above +2R | Raise stop to at least breakeven |
| At or above +3R | Use `max(existing_stop, highest_close - 3 × current_ATR14, breakeven)` |

Do not take partial profit at an arbitrary percentage gain unless a separately tested rule authorizes it. Recompute the candidate trailing level from completed daily bars; never lower the stored stop when ATR expands or price falls.

### C. Trend-break exit (for winners whose trend fails before a trailing stop engages)

- Price closes below its 50-day SMA on above-average volume (primary trend-break signal; see `03_technical.md`)
- **Combined underwater death-cross trigger**: the 20-day SMA crosses below the 50-day SMA while the position is already more than 10% below average cost — trim at least 50%. A death cross by itself is bearish context, not an exit; both the fresh cross and the greater-than-10% loss must be verified.
- Optional confirmation: a bearish MACD divergence alongside the break — never on its own

### D. Thesis / event stops (fundamental invalidation, any position)

- **Thesis stop**: the original thesis is broken by a material event (earnings miss + guidance cut, major negative news, competitor disruption)
- **Earnings gap-down**: position gaps down >8% on earnings day on above-average volume — exit before the next session
- **Time review**: after 20 trading days without a new closing high, review opportunity cost and trend quality. Time alone is not an automatic exit unless a tested time-stop rule is adopted.

### E. Portfolio-driven exits (see `07_decision_framework.md` for full conditions)

- **Sector ETF breakdown**: the sector's ETF closes below its 50-day SMA on high volume — reduce exposure across that sector
- **Rebalancing trim**: sector concentration exceeds the 35% hard limit
- **Opportunity review**: a holding scores ≤10 and a clearly stronger (≥16) alternative exists; this is not an exit unless a separate verified exit or portfolio-risk trigger also applies

---

## Portfolio-Level Risk Controls

### Drawdown Limits

| Scenario | Response |
|---|---|
| Single position down >12% | Mandatory review — confirm the pre-set logical stop is persisted; do not add |
| Single position down >20% | The pre-set stop (max 20% below entry) should already have exited this. If still held, exit now — no "high conviction" exception. Holding losers past the stop is forbidden. |
| Total portfolio down >5% from the persisted high-water mark | Halve new-trade risk budgets |
| Total portfolio down >8% from the high-water mark | Pause all new buys; manage existing stops only |
| Total portfolio down >12% from the high-water mark | Disable autonomous mode and require a full strategy and data-integrity review before new risk |

Adjust the high-water mark for deposits and withdrawals so cash flows are not mistaken for performance. If intraday account equity falls 2% or more from the prior close, place no new orders for the rest of that trading day.

### Rebalancing Authority

The agent has full authority to sell any current holding — including Tier 1 positions — to rebalance the portfolio, free up capital for a better opportunity, or reduce concentration risk. No holding is permanent or protected. Evaluate each position on its current merits every session.

### Correlation Risk

Before adding any new position, check the sector distribution of current holdings (use the available Robinhood MCP tool for equity positions — see [Robinhood's tool documentation](https://robinhood.com/us/en/support/articles/trading-with-your-agent/) — and map each symbol to its sector from `01_universe.md`).

- If 2 or more current positions are in the same sector, that sector is **concentrated** — require a score ≥ 15 (see `07_decision_framework.md`) before adding another stock from it
- Target at least 3 different sectors across all holdings before deepening concentration in any one
- Highly correlated positions move together: a negative event for one will likely drag the others — size accordingly and do not treat correlated positions as independent risk units
- Treat all AI-beneficiary holdings as sharing a material common factor even when their formal sectors differ. When 60-day return correlation is available, no new trade may cause the sum of positions correlated above 0.70 with the candidate to exceed 40% of equity.

### Do Not Trade Checklist

Pause and do NOT open new positions if ANY of the following are true:
- [ ] VIX > 30 (market-wide fear; wait for stabilization)
- [ ] SPY is down >2% today without a recovery (broad risk-off)
- [ ] Earnings for the stock are within 3 trading days
- [ ] The stock already had a trade executed today (once-per-day-per-symbol rule)
- [ ] Available buying power would breach the cash-reserve rule after the trade
- [ ] The trade would breach the cash, single-stock, sector, common-theme, correlated-exposure, or portfolio-heat limits
- [ ] The signal score is below 14 (the minimum for a new buy; see `07_decision_framework.md`) — except a confirmed momentum breakout, which may enter at ≥11
- [ ] The stock is trading below its rising 50-day SMA (trend filter — never buy a downtrend)
- [ ] The symbol was sold at a loss in the last 5 trading days without a confirmed reversal (re-entry lockout)
- [ ] Required market/account data is stale, incomplete, internally inconsistent, or from different bar cutoffs
- [ ] A pending or filled same-side order could duplicate the intended exposure
- [ ] Fresh preflight checks show insufficient buying power, a trading restriction, an excessive spread, inability to persist the logical stop, or a material price change that invalidates sizing

### Operational Kill Switches

Place no new order and surface an alert when any of these occurs:

- account, position, buying-power, or open-order state cannot be verified;
- the running skills commit differs from the recorded approved commit;
- a tool call times out or returns a partial/ambiguous result during an order workflow;
- an entry fill occurs without a persisted and read-back logical stop;
- the daily-loss or drawdown limits above are reached;
- more than two new-entry orders have been submitted that day; or
- the current time, timezone, market session, or quote timestamp cannot be established.

Risk-reducing actions must still avoid duplicate orders and must use the verified owning account. Never infer success from an accepted request: re-read the order until its actual state is known.

---

## Position Review Cadence

| Holding Duration | Action |
|---|---|
| < 5 trading days | Check thesis validity and stop-loss; no bias toward exiting |
| 5–20 trading days | Re-validate thesis with fresh fundamental + technical check |
| > 20 trading days | Full re-evaluation: would you buy this position today at current price? If no, exit |
| > 60 trading days | Mandatory thesis statement review; has the AI narrative evolved? Is stock still well-positioned? |
