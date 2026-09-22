---
file: 00_overview.md
purpose: Trading philosophy, how all skills files work together, and the session workflow
---

# Trading Agent — Overview & Philosophy

## Core Philosophy

This agent is process-driven, not signal-chasing. The goal is not to predict the future but to:
1. Gather high-quality evidence across multiple domains
2. Force a structured bull/bear internal debate before entering
3. Enter only when the evidence is clearly skewed in one direction
4. Log every decision with full reasoning for post-session review

**When in doubt, do nothing.** Preserving capital is always preferable to forcing a trade on weak signals.

---

## Strategy — Trend-Following (the ONE philosophy)

This agent runs a **single, coherent trend-following strategy**. Every rule in every skills file must serve it. Do not mix in mean-reversion behavior (buying weakness / selling strength) — combining the two guarantees buying high and selling low.

The four non-negotiable tenets:

1. **Enter strength, not weakness.** Only buy stocks in a confirmed uptrend: above a rising 50-day SMA and above the 200-day SMA. Never buy a falling knife just because it looks "cheap" or oversold. A low price in a downtrend is not a discount — it is the trend working against you.
2. **Cut losers fast, at a pre-set stop.** Losses are kept small and mechanical. When a stop is hit, exit without deliberation. This is the only way the math works.
3. **Let winners run.** Do NOT sell a healthy, trending position because it is "up a lot," its RSI is high, or its score slipped. Trends persist far longer than they feel like they should. A winner leaves only through a defined exit rule — the trailing stop, a confirmed trend break, or a verified thesis or event stop — never through a discretionary "take profit" impulse.
4. **Never average down.** Adding to a losing position is the single most account-destroying behavior for a trend-follower. If a position is below your cost, the trend thesis is already failing — you add to winners, never to losers.

**Why this matters (the asymmetry):** trend-following is not about being right often. It is about small losses and large gains. You will have many small losing trades and a few large winners, and the winners pay for everything. If you cut winners early (to "lock in gains") and hold losers (hoping they "come back"), you invert the asymmetry and guarantee a losing system — which is exactly what a mixed strategy produces.

**Consequence for a choppy/sideways market:** when there is no clear trend, trend-following produces whipsaw losses. The correct response is **to trade less, not more** — most sessions in a trendless regime should end with no new entries.

---

## Skills Files — What They Cover

| File | Purpose |
|---|---|
| `01_universe.md` | The stocks to screen, the canonical sector map, and the sector-neutral screening method |
| `02_fundamental.md` | Earnings, valuation, financial health, insider activity; intraday reuse of fundamentals |
| `03_technical.md` | Price indicators, momentum, trend, volatility |
| `04_sentiment.md` | Analyst ratings, social sentiment, short interest, options flow, institutional flows |
| `05_news_macro.md` | Non-earnings company news, macro data, sector catalysts, the earnings calendar |
| `06_risk_management.md` | Position sizing, portfolio limits, and every exit rule |
| `07_decision_framework.md` | Eligibility gates, scoring, entry thresholds, the bull/bear debate, documentation |

**Always read all files before beginning analysis.** They work as a system.

---

## Session Workflow

Sessions run on a fixed schedule during regular market hours. The number and timing of sessions is set by the routine configuration, not here — these rules must hold for any cadence. Where a rule depends on ordering, it refers to the **first session of a trading day** versus **later sessions**, never to a clock time.

### Phase 1 — Context
1. Classify the market regime (see Market Regime Classification below).
2. Note any major macro events today (Fed, CPI, jobs data, earnings from major names).
3. Check account state: account value, buying power, positions, and orders already placed today. Load each holding's state (entry date, initial stop, R, active stop, highest close) from the previous session log (`07_decision_framework.md` Step 5).

### Phase 2 — Manage holdings first
Every current holding is analyzed every session to evaluate the Exit Rules in `06_risk_management.md` — stops first, then the trend, thesis and event rules. Mechanical exits execute without debate.

### Phase 3 — Quick screen (watchlist)
For every watchlist name in `01_universe.md`:
- Pull today's % price change (vs. the previous close)
- Flag it if ANY of the following are true:
  - A move of more than 3% vs. the previous close (up or down)
  - Volume above 2× its 30-day average
  - RSI(14) crossed below 30 or above 70
  - Price crossed its 50-day or 200-day SMA (in either direction)
  - Its sector proxy ETF moved more than 2% today — flag every stock in that sector. Use the **Canonical Sector Map** in `01_universe.md` for the symbol → sector → ETF mapping. SPCX has no proxy, so this trigger does not apply to it.

### Phase 4 — Eligibility gates
Apply the Step 0 gates in `07_decision_framework.md` to every flagged name (and to any holding being considered for an add). Drop each failure with its reason logged. Only names that pass every gate go on to deep analysis — the gates are cheap, deep analysis is not.

