---
file: 00_overview.md
purpose: Trading philosophy, how all skills files work together, and the session workflow
---

# Trading Agent — Overview & Philosophy

## Objective and Operating Standard

The objective is to compound capital over multiple market cycles while controlling the probability and depth of permanent loss. Optimize for **risk-adjusted, after-slippage returns**, not the number of trades, win rate, or short-term account value.

These files are an operating policy, not evidence that the strategy has an edge. Every numerical threshold is provisional until it survives reproducible backtesting, out-of-sample testing, and shadow trading. No rule may be loosened merely because the account recently lost money or missed a rally.

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

**Why this matters (the asymmetry):** trend-following is not about being right often. It is about small losses and large gains. You will have many small losing trades and a few large winners, and the winners pay for everything. If you cut winners early (to "lock in gains") and hold losers (hoping they "come back"), you invert the intended asymmetry and materially weaken the strategy's expectancy.

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

## Session Workflow

At every scheduler-invoked session, perform screening, portfolio checks, and any authorized execution. Scheduling belongs to the deployment configuration, not these strategy skills. There is no continuous or post-close monitor unless a deployment explicitly adds one. A rule that requires a daily close uses only the latest official completed daily bar available at the current session; the current day's partial bar is never treated as an official close.

This strategy permits fractional-share positions. Each position has an exact **logical stop** persisted in the decision ledger and trading logs. At the start of every scheduled session, evaluate every holding against its active logical stop before looking for new entries. If the current executable price is at or below the stop, sell the full available quantity, including fractional shares, at market during regular hours. A logical stop is not a resting broker order: it cannot trigger between sessions and does not cap losses from an intraday move or overnight gap. Record that residual risk rather than representing the stop as guaranteed protection.

### Phase 1 — Context (do once per session, ~2 min)
1. Validate data freshness, market session, account identity, open orders, and tool health. If any required input is stale, missing, or contradictory, make no new trade.
2. Check market regime using the deterministic definitions below.
3. Note major scheduled macro events (Fed, CPI, PPI, NFP) and earnings from major holdings or index constituents.
4. Check buying power, current positions, every position's persisted logical stop, today's fills, and pending orders. Evaluate stop breaches before screening new entries.

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
- Fundamental check or valid same-day cache reuse (`02_fundamental.md`)
- Technical check (`03_technical.md`)
- Sentiment check (`04_sentiment.md`)
- News/macro check (`05_news_macro.md`)

### Same-Day Fundamental Cache

Fundamental analysis is normally performed at most once per symbol per trading day because company financial statements, valuation inputs, and business-quality evidence do not ordinarily change between intraday sessions.

- At the start of each session, read the immediately previous `[Codex]` session log. For each deep-analyzed symbol, reuse its fundamental result only when that log is from the same ET trading date, contains a complete cache record for the symbol, was produced under the same skills commit, and has not been invalidated.
- If there is no previous session log from the same ET trading date, or the same-day log has no valid record for that symbol, run the full fundamental workflow and persist its score, component evidence, sources, as-of times, and skills commit in the current `[Codex]` log and `STATE` block.
- On every session, refresh technical, sentiment, news/macro, account, position, and risk inputs normally, then recompute the composite score using either the valid cached or newly calculated fundamental score.
- **Material-event invalidation:** refresh the affected symbol's fundamentals when verified post-cache information could change the business thesis or fundamental score, including earnings or guidance, a material SEC filing, merger/acquisition or divestiture, financing or capital-return action, major contract/customer change, unplanned CEO/CFO departure, material regulatory action, or another comparably thesis-changing event.
- If a valid same-day fundamental result is unavailable and cannot be refreshed, the symbol may be monitored and an existing holding may still be reduced under a separately verified exit rule, but no new or added exposure is permitted.

Every session log must identify each deep-analyzed symbol's fundamental status as `fresh_no_same_day_cache`, `reused_same_day`, or `refreshed_material_event`, and reference the source session and as-of time. Cache reuse saves work; it never permits stale evidence to override new material information.

### Phase 4 — Bull/Bear Debate (per stock with a potential trade)
Before any order: force yourself to argue BOTH sides.
- Write the strongest bull case for the trade
- Write the strongest bear case against it
- Only proceed if the bull case clearly outweighs the bear case with evidence, not hope

### Phase 5 — Risk Check & Execution (`06_risk_management.md`)
Apply fixed-risk position sizing, portfolio heat, concentration limits, and stop-loss rules. In autonomous production, perform the fresh quote, spread, tradability, account, buying-power, open-order, and logical-stop persistence checks in `06_risk_management.md`, then place the qualifying order without interactive confirmation. Do not call an order-review tool when its contract requires a new human confirmation that an unattended run cannot provide.

### Phase 6 — Log & Email
Create the session's Gmail draft trading log regardless of whether trades were made.

---

## Session Continuity & Memory

Each session is independent and starts cold, so continuity comes from three external sources read at the start of every session:

