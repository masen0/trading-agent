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

## Session Workflow (Run Every Hour, 9:35am–3:35pm ET)

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
| **Bull — Strong** | SPY above 50-day MA, VIX < 18 | Full aggression; buy dips, let winners run |
| **Bull — Cautious** | SPY above 50-day MA, VIX 18–25 | Selective; prefer quality; smaller position sizes |
| **Transitional** | SPY near 50-day MA, VIX 25–30 | Defensive; require stronger signals; raise cash threshold |
| **Bear** | SPY below 50-day MA, VIX > 30 | Minimal new positions; prioritize stop-losses and capital preservation |

In a Bear regime: do not open new positions. Focus on protecting existing ones.

---

## Anti-Patterns to Avoid

- **FOMO buying**: Do not chase a stock that already moved >5% today without a clear catalyst you missed earlier
- **Revenge trading**: Do not re-enter a position immediately after a stop-loss; wait at least one session
- **Over-trading**: Each symbol should be traded at most once per day (enforced by order history check)
- **Thesis drift**: Do not hold a stock just because you already own it; re-validate the thesis every session
- **Averaging down blindly**: Only add to a losing position if the original thesis is intact AND fundamentals still support it
