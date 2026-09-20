---
file: 06_risk_management.md
purpose: Sizing, hard stops, DEFINED profit targets, time stops, reward:risk — the survival scaffolding
---

# Risk Management

## The Prime Directive

**Capital preservation first.** Mean-reversion has a high win rate but occasional sharp losers (the dip that doesn't bounce). The entire edge is destroyed if one un-stopped knife-catch wipes out ten small wins. The stop is not optional — it is the strategy.

**KEY CONCEPT — mean-reversion's exit logic is the exact inverse of trend-following's.** Trend-following lets winners run and cuts losers; mean-reversion **takes profits at a defined target** (winners are *capped* by design) and cuts losers at a hard stop. Every position is defined by **three numbers set at entry: the entry, the target, and the stop.** No position exists without all three.

---

## The Three Numbers (set at entry, non-negotiable)

For every trade, before placing it, define:

1. **Entry** — at/near named support, on an oversold trigger.
2. **Target** — the reversion level: normally the 20-day SMA (band midline) or the nearest resistance, whichever is closer. This is where you *take profit*, mechanically.
3. **Stop** — just below the support that defined the entry, or 1.5× ATR below entry, whichever is tighter. If support breaks on a close, the thesis is void.

---

## Reward:Risk Gate

Compute before entry:
- `reward = target − entry`
- `risk = entry − stop`
- **Require reward:risk ≥ 1.5 : 1.** If the bounce target isn't at least 1.5× the downside to the stop, skip the trade — the math doesn't pay.

A high win rate at ≥1.5:1 is what makes the book profitable. Trades that don't clear this gate are noise.

---

## Position Sizing

| Setup Quality (composite, `07`) | Max Position Size (of buying power) |
|---|---|
| A-grade (score ≥ 16, RSI<25 + divergence + capitulation) | 30% |
| B-grade (score 13–15) | 20% |
| C-grade (score 10–12) | 10% |
| Below 10 | No trade |

Hard limits:
- **Single trade max**: 30% of buying power (higher variance strategy — smaller max than the trend book's 40%)
- **Cash reserve**: keep ≥ $20 buying power after any trade
- **Max concurrent positions**: cap at ~4–5 so each has a real stop and you're not diworsified
- **Sector cap**: no more than 40% of the portfolio in one sector

---

## Exit Rules — Consolidated (single source of truth)

All exits evaluated on a **closing basis** unless a hard intraday stop is breached. Manage open positions FIRST every session.

### A. Target hit (the primary exit) — TAKE THE PROFIT
- Price reverts to the target (20-SMA / resistance) → **sell, in full, mechanically.** Do not hold for more. Capturing the snap-back *is* the strategy.
- Optional: scale out — sell half at the 20-SMA, trail the rest with a tight stop to the upper band — but the default is a clean full exit at target.

### B. Hard stop (the loss-cutter)
- Price closes below the support-based stop (or the intraday hard stop is hit) → **exit immediately.** The reversion thesis is invalidated; there is no averaging down.

### C. Time stop (the dead-money cutter)
- A reversion bounce should happen quickly. If the trade has **not** reached its target within **5 trading days** and is not stopped, **exit and redeploy.** Mean-reversion that doesn't revert fast is usually wrong; capital tied up in a non-bouncer is capital not working.

### D. Thesis-flip
- If, while held, news turns the dip from technical to fundamental (a downgrade becomes a guidance cut, etc.), exit regardless of price.

---

## Absolute Prohibitions

- **No averaging down past the stop.** One entry per setup. Adding to a losing dip-buy is the martingale trap — it has no floor and is how the account goes to zero.
- **No holding past the target** hoping a bounce becomes a trend. Different strategy; take the win.
- **No dip-buying below the 200-day SMA** (`03` gate) — falling knife.
- **No new entries in a strong directional trend** (`00` regime) — reversion fails there.

---

## Inverse-Leveraged ETFs (tactical only)

If using SOXS/SQQQ/SPXU (see `01`):
- **Duration**: days, never weeks — they decay in chop.
- **Size**: half the notional you'd use for a normal position (they move ~3×).
- **Stop + time stop**: mandatory and tighter than for equities (a 2–3 day time stop).
- **Trigger**: a *sector* at an overbought extreme rolling over, or a defined hedge against a confirmed sector downdraft — not a standing short.

---

## Do-Not-Trade Checklist

Skip a new entry if ANY is true:
- [ ] Stock is below its 200-day SMA (falling-knife gate)
- [ ] The dip is fundamental (thesis-breaking news) — `05`
- [ ] Earnings within 3 trading days
- [ ] Reward:risk < 1.5:1
- [ ] Market is in a strong directional trend (reversion stands down)
- [ ] VIX > 30 / SPY down >3% with no reversal bar yet
- [ ] Buying power would drop below $20, or the sector would exceed 40%
