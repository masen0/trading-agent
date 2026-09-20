---
file: 07_decision_framework.md
purpose: Combine signals into a dip-buy decision with a defined target, stop, and minimum reward:risk
---

# Decision Framework

## Overview

Mean-reversion decisions are gated, then scored, then sized. **Order matters: the gates come first — a great score cannot rescue a trade that fails a gate.**

**KEY CONCEPT — this framework is built around a defined *trade structure* (entry / target / stop), not a directional opinion.** The output of a decision is never just "buy X" — it is "buy X at ~$E, target $T, stop $S, reward:risk R" or it is "no trade." If you cannot state the three numbers and clear the reward:risk gate, there is no trade.

---

## Step 1 — The Gates (all must pass, or STOP)

1. **Trend gate**: price above a flat/rising 200-day SMA (`03`). Below it → no trade.
2. **News gate**: the dip is *technical*, not a thesis-breaking event (`05`). Fundamental dip → no trade.
3. **Quality gate**: the business passes the light fundamental filter (`02`). Fail → no trade.
4. **Event gate**: no earnings within 3 trading days; no major macro print today.
5. **Regime gate**: market is range-bound or mildly trending, not a strong directional trend (`00`).

Only setups clearing all five gates are scored.

---

## Step 2 — Composite Score

| Domain | Score | Weight | Rationale |
|---|---|---|---|
| Technical (`03`) | 0–5 | **1.75×** | The setup itself — highest weight |
| Sentiment (`04`) | 0–5 | **1.25×** | Contrarian extreme confirms the reversion |
| News (`05`) | 0–5 | 1.0× | Confirms the dip is technical |
| Fundamental gate (`02`) | 0–2 | 1.0× | Light quality confirmation |

**Weighted total** = (T×1.75) + (Sent×1.25) + (News×1.0) + (Fund×1.0). Max = 8.75 + 6.25 + 5 + 2 = 22.
**Normalize to 0–20**: Final = (Weighted / 22) × 20.

Weights are deliberately technical- and sentiment-heavy — the opposite emphasis from the trend book, because on this timeframe the *setup and the emotional extreme* drive the edge, not the fundamentals.

---

## Step 3 — Structure the Trade (the three numbers)

Define, explicitly:
- **Entry** ≈ current price at/near support
- **Target** = 20-day SMA (or nearest resistance if closer)
- **Stop** = just below support (or entry − 1.5×ATR, tighter)
- **Reward:Risk** = (Target − Entry) / (Entry − Stop) — **must be ≥ 1.5:1** (`06`)

If reward:risk < 1.5, **no trade**, regardless of score.

---

## Step 4 — Decision & Sizing

| Final Score | Grade | Action (if all gates pass and R:R ≥ 1.5) |
|---|---|---|
| 16–20 | A | Buy at 30% BP |
| 13–15 | B | Buy at 20% BP |
| 10–12 | C | Buy at 10% BP |
| < 10 | — | No trade |

---

## Step 5 — Trade Documentation (required for every trade)

```
Trade: BUY $[amt] [TICKER] — mean-reversion dip-buy
Setup: RSI [x], [at/below lower band], support $[S], 200-SMA gate PASS
Dip cause: [technical reason] — news-clean confirmed
Score: [X]/20 (T:[x] Sent:[x] News:[x] Fund:[x])
Entry: $[E]  |  Target: $[T] (20-SMA/resistance)  |  Stop: $[S] (below support)
Reward:Risk: [R]:1   |   Time stop: 5 trading days
```

Record the target and stop **at entry** — they are not revisited except to *exit*.

---

## Step 6 — Managing the Open Trade

The exit is **reversion to the mean (the target), not the opposite extreme.** You do NOT wait for RSI>70 — you sell when price returns to fair value (the 20-SMA / target), typically with RSI back around 45–55. Waiting for overbought is trend-following and gives the bounce back.

Each session, for every open position, in order:
1. **Target hit (reverted to the 20-SMA / resistance)?** → sell in full, book the win (`06` A).
2. **Stop hit (close below support)?** → exit, no averaging down (`06` B).
3. **5-day time stop reached with no target?** → exit, redeploy (`06` C).
4. **Dip turned fundamental?** → exit (`06` D).
5. **Rare overshoot** — bounce blew *past* the target and RSI is now >70 before you could exit? → sell now; don't wait for a pullback.
6. Otherwise → hold; do not move the target up ("let it run") and do not move the stop down ("give it room").

---

## Step 7 — Weekly Review (Fridays)

Because this strategy is high-frequency and win-rate-driven, track the stats that actually diagnose it:

1. **Win rate** (target-hits ÷ total closed) — the core health metric; expect 55–70% when working.
2. **Average win vs. average loss** — wins are capped, so losers must be kept small; is avg loss ≤ avg win?
3. **Time-stop rate** — a rising share of dead-money exits means setups aren't reverting (regime turning trendy).
4. **Falling-knife check** — did any loser turn out to be below the 200-SMA or news-driven? That's a gate failure, not a strategy failure — fix the discipline.
5. **Regime note** — was the market range-bound (favorable) or trending (unfavorable)? Contextualizes the week's P&L.

---

## Contrast With the Trend-Following Book (for reference)

| | Trend-following | Mean-reversion (this book) |
|---|---|---|
| Buy | Strength (above rising 50-SMA) | Weakness (oversold at support) |
| Exit trigger | Trailing stop / trend break | **Reversion to the mean (20-SMA target)** — NOT waiting for overbought |
| Overbought RSI | Not a sell | Only an exit on a rare overshoot, or an inverse-ETF short |
| Winners | Let them run (uncapped) | Capped at a target — take profit |
| Losers | Cut at stop | Cut at stop (same) |
| Best regime | Sustained trend | Range-bound / chop |
| Fundamentals | High weight | Light gate only |
| Sentiment | Low weight | High weight (contrarian) |

Both books share the same survival scaffolding: a hard stop, no averaging down past it, and defined sizing.
