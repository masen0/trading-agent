---
file: 03_technical.md
purpose: Technical analysis — indicators to compute from OHLCV data, signals, and thresholds
---

# Technical Analysis

## Data to Pull

Use the available Robinhood MCP tool for equity historical OHLCV data — see [Robinhood's tool documentation](https://robinhood.com/us/en/support/articles/trading-with-your-agent/) for the current tool name. If a dedicated technical indicators tool is available (e.g. for RSI), prefer it over computing manually from raw OHLCV.

**Request at least 250 daily bars** (roughly 12 calendar months) for each stock being analyzed. This is a hard requirement, not a suggestion: the 200-day SMA used by the trend rules needs 200 completed bars, so a 90-day pull cannot produce it. If fewer than 200 bars are returned for a symbol (e.g. a recent IPO), record that the 200-day SMA is **unavailable** and treat the long-term trend gate as NOT satisfied — never estimate or substitute a shorter average in its place.

For weekly trend context, also pull 1 year of weekly bars.

---

## Indicators to Compute

### 1. Moving Averages (Trend Direction)

Compute from daily close prices:

| MA | Calculation | Interpretation |
|---|---|---|
| 20-day SMA | Average of last 20 closes | Short-term trend; momentum |
| 50-day SMA | Average of last 50 closes | Medium-term trend; key support/resistance |
| 100-day SMA | Average of last 100 closes | Intermediate trend |
| 200-day SMA | Average of last 200 closes | Long-term bull/bear line |

**Signals:**
- Price above all four MAs = strong uptrend — bulls in control
- Price above the 20- and 50-day but below the 100-day, while still above the 200-day = recovering from a correction — eligible if the 50-day SMA is rising, but weaker than a full stack
- **Price below the 50-day SMA = disqualified for any new entry or add.** This is absolute, not a "higher bar." No fundamental story, score, or oversold reading overrides it (see `00_overview.md` tenet 1 and the trend filter in `07_decision_framework.md`).
- **Price below the 200-day SMA = long-term downtrend — no new positions.** A "reversal" only counts once price has actually reclaimed the 50-day SMA on above-average volume; anticipating a reversal is forbidden.
- 20-day crossing above 50-day ("golden cross") = bullish momentum building
- 20-day crossing below 50-day ("death cross") = bearish. It is an exit **only** under the underwater death-cross rule in `06_risk_management.md` Exit Rule C (position more than 10% below its cost); on any other position it is a watch item.

**Distance from 50-day SMA** — this metric governs **new entries only. It is never a reason to sell or trim an existing winner.**

| Distance | Meaning for a NEW entry | Meaning for an EXISTING holding |
|---|---|---|
| >20% above | Very extended — poor entry timing; wait for consolidation or a pullback that holds above the 50-SMA | **No action.** Extension is what a winning trend looks like. It leaves only through an Exit Rule in `06_risk_management.md`. |
| 10–20% above | Extended — elevated entry risk; prefer a better price | No action |
| 0–10% above | Healthy entry zone | No action |
| **Below the 50-SMA** | **Ineligible — do not buy. Not a discount, not an "add zone."** | A completed close below the 50-SMA on above-average volume is a trend break (`06` Exit Rule C) |

> A stock trading below its 50-day SMA is in a downtrend. The further below, the *stronger* the downtrend — not the better the bargain. Treating a deep discount to the 50-SMA as an opportunity is the falling-knife error that drove the 2026-07-13 week's losses.

---

### 2. RSI — Relative Strength Index (Momentum)

**Calculation** (14-period, daily):
```
delta = daily_close.diff()
gain = delta.where(delta > 0, 0).rolling(14).mean()
loss = (-delta.where(delta < 0, 0)).rolling(14).mean()
RS = gain / loss
RSI = 100 - (100 / (1 + RS))
```

| RSI Level | Signal (trend-following interpretation) |
|---|---|
| > 70 | Overbought — NOT a sell signal in a confirmed uptrend. Strong trends stay overbought for weeks. Do not trim a healthy winner on high RSI alone; let the trailing stop do the work. Only relevant as a caution against *initiating* a brand-new position at an extended price. |
| 60–70 | Strong momentum — healthy for an existing uptrend position. Acceptable for a new entry if the trend is confirmed. |
| 40–60 | Neutral. |
| 30–40 | Weak momentum — if the stock is below its 50-day SMA this confirms a downtrend; do NOT treat as a buy. |
| < 30 | Oversold — in a downtrend this is a falling knife, NOT a buy. A low RSI in a downtrend means the trend is strong to the downside. Only meaningful as a bullish signal if it accompanies a *confirmed* reversal (price reclaiming the 50-day SMA). |
| < 20 | Deeply oversold — almost always signals a structural breakdown, not an opportunity. Avoid. |

**Important**: RSI divergence is a **confirming** signal only — never an action on its own:
- Price makes new high but RSI makes lower high → bearish divergence → a **watch item**. Do NOT trim on it alone; it is only valid as confirmation alongside a genuine trend break (see Exit Rules in `06_risk_management.md`).
- Price makes new low but RSI makes higher low → bullish divergence → relevant **only** for lifting a re-entry lockout on a name previously sold at a loss (`07_decision_framework.md`). It is not by itself a buy signal, and it never permits an entry below the 50-day SMA.

---

### 3. MACD — Moving Average Convergence Divergence (Momentum Direction)

**Calculation** (12, 26, 9 standard):
```
EMA12 = close.ewm(span=12).mean()
EMA26 = close.ewm(span=26).mean()
MACD_line = EMA12 - EMA26
Signal_line = MACD_line.ewm(span=9).mean()
Histogram = MACD_line - Signal_line
```

| Signal | Interpretation |
|---|---|
| MACD line crosses above signal line | Bullish — momentum turning positive |
| MACD line crosses below signal line | Bearish — momentum turning negative |
| Histogram expanding positively | Strengthening bullish momentum |
| Histogram shrinking (was positive) | Bullish momentum weakening — watch for reversal |
| Both MACD and signal above zero | Uptrend confirmed |
| Both below zero | Downtrend confirmed |

---

### 4. Bollinger Bands (Volatility & Extension)

**Calculation** (20-day, 2 standard deviations):
```
SMA20 = close.rolling(20).mean()
STD20 = close.rolling(20).std()
Upper_band = SMA20 + (2 × STD20)
Lower_band = SMA20 - (2 × STD20)
Band_width = (Upper_band - Lower_band) / SMA20
```

| Signal | Interpretation |
|---|---|
| Price touches/exceeds upper band | Statistically extended — poor timing for a NEW entry; do not chase. **Not a sell signal on an existing winner.** |
| Price at or below lower band | Statistically stretched to the downside. **Not a buy signal** — if the stock is below its 50-day SMA it is ineligible regardless. Mean-reversion bounces are not this strategy's edge. |
| Band width very narrow (squeeze) | Volatility compression — big move coming, direction unclear |
| Band width expanding | Trend is accelerating in current direction |
| Price walking upper band (multiple closes near upper band) | Strong trend — not a reversal signal on its own |

---

### 5. Volume Analysis

**Signals:**
- Volume today vs. 30-day average volume: compute ratio
- Volume > 2× average on an up day: strong institutional buying — bullish confirmation
- Volume > 2× average on a down day: strong institutional selling — bearish signal
- Price up significantly on below-average volume: weak move — may not sustain
- Volume dry-up during a pullback that holds above a rising 50-day SMA: selling pressure fading — supportive for an eligible trend candidate. It is not a buy signal on its own, and never for a stock below its 50-day SMA.

---

### 6. ATR — Average True Range (Volatility Sizing)

**Calculation** (14-day):
```
TR = max(high-low, abs(high-prev_close), abs(low-prev_close))
ATR = TR.rolling(14).mean()
ATR_pct = ATR / close × 100  # as % of price
```

Use ATR to set stop-loss distances (see `06_risk_management.md`) and understand normal daily price swings.

---

## Composite Technical Score (0–5)

Assign after computing all indicators:

| Score | Conditions |
|---|---|
| 5 | Price above all MAs + RSI 40–65 + MACD bullish crossover + volume confirming |
| 4 | Price above 20/50 SMA + RSI 40–65 + MACD positive |
| 3 | Mixed signals — some positive, some negative |
| 2 | Above the 50-day SMA but momentum is poor (RSI > 70 on a non-breakout setup, or MACD rolling over) |
| 1 | Price below the 50-day SMA (ineligible for entry regardless of score) |
| 0 | Technical breakdown — below the 200-day SMA, MACD negative, all MAs declining |

**Two clarifications so the score doesn't fight the rules elsewhere:**

1. **Below the 50-day SMA is a hard gate, not a score penalty.** Such a name scores 1 or 0 *and* is disqualified outright by the trend filter. Never let a strong fundamental/news score pull a sub-50-SMA name over the composite threshold — the gate is checked independently of the score.
2. **Do not penalize a confirmed momentum breakout for being overbought.** When price closes at a new 52-week high on volume ≥ 1.5× its 30-day average (the exception in `07_decision_framework.md`), score RSI > 70 as *neutral*, not as a 2. Breakouts are overbought by definition; scoring them down would silently veto the very setup the exception was written to allow.

---

## Quick Reference: Ideal Entry Conditions

For a new BUY (trend confirmation required):
- Price is above a rising 50-day SMA and above the 200-day SMA (trend filter — non-negotiable; `07_decision_framework.md` Step 0)
- RSI between 40–65 (momentum present, trend confirmed — do NOT buy sub-40 weakness)
- MACD histogram expanding positively, or just crossed bullish
- Volume on recent up days exceeds volume on recent down days

For an EXIT — the technical inputs to the Exit Rules in `06_risk_management.md` (which is authoritative):
- A completed daily close below the 50-day SMA on above-average volume (trend break, Exit Rule C)
- The active stop — initial or R-multiple trailing — is breached (Exit Rules A and B)
- A bearish MACD divergence only as confirmation alongside a trend break — never on its own

Do NOT exit on: high RSI alone, price touching the upper Bollinger Band, or a position simply being "up a lot." In an uptrend these are signs of strength, not reasons to sell. A winner leaves only through an Exit Rule in `06`.
