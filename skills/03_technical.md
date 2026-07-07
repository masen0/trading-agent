---
file: 03_technical.md
purpose: Technical analysis — indicators to compute from OHLCV data, signals, and thresholds
---

# Technical Analysis

## Data to Pull

Use the available Robinhood MCP tool for equity historical OHLCV data — see [Robinhood's tool documentation](https://robinhood.com/us/en/support/articles/trading-with-your-agent/) for the current tool name. If a dedicated technical indicators tool is available (e.g. for RSI), prefer it over computing manually from raw OHLCV. Request at least **90 days of daily bars** for each stock being analyzed.
90 days gives enough history for 50-day SMA and meaningful RSI/MACD calculations.
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
- Price below 50-day SMA = medium-term downtrend — require stronger signals to buy
- Price below 200-day SMA = long-term downtrend — avoid new positions unless clear reversal
- 20-day crossing above 50-day ("golden cross") = bullish momentum building
- 20-day crossing below 50-day ("death cross") = bearish signal — consider reducing exposure

**Distance from 50-day SMA** (key metric):
- Price >20% above 50-day: extremely extended — overbought, high trim signal
- Price >10% above 50-day: extended — elevated risk for new buys
- Price within ±5% of 50-day: neutral zone
- Price 5–15% below 50-day: oversold territory — potential add zone if thesis intact
- Price >15% below 50-day: deeply oversold — check if fundamentals still support or if breakdown

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

| RSI Level | Signal |
|---|---|
| > 75 | Strongly overbought — trim signal (especially if >10% above 50-day SMA) |
| 65–75 | Overbought — caution for new buys |
| 45–65 | Neutral |
| 35–45 | Mildly oversold — monitor for entry |
| < 35 | Oversold — consider adding if thesis intact and no fundamental breakdown |
| < 25 | Deeply oversold — strong add signal if fundamentals solid; could also mean breakdown |

**Important**: RSI divergence is a high-value signal:
- Price makes new high but RSI makes lower high → bearish divergence → consider trimming
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
- Volume today vs. 30-day average volume: compute ratio
- Volume > 2× average on an up day: strong institutional buying — bullish confirmation
- Volume > 2× average on a down day: strong institutional selling — bearish signal
- Price up significantly on below-average volume: weak move — may not sustain
- Volume dry-up at support: sellers exhausted — potential reversal point

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
| 4 | Price above 20/50 SMA + RSI < 65 + MACD positive |
| 3 | Mixed signals — some positive, some negative |
| 2 | Price below 50-day SMA OR RSI > 72 (overbought for new buy) |
| 1 | Price below 200-day SMA + MACD negative + high RSI divergence |
| 0 | Technical breakdown — price making new lows, all MAs declining |

---

## Quick Reference: Ideal Entry Conditions

For a new BUY:
- Price is above 50-day SMA (uptrend filter)
- RSI between 35–60 (not overbought, not in freefall)
- MACD histogram expanding positively, or just crossed bullish
- Volume on recent up days exceeds volume on recent down days

For a TRIM or SELL:
- RSI > 72 with price >15% above 50-day SMA
- Bearish MACD divergence
- Price breaking below 50-day SMA on above-average volume
- Price touches upper Bollinger Band on declining volume
