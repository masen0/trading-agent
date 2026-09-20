---
file: 00_overview.md
purpose: Mean-reversion trading philosophy, how the skills files work together, and the session workflow
---

# Mean-Reversion Agent — Overview & Philosophy

## Core Philosophy

Prices oscillate around a mean. When they stretch to an extreme, they tend to snap back. This agent buys **temporary weakness** and sells into **strength / at a defined target** — capturing the snap-back, not the trend.

**KEY CONCEPT — this is the mirror image of the trend-following book.** Where that strategy buys strength and lets winners run indefinitely, this one buys weakness and *caps* its winners at a defined target. It is designed to make money in **choppy, range-bound markets** — exactly the environment where trend-following bleeds.

**When in doubt, do nothing.** A missed bounce costs nothing. A falling knife costs capital.

---

## The ONE Philosophy — Disciplined Mean-Reversion

Four non-negotiable tenets. Every rule in every file serves them:

1. **Buy oversold, not strength.** Entries happen when a stock is stretched *below* its mean (low RSI, at/below the lower Bollinger band), never when it is making new highs.
2. **Only dip-buy in an intact uptrend.** The higher-timeframe trend must still be up (price above a rising 200-day SMA). This is the single rule that separates a *buyable dip* from a *falling knife*. A dip in a downtrend is not an opportunity — it is the trend working, and it has no floor.
3. **Take profits at a defined target.** The exit is the mean (typically the 20-day SMA) or a pre-set reward level. Mean-reversion *caps* its winners by nature — do NOT "let it run." Holding past the target hoping for more is thesis drift.
4. **Hard stop below support; one entry, never average down past it.** The dip must have a floor you can point to (support, prior low). If it breaks, you were wrong — exit. Adding below your stop is the martingale trap and has no bottom.

**Why this works (and when it doesn't):** In a range-bound market, extremes overshoot and revert — the edge is real and repeatable. In a *strong directional trend*, mean-reversion is dangerous: "oversold" keeps getting more oversold. So this strategy must actively stand down when the market is trending hard (see Regime, below). It is a chop specialist, not an all-weather system.

---

## Skills Files — What They Cover

| File | Purpose |
|---|---|
| `01_universe.md` | Which stocks/ETFs to trade and how they're screened |
| `02_fundamental.md` | Light quality filter — only dip-buy sound businesses so the bounce is likely |
| `03_technical.md` | **The core** — oversold/overbought, bands, support, the reversion setup |
| `04_sentiment.md` | Contrarian extremes — buy panic, fade euphoria |
| `05_news_macro.md` | The falling-knife filter — is the dip technical (buyable) or fundamental (avoid)? |
| `06_risk_management.md` | Sizing, hard stops, profit targets, time stops, reward:risk |
| `07_decision_framework.md` | Combine signals into a buy/target/stop decision with a min reward:risk |

**Always read all files before analysis.** They work as a system.

---

## Session Workflow (Runs 2× daily: 9:35am & 12:35pm ET)

### Phase 1 — Context (~2 min)
1. Establish market regime (see below). If strongly trending, mean-reversion stands down for new entries.
2. Note major macro/news events.
3. Check account: buying power, current positions, today's orders.

### Phase 2 — Manage open positions FIRST
For every open position, check in this order: profit target hit? → hard stop hit? → time stop hit? Act mechanically before looking for anything new.

### Phase 3 — Screen for reversion setups
Scan the universe for stocks that are (a) oversold, (b) at/near support, (c) still in a higher-timeframe uptrend, (d) with no thesis-breaking news. Only these advance to deep analysis.

### Phase 4 — Score & size the setup
Run the decision framework (`07`), require the minimum reward:risk, size per `06`.

### Phase 5 — Execute & document
Place the entry, and record the target and stop at the same moment. Every trade is logged.

---

## Market Regime Classification (mean-reversion lens)

| Regime | Conditions | Posture |
|---|---|---|
| **Range-bound / choppy** | SPY oscillating around a flat 50-day SMA; VIX 13–22 | **Ideal** — full mean-reversion activity |
| **Mild uptrend** | SPY gently above a rising 50-day SMA | Dip-buy pullbacks to support; normal size |
| **Strong trend (up or down)** | SPY extended, momentum one-directional, wide daily ranges | **Stand down on new entries** — reversion fails in strong trends; manage existing only |
| **High stress** | VIX > 30 or SPY down >3% intraday | No new longs; capitulation may be buyable *only* with confirmation of a reversal bar |

---

## Anti-Patterns to Avoid

- **Catching a falling knife**: buying an oversold stock that is *below its 200-day SMA / in a downtrend*. Oversold in a downtrend gets more oversold. Forbidden.
- **No stop / mental stop**: every dip-buy has a hard stop below support, set at entry.
- **Averaging down past the stop**: the martingale trap — has no floor. One entry only.
- **Holding past the target**: mean-reversion takes the bounce and leaves. Greed here gives the gain back.
- **Fighting a strong trend**: do not fade a powerful directional move; wait for it to stall into a range.
- **Dip-buying bad news**: a gap-down on a guidance cut or investigation is not a dip — it is a repricing.
