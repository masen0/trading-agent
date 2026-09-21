---
file: 03_technical.md
purpose: Technical analysis — indicators to compute from OHLCV data, signals, and thresholds
---

# Technical Analysis

## Data to Pull

Use adjusted OHLCV data so splits and distributions do not create false signals. Request at least **260 completed daily bars** for each stock being analyzed; use 300 when available to provide indicator warm-up. Never combine an incomplete intraday bar with completed daily bars.

If a dedicated indicator tool is used, verify its adjustment policy, RSI convention, bar cutoff, and timezone. If these cannot be established, compute indicators consistently from adjusted bars.
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
- Price above 20 and 50 but below 100/200 = recovering from correction — cautious positive
- Price below 50-day SMA = medium-term downtrend — no new long entry
- Price below 200-day SMA = long-term downtrend — avoid new positions unless clear reversal
- 20-day crossing above 50-day ("golden cross") = bullish momentum building
- 20-day crossing below 50-day ("death cross") = bearish context; not a standalone exit

For this system, a moving average is **rising** when its current value is at least 0.5% above its value 10 completed sessions ago, **falling** when at least 0.5% below, and otherwise flat. The labels "golden cross" and "death cross" are not standalone trade triggers.

**Distance from 50-day SMA** (key metric):
- Price >20% above 50-day: extremely extended — block new entries unless the validated breakout exception applies; do not trim an existing winner on distance alone
- Price >10% above 50-day: extended — elevated risk for new buys
- Price within ±5% of 50-day: neutral zone
- Price 5–15% below 50-day: downtrend for this long-only strategy — no new entry
- Price >15% below 50-day: major breakdown risk — no new entry and review any existing position

---

### 2. RSI — Relative Strength Index (Momentum)

**Calculation** (14-period Wilder RSI, daily):
```
delta = daily_close.diff()
gain = delta.clip(lower=0)
loss = (-delta.clip(upper=0))
avg_gain = gain.ewm(alpha=1/14, adjust=False, min_periods=14).mean()
avg_loss = loss.ewm(alpha=1/14, adjust=False, min_periods=14).mean()
RS = avg_gain / avg_loss
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

RSI divergence is secondary confirmation and must be defined over explicit swing points; it never overrides the trend filter:
- Price makes new high but RSI makes lower high → bearish divergence → monitor; do not trim without an exit trigger
- Price makes new low but RSI makes higher low → bullish divergence → potential reversal entry

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

### 4. Bollinger Bands (Volatility & Mean Reversion)

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
| Price touches/exceeds upper band | Statistically extended — potential mean reversion; do not chase |
| Price at or below lower band | Potential oversold bounce; check RSI and volume for confirmation |
| Band width very narrow (squeeze) | Volatility compression — big move coming, direction unclear |
| Band width expanding | Trend is accelerating in current direction |
| Price walking upper band (multiple closes near upper band) | Strong trend — not a reversal signal on its own |

---

### 5. Volume Analysis

**Signals:**
- Completed-day volume versus the median of the prior 30 completed sessions. Intraday volume may be compared only with the median volume at the same elapsed session time; never compare partial-day volume directly with full-day average volume.
- Volume > 2× average on an up day: strong institutional buying — bullish confirmation
- Volume > 2× average on a down day: strong institutional selling — bearish signal
- Price up significantly on below-average volume: weak move — may not sustain
- Volume dry-up at support: sellers exhausted — potential reversal point

### Relative Strength

Compute `RS63 = stock_total_return_63_sessions - SPY_total_return_63_sessions` over identical adjusted-close dates. Compute sector-relative strength the same way using the mapped sector ETF. Positive values indicate outperformance; universe percentile ranks must use the same timestamp and eligible universe.

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

## Technical Score (0–5)

Assign after computing all indicators:

| Score | Conditions |
|---|---|
| 5 | Above rising SMA50 and SMA200; top-quintile 63-session relative strength; MACD positive; confirming completed-bar volume |
| 4 | Above rising SMA50 and SMA200 with positive relative strength and no major extension |
| 3 | Eligible uptrend but mixed momentum, flat SMA50, or extension that blocks an immediate entry |
| 2 | Below SMA50 or negative relative strength; no new entry |
| 1 | Below SMA200 with falling SMA50 or confirmed high-volume breakdown |
| 0 | Invalid/stale data or severe technical breakdown |

---

## Quick Reference: Ideal Entry Conditions

For a new BUY (trend confirmation required):
- Price is above a rising 50-day SMA (uptrend filter — non-negotiable)
- Price is above its 200-day SMA
- 63-session total return exceeds SPY's over the same dates; prefer positive sector-relative strength as well
- RSI between 40–65 (momentum present, trend confirmed — do NOT buy sub-40 weakness)
- MACD histogram expanding positively, or just crossed bullish
- Volume on recent up days exceeds volume on recent down days

For an EXIT (trend break, not mean-reversion):
- Price closes below the 50-day SMA on above-average volume (the trend has broken — this is the primary exit)
- The ATR-based stop-loss or trailing stop is hit (see `06_risk_management.md`)
- Bearish MACD divergence AS CONFIRMATION alongside a trend break — not on its own

Do NOT exit on: high RSI alone, price touching the upper Bollinger Band, or a position simply being "up a lot." In an uptrend these are signs of strength, not reasons to sell. Winners are exited by the trailing stop.
