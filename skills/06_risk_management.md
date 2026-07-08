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

| Conviction Level | Max Position Size (of available buying power) |
|---|---|
| High conviction (score 16–20; see decision framework) | 40% of available buying power |
| Medium conviction (score 12–15) | 25% of available buying power |
| Low conviction (score 8–11) | 10% of available buying power |
| Below threshold (score < 8) | No trade |

**Example**: $200 buying power, medium conviction → max $50 trade. This matches the $50 increment used in the dip-buy ladder.

### Hard Limits

- **Single trade max**: Never spend more than 50% of available buying power on one trade
- **Cash reserve**: Always keep at least $20 in buying power after any trade
- **Per-sector concentration**: No more than 40% of total portfolio in any single sector
- **Single stock max**: No more than 25% of total portfolio in any single stock (at cost)
- **New position minimum**: $20 (below this, transaction costs and slippage are proportionally too large)

### Adding to Existing Winners vs. Losers

**Adding to winners (pyramid up)**:
- Only add if the stock is above your average cost AND above the 50-day SMA
- Limit adds to 50% of original position size (avoid overweighting late entries at higher prices)

**Adding to losers (averaging down)**:
- Only if ALL of the following are true:
  1. The original thesis is still intact (re-validate from fundamentals)
  2. The decline is due to market/sector weakness, not company-specific bad news
  3. The stock is not in a technical breakdown (still above 200-day SMA or clearly oversold RSI < 30)
  4. You have conviction to hold for at least 30 more days
- Maximum one averaging-down add per position

---

## Stop-Loss Rules

### Setting Stop-Losses

Use ATR (14-day) to set dynamic stop-loss distances:

| Conviction | Stop Distance |
|---|---|
| High conviction | 2.5× ATR below entry price |
| Medium conviction | 2.0× ATR below entry price |
| Low conviction | 1.5× ATR below entry price |

**Minimum stop distance**: Always at least 5% below entry (don't get shaken out by normal noise).
**Maximum stop distance**: Never more than 20% below entry. AI/tech stocks in this universe can move 8–15% in a single session; a 15% ceiling would stop out fundamentally intact positions on normal volatility.

### Stop-Loss Triggers (Execute Without Hesitation)

These are pre-decided rules — do not deliberate when they're hit:

1. **Price stop**: Stock closes below stop price (calculated at entry using ATR method above)
2. **Thesis stop**: Original investment thesis is broken by a material event (earnings miss + guidance cut, major negative news, competitor disruption)
3. **Time stop**: If a position is flat to down >5% after 20 trading days with no clear catalyst approaching, reassess and consider exiting to redeploy capital

### Trailing Stops for Winners

Once a position is up >20%:
- Move stop to breakeven (cost basis)

Once a position is up >40%:
- Trail stop at 15% below current price (locks in minimum 25% profit)

Once a position is up >75%:
- Consider taking 25–33% off the table (partial profit)
- Trail remaining with 20% trailing stop

---

## Portfolio-Level Risk Controls

### Drawdown Limits

| Scenario | Response |
|---|---|
| Single position down >12% | Mandatory review — re-validate thesis or exit |
| Single position down >20% | Exit unless extremely high conviction with clear catalyst near-term |
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
- [ ] The signal score is below 8 (see `07_decision_framework.md`)

---

## Position Review Cadence

| Holding Duration | Action |
|---|---|
| < 5 trading days | Check thesis validity and stop-loss; no bias toward exiting |
| 5–20 trading days | Re-validate thesis with fresh fundamental + technical check |
| > 20 trading days | Full re-evaluation: would you buy this position today at current price? If no, exit |
| > 60 trading days | Mandatory thesis statement review; has the AI narrative evolved? Is stock still well-positioned? |