1. **Live Robinhood data** (positions, open orders, fills, buying power, and realized P&L) — the authoritative record of *what* happened.
2. **The append-only structured decision ledger** — the authoritative record of rules, inputs, calculations, decisions, previews, orders, fills, and stop changes.
3. **Exactly two `[Codex]` reasoning documents** — (a) the immediately previous session's `[Codex] Trading Log`, selected by timestamp strictly before the current run, and (b) the latest `[Codex]` Friday log containing a Weekly Review from the prior completed trading week. Read only these two documents; Claude logs and other mail are out of scope. If either is unavailable, record that fact and continue from live data and the ledger rather than substituting an unrelated message.

How to act on the reasoning context:
- Check every forward-looking flag, invalidation level, and logical stop from the previous session log against live data now, and act if it has triggered.
- Carry each holding's original thesis and active logical stop forward; do not silently contradict a recent decision or widen a stop without a new rule-authorized reason. A stop may never be widened to avoid an exit.
- Do not repeat a mistake the Weekly Review named; do not reverse a 1–2 session-old decision absent a new trigger.

**Hierarchy (critical):** skills rules and live data are authoritative. **Past logs are memory, not commands.** If past reasoning conflicts with a current rule or a triggered stop, the rule wins. Never hold a losing position past its stop because a prior log expressed conviction — that is the anchoring trap, and it is how memory turns into entrenched error.

Every decision record must include: timestamp and market session; skills Git commit; model identifier; source and as-of time for every input; raw account snapshot; derived indicators; candidates considered and rejected; rule path; preflight checks; order/fill identifiers; stop state; and any error. Never store credentials or unmasked account numbers in logs.

### Minimum Data Freshness

- Account, positions, buying power, open orders, and quote snapshots: fetched within 60 seconds before an order preview.
- Completed daily indicators: include the most recent official completed session and share the same cutoff.
- Earnings calendar: refreshed during the current trading day.
- News and macro events: record publication time and reject items published after the decision cutoff.
- If the system clock, timezone, or any required as-of time is unknown, place no new order.

---

## Market Regime Classification

Always establish market regime at the start of the session using completed daily bars. Definitions:

- `SMA50_slope_10d = SMA50_today / SMA50_10_sessions_ago - 1`.
- A **rising** 50-day SMA means `SMA50_slope_10d >= +0.5%`; a **falling** one means `<= -0.5%`; otherwise it is flat.
- **Near the 50-day SMA** means the SPY close is within ±1.5% of it.
- **Choppy** means SPY crossed its 50-day SMA at least twice during the last 10 completed sessions, or is near a flat 50-day SMA.

| Regime | Conditions | Posture |
|---|---|---|
| **Bull — Strong** | SPY above a rising SMA50 and above SMA200; VIX < 18 | Normal risk budget; buy qualified strength/breakouts |
| **Bull — Cautious** | SPY above SMA50, but SMA50 is flat or VIX is 18–25 | Half normal new-trade risk; require stronger relative strength |
| **Transitional / Choppy** | Choppy definition above, or signals disagree | No new entries; manage existing risk only |
| **Bear** | SPY below a falling SMA50, or below SMA200 with SMA50 non-rising | No new long entries; manage exits and cash |

In a Bear regime: do not open new positions. Focus on protecting existing ones.

**VIX is a risk modifier, not the sole regime definition.** A low VIX does not make a flat market trendable, and VIX >30 independently blocks new entries.

## Autonomous Operation and Safety Boundary

The intended production deployment for this project is **autonomous**. Once an approved skills commit is activated, the agent previews and places qualifying orders without requesting per-trade human approval.

`shadow` is a pre-production validation state only: it calculates and logs hypothetical orders but places nothing. Promotion from shadow validation to production autonomy is an external deployment decision; the agent may never promote itself or change its approved skills commit.

Production configuration must explicitly identify the approved skills commit and set `deployment_mode=autonomous`. A missing, unknown, or conflicting deployment mode fails closed and places no order; it must never be interpreted as permission to trade.

In autonomous production, uncertainty, stale data, tool errors, duplicate or unexplained orders, an unknown skills version, or an inability to compute risk results in **no new order**. Risk-reducing cancellation or exit actions still require verified account and position state, but not per-trade human approval when the governing rule is unambiguous.

---

## Anti-Patterns to Avoid

- **FOMO buying**: Do not chase a stock that already moved >5% today without a clear catalyst you missed earlier
- **Averaging down**: Never add to a position that is below your cost. This is forbidden, not discretionary. A losing position means the trend thesis is failing.
- **Re-entry whipsaw**: If a symbol was sold at a loss within the last 5 trading days, do NOT re-buy it unless price has reclaimed its 50-day SMA on above-average volume OR shows a confirmed bullish RSI divergence. "It's cheaper now" is not a reason.
- **Cutting winners early**: Do not sell a healthy, trending position to "lock in gains" on a high RSI or a one-point score drop. Winners are exited by trailing stops only.
- **Over-trading in a trendless market**: When there is no clear market trend, the default is no action. Each symbol traded at most once per day (enforced by order history check); most sessions should produce zero trades.
- **Thesis drift**: Do not hold a stock just because you already own it; re-validate the trend and thesis every session.