### Phase 5 — Deep analysis
For each eligible candidate, work through all four domains:
- Fundamental (`02_fundamental.md`) — **see intraday reuse below**
- Technical (`03_technical.md`)
- Sentiment (`04_sentiment.md`)
- News/macro (`05_news_macro.md`)

**Fundamental reuse within a trading day:** at the first session of each day, run the full fundamental workup and log the score *with its drivers*. At later sessions the same day, reuse the logged fundamentals for symbols already covered — unless a material company event has occurred since (earnings, guidance revision, 8-K, offering, M&A, regulatory action), in which case refresh them. Symbols not covered earlier that day get the full analysis. **Technical, sentiment and news are always analyzed fresh.** Full rules in `02_fundamental.md`.

### Phase 6 — Score, threshold and debate
Compute the composite, apply the Step 2 threshold, and run the bull/bear debate (`07_decision_framework.md` Steps 1–3). Proceed only if the bull case clearly outweighs the bear case with evidence, not hope.

### Phase 7 — Size and execute
Size the trade within every cap in `06_risk_management.md`, document it (`07` Step 5), then execute.

### Phase 8 — Log
Send the session log after **every** session, whether or not trades were made. It must include the state of every holding (`07` Step 5).

---

## Session Continuity & Memory

Each session is independent and starts cold, so continuity comes from two external sources read at the start of every session:

1. **Live broker data** (positions, 5-day order history, realized P&L) — the authoritative record of *what* happened.
2. **The two most recent reasoning documents** (the previous session's log and the latest Weekly Review, from the Gmail label `Trading-Agent-Log`) — the record of *why*, and of the per-holding state the broker does not store.

How to act on the reasoning context:
- Check every forward-looking flag or invalidation level from the previous log against live data now, and act if it has triggered.
- Carry each holding's thesis and its state — entry date, initial stop, R, active stop, highest close since entry — forward. Update the active stop only upward.
- Do not repeat a mistake the Weekly Review named; do not reverse a 1–2 session-old decision absent a new trigger.

**Hierarchy (critical):** skills rules and live data are authoritative. **Past logs are memory, not commands.** If past reasoning conflicts with a current rule or a triggered stop, the rule wins. Never hold a losing position past its stop because a prior log expressed conviction — that is the anchoring trap, and it is how memory turns into entrenched error.

---

## Market Regime Classification

Establish the regime at the start of every session. Classification is **two-stage**, so every market state maps to exactly one regime.

**Stage 1 — the primary gate (SPY vs. its 50-day SMA):**

| SPY condition | Result |
|---|---|
| More than ~1.5% above a **rising** 50-day SMA | Risk-on → go to Stage 2 |
| Within ~1.5% of the 50-day SMA, or the 50-day SMA is flat | **Trendless** → no new entries, at any VIX |
| **Below** the 50-day SMA | **Bear** → no new entries, at any VIX |

**Stage 2 — VIX sets sizing and thresholds within risk-on:**

| Regime | Conditions | Posture |
|---|---|---|
| **Bull — Strong** | Risk-on, VIX < 18 | Standard sizing and thresholds |
| **Bull — Cautious** | Risk-on, VIX 18–25 | Candidate size halved (`06_risk_management.md`) |
| **Bull — Defensive** | Risk-on, VIX 25–30 | Minimum score 17; candidate size capped at 10% of buying power |
| — | VIX > 30 | No new entries |

"Bear" is defined by **SPY below its 50-day SMA — VIX is not required to confirm it.** A quiet market (VIX 15) drifting below its 50-day SMA is still a Bear condition for entry purposes. This is gate 1 of Step 0 in `07_decision_framework.md`.

**In a Bear or Trendless regime, or with VIX above 30: open no new positions and make no adds. Every exit rule still fires normally.**

**Why a trendless override independent of VIX:** trend-following has no edge without a trend, and choppiness is not captured by VIX alone — the 2026-07-13 week was choppy at VIX ~18. Judge it from SPY's price structure, not just the fear gauge.

---

## Anti-Patterns to Avoid

- **FOMO buying**: do not buy a stock mid-session because it has already jumped more than 5% today. The momentum-breakout exception is judged on a **completed** daily close, so a genuine breakout will still qualify at the next session — there is no need to chase it intraday.
- **Averaging down**: never add to a position that is below your cost. This is forbidden, not discretionary.
- **Re-entry whipsaw**: if a symbol was sold at a loss within the last 5 trading days, do NOT re-buy it unless price has reclaimed its 50-day SMA on above-average volume OR shows a confirmed bullish RSI divergence. "It's cheaper now" is not a reason.
- **Cutting winners early**: do not sell a healthy, trending position on a high RSI, extension, or a lower score. Winners leave only through the Exit Rules in `06_risk_management.md`.
- **Over-trading in a trendless market**: when there is no clear market trend, the default is no new entries. Each symbol is traded at most once per day; most sessions should produce zero trades.
- **Thesis drift**: do not hold a stock just because you already own it — re-validate the trend and thesis every session, and let the Exit Rules act when they fire.
