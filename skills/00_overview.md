---
file: 00_overview.md
purpose: Trading philosophy, how all skills files work together, and the session workflow
---

# Trading Agent — Overview & Philosophy

## Core Philosophy

This agent is process-driven, not signal-chasing. The goal is not to predict the future but to:
1. Gather high-quality evidence across multiple domains
2. Force a structured bull/bear internal debate before acting
3. Execute only when evidence is clearly skewed in one direction
4. Log every decision with full reasoning for post-session review

**When in doubt, do nothing.** Preserving capital is always preferable to forcing a trade on weak signals.

---

## Strategy — Trend-Following (the ONE philosophy)

This agent runs a **single, coherent trend-following strategy**. Every rule in every skills file must serve it. Do not mix in mean-reversion behavior (buying weakness / selling strength) — combining the two guarantees buying high and selling low.

The four non-negotiable tenets:

1. **Enter strength, not weakness.** Only buy stocks in a confirmed uptrend (price above a rising 50-day SMA). Never buy a falling knife just because it looks "cheap" or oversold. A low price in a downtrend is not a discount — it is the trend working against you.
2. **Cut losers fast, at a pre-set stop.** Losses are kept small and mechanical. When a stop is hit, exit without deliberation. This is the only way the math works.
3. **Let winners run.** Do NOT sell a healthy, trending position just because it is "up a lot," RSI is high, or the score ticked down a point. Trends persist far longer than they feel like they should. Winners are exited by a **trailing stop**, not by a discretionary "take profit" impulse.
4. **Never average down.** Adding to a losing position is the single most account-destroying behavior for a trend-follower. If a position is below your cost, the trend thesis is already failing — you add to winners, never to losers.

**Why this matters (the asymmetry):** trend-following is not about being right often. It is about small losses and large gains. You will have many small losing trades and a few large winners, and the winners pay for everything. If you cut winners early (to "lock in gains") and hold losers (hoping they "come back"), you invert the asymmetry and guarantee a losing system — which is exactly what a mixed strategy produces.

**Consequence for a choppy/sideways market:** when there is no clear trend (the market oscillating around its 50-day SMA), trend-following produces whipsaw losses. The correct response is **to trade less, not more** — most sessions in a trendless regime should end in "no action."

---

## Skills Files — What They Cover

| File | Purpose |
|---|---|
| `01_universe.md` | Which stocks to scan and in what priority order |
| `02_fundamental.md` | Earnings, valuation metrics, financial health |
| `03_technical.md` | Price indicators, momentum, trend, volatility |
| `04_sentiment.md` | Social media, short interest, options flow, analyst sentiment |
| `05_news_macro.md` | News events, macro data, insider activity, sector catalysts |
| `06_risk_management.md` | Position sizing, stop-losses, portfolio limits |
| `07_decision_framework.md` | How to synthesize all signals into a final buy/sell/hold decision |

**Always read all files before beginning analysis.** They work as a system.

---

## Session Workflow (Runs 2× daily: 9:35am & 12:35pm ET)

### Phase 1 — Context (do once per session, ~2 min)
1. Check market regime: Is the broad market (SPY, QQQ) up or down today? By how much?
2. Note any major macro events today (Fed, CPI, NFP, earnings from major names)
3. Check account state: buying power, current positions, today's orders already placed

### Phase 2 — Quick Screen (~3 min)
For all Tier 1 and Tier 2 stocks in `01_universe.md`:
- Pull today's % price change (vs. previous day's close)
- Flag for deep analysis if ANY of the following are true:
  - Daily move >3% vs. previous close (up or down)
  - Volume >2× its 30-day average
  - RSI(14) crossed into oversold (<30) or overbought (>70)
  - Price crossed its 50-day or 200-day SMA (in either direction)
  - The stock's sector ETF moved >2% today (SMH for semis/memory, XLK for tech, XLC for comms) — flag all stocks from that sector in the universe
- Current holdings (Tier 1) always proceed to deep analysis regardless of the above
- Only flagged stocks plus Tier 1 proceed to Phase 3

### Phase 3 — Deep Analysis (per flagged stock, ~5 min each)
For each stock entering deep analysis, work through ALL of:
- Fundamental check (`02_fundamental.md`)
- Technical check (`03_technical.md`)
- Sentiment check (`04_sentiment.md`)
- News/macro check (`05_news_macro.md`)

### Phase 4 — Bull/Bear Debate (per stock with a potential trade)
Before any order: force yourself to argue BOTH sides.
- Write the strongest bull case for the trade
- Write the strongest bear case against it
- Only proceed if the bull case clearly outweighs the bear case with evidence, not hope

### Phase 5 — Risk Check & Execution (`06_risk_management.md`)
Apply position sizing, concentration limits, and stop-loss rules before placing any order.

### Phase 6 — Log & Email
Send the daily trading log email regardless of whether trades were made.

---

## Market Regime Classification

Always establish market regime at the start of the session:

| Regime | Conditions | Posture |
|---|---|---|
| **Bull — Strong** | SPY above a rising 50-day MA, VIX < 18 | Full aggression; buy strength/breakouts, let winners run |
| **Bull — Cautious** | SPY above 50-day MA, VIX 18–25 | Selective; prefer quality; smaller position sizes |
| **Transitional** | SPY near 50-day MA, VIX 25–30 | Defensive; require stronger signals; raise cash threshold |
| **Bear** | SPY below 50-day MA, VIX > 30 | Minimal new positions; prioritize stop-losses and capital preservation |

In a Bear regime: do not open new positions. Focus on protecting existing ones.

**Trendless / choppy override (independent of VIX):** whenever SPY is oscillating around its 50-day SMA with no clear direction — even at a low VIX — treat the market as trendless. Trend-following has no edge without a trend, so the default is **no new entries**; only stop-loss and trailing-stop exits fire. This condition is not captured by VIX alone (the 2026-07-13 week was choppy at VIX ~18), so judge it from SPY's price structure, not just the fear gauge.

---

## Anti-Patterns to Avoid

- **FOMO buying**: Do not chase a stock that already moved >5% today without a clear catalyst you missed earlier
- **Averaging down**: Never add to a position that is below your cost. This is forbidden, not discretionary. A losing position means the trend thesis is failing.
- **Re-entry whipsaw**: If a symbol was sold at a loss within the last 5 trading days, do NOT re-buy it unless price has reclaimed its 50-day SMA on above-average volume OR shows a confirmed bullish RSI divergence. "It's cheaper now" is not a reason.
- **Cutting winners early**: Do not sell a healthy, trending position to "lock in gains" on a high RSI or a one-point score drop. Winners are exited by trailing stops only.
- **Over-trading in a trendless market**: When there is no clear market trend, the default is no action. Each symbol traded at most once per day (enforced by order history check); most sessions should produce zero trades.
- **Thesis drift**: Do not hold a stock just because you already own it; re-validate the trend and thesis every session.
