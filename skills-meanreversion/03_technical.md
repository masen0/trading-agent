---
file: 03_technical.md
purpose: The core of mean-reversion — oversold/overbought, bands, support, and the reversion setup
---

# Technical Analysis — The Core

**KEY CONCEPT — the indicators are read in the OPPOSITE direction from the trend-following book.** Here, **oversold (low RSI, price below the lower Bollinger band) is a BUY signal**, and **overbought is a SELL/exit signal** — but *only* inside an intact higher-timeframe uptrend. The single most important line in this file: **the 200-day SMA is the gate. Above it, dips are buyable. Below it, the same "oversold" reading is a falling knife — do not touch it.**

---

## Data to Pull

At least 250 daily bars per name (for the 200-day SMA) plus recent intraday context. Prefer a dedicated indicators tool for RSI/Bollinger if available.

---

## 1. The Trend Gate (checked FIRST, every time)

Compute the **200-day SMA**. This is a pass/fail gate before any oversold reading matters:

- **Price ABOVE a flat/rising 200-day SMA** → dips are buyable. Proceed.
- **Price BELOW the 200-day SMA** → the higher-timeframe trend is down. **STOP. Do not dip-buy**, no matter how oversold. This is the falling-knife rule.

Also note the 50-day SMA — it is the typical **reversion target** for an oversold bounce (price tends to snap back toward it).

---

## 2. RSI — the primary oversold trigger (read inverted)

RSI(14), standard calculation.

| RSI Level | Mean-Reversion Signal (only if above the 200-day SMA) |
|---|---|
| < 25 | Deeply oversold — **A-grade** entry candidate |
| 25–30 | Oversold — **primary (B-grade)** buy zone |
| 30–35 | Mildly oversold — a **C-grade** entry ONLY with a confirming bullish divergence or a clean support test; smaller size |
| 35–60 | Neutral — no entry edge |
| 60–70 | Elevated — a held position here is likely near its target; prepare to exit |
| > 70 | Overbought — only relevant as (a) a rare *overshoot* exit if a bounce blew past its target, or (b) an inverse-ETF/short setup on a sector. NOT the normal long exit. |

Entries are **tiered by how stretched the setup is** (A/B/C above), not a hard binary at 30 — conviction and position size scale with the depth of the oversold reading plus confirmation.

**Bullish RSI divergence** (price makes a lower low, RSI makes a higher low) at support is the highest-quality reversion trigger — it signals selling is exhausting.

**IMPORTANT — the normal exit is the MEAN, not the opposite extreme.** You do NOT wait for RSI>70 to sell. The default exit is reversion to the target (the 20-day SMA / nearest resistance), which is typically reached with RSI back around 45–55 — long before overbought. Holding an oversold bounce all the way to overbought is "letting the winner run," which is trend-following, not mean-reversion, and gives the gain back. Entry needs an extreme; exit needs only a return to fair value.

---

## 3. Bollinger Bands — the stretch measure

Bollinger Bands (20-day SMA, 2 SD). The 20-SMA (the band midline) is the **mean** you are reverting to.

| Condition | Signal |
|---|---|
| Price closes **at or below the lower band** | Statistically stretched — primary reversion buy trigger (pair with RSI<30) |
| Price returns to the **midline (20-SMA)** | **Reversion target reached — take profit** |
| Price at or above the **upper band** | Overbought — exit longs; do not chase |
| Bands very wide | High volatility — bounce potential larger, but stop must be wider |

---

## 4. Support / Resistance — where the floor is

The dip must have a *floor you can name*, or there is no stop level and no trade.

- Identify the nearest **support**: a prior swing low, a round number, the 200-day SMA, or a high-volume shelf.
- **Buy near support**, place the **hard stop just below it** (`06`). If support breaks on a close, the reversion thesis is void — exit.
- Identify the nearest **resistance** above — it caps the realistic target.

---

## 5. Volume & the reversal bar

- **Capitulation volume** (a spike on the down day) followed by a **reversal bar** (a close well off the lows) is the classic "sellers exhausted" signal — the best time to enter a bounce.
- Falling on *light* volume into support = quiet dip, also buyable.
- Falling on *heavy, sustained* volume with no reversal bar = distribution — wait; the knife is still falling.

---

## 6. Stochastics (optional confirmation)

Stochastic oscillator (14,3,3): a cross back up out of oversold (<20) confirms the RSI/Bollinger trigger. Use as a secondary confirmation, not a primary trigger.

---

## Composite Technical Score (0–5)

| Score | Conditions |
|---|---|
| 5 | Above 200-SMA + RSI<25 + at/below lower band + at named support + bullish divergence or reversal bar |
| 4 | Above 200-SMA + RSI<30 + at/below lower band + at support |
| 3 | Above 200-SMA + mildly oversold (RSI 30–35) near support WITH a confirming divergence/reversal bar — C-grade |
| 2 | Oversold but stretched far from any support (no clean stop) |
| 1 | Oversold but 200-SMA gate is marginal/flat |
| 0 | **Below the 200-day SMA — falling knife, no trade** |

---

## Quick Reference

**Entry (all required):** above rising 200-SMA + RSI ≤ 30 + at/below lower Bollinger + at a named support + no thesis-breaking news.

**Exit (any one):** price reverts to the 20-SMA (target) · a higher pre-set reward target hit · hard stop below support breached on a close · time stop reached (`06`).

**Never:** buy oversold below the 200-SMA · hold past the reversion target hoping for a trend · add below the stop.
