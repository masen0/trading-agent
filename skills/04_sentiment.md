---
file: 04_sentiment.md
purpose: Sentiment analysis — social media, analyst ratings, short interest, options flow
---

# Sentiment Analysis

## Data Sources

All sentiment data comes from web search. Search specifically for:
- Reddit (r/wallstreetbets, r/investing, r/stocks, r/SecurityAnalysis)
- X/Twitter financial community (search for `$TICKER` on X)
- StockTwits (stocktwits.com/symbol/TICKER)
- Analyst ratings: search "TICKER analyst rating upgrade downgrade [current month]"
- Short interest: search "TICKER short interest" or finviz.com
- Options flow: search "TICKER unusual options activity"

---

## 1. Social Media Sentiment

Search: `"$TICKER" site:reddit.com last week` and `"$TICKER" sentiment today`

**What to look for:**
- Dominant tone: overwhelmingly bullish, mixed, or bearish
- Retail hype vs. substantive discussion
- Any viral posts or unusual volume of discussion

| Signal | Interpretation |
|---|---|
| Heavy retail hype ("to the moon", meme energy) | Contrarian warning — often late-stage retail FOMO; do not chase |
| Substantive bull thesis gaining traction | Mild positive — not a primary signal but supportive |
| Fear / panic / "it's over" posts dominating | Contrarian positive — retail capitulation often near bottoms |
| Quiet / minimal social activity | Neutral — no signal |

**Important**: Social sentiment is a contrarian indicator at extremes. Extreme retail bullishness near highs is a caution signal; extreme retail panic near lows is a supportive signal. In the middle, it adds little.

**Trend filter still governs.** Sentiment only adjusts the composite score — it never overrides the trend rule. "Extreme panic near lows" may raise the sentiment sub-score, but you still may NOT open a new position in a stock trading below its rising 50-day SMA (see `00_overview.md` and `07_decision_framework.md`). Contrarian sentiment can help time a re-entry *after* a trend has been reconfirmed; it is never a license to buy a falling knife.

---

## 2. Analyst Ratings & Price Target Changes

Search: `"TICKER" analyst upgrade OR downgrade OR "price target" [current month]`

| Signal | Interpretation |
|---|---|
| Multiple upgrades in same week | Positive momentum — institutions increasing coverage |
| Upgrade from Sell/Neutral to Buy by major bank | Meaningful positive catalyst |
| Downgrade from Buy to Neutral/Sell | Meaningful negative — respect this more than upgrades |
| Price target increase with Buy maintained | Mildly positive |
| Price target cut with Buy maintained | Soft negative — analyst less confident |
| Consensus Buy% > 80% | Widely loved — less room for upside surprise; high expectations |
| Consensus Buy% < 40% | Under-followed or out of favor — potential for re-rating higher if results improve |

**Note on analyst herding**: Analysts tend to upgrade after stocks have already moved up and downgrade after they've fallen. Use ratings as context, not as primary buy/sell triggers.

---

## 3. Short Interest (Contrarian Indicator)

Search: `"TICKER" short interest float` or check finviz.com

| Short Float % | Interpretation |
|---|---|
| < 3% | Low short interest — market is not skeptical; no squeeze potential |
| 3–10% | Normal range — neutral |
| 10–20% | Elevated short interest — meaningful skepticism; potential for short squeeze on good news |
| > 20% | High conviction shorts — either market sees real risk, OR powerful short squeeze fuel on catalyst |
| > 30% | Extreme — very high risk both ways; shorts may be right, or a squeeze could be violent |

**Short squeeze conditions** (all must be present):
1. Short float > 15%
2. Strong positive catalyst (earnings beat, contract win, industry news)
3. Technical breakout on high volume

If all three present simultaneously, a squeeze may amplify the move significantly.

---

## 4. Options Flow & Implied Volatility

Search: `"TICKER" unusual options activity` or `"TICKER" call sweep`

### Unusual Options Activity
| Signal | Interpretation |
|---|---|
| Large call sweeps (market-order calls, short-dated) | Smart money positioning for near-term move up |
| Large put sweeps | Hedging or directional bet on downside |
| Call/Put ratio > 3:1 on unusual volume day | Bullish positioning |
| Put/Call ratio > 2:1 on unusual volume | Bearish hedging or directional short |

### Implied Volatility (IV)
| IV Condition | Interpretation |
|---|---|
| IV Rank > 80 (IV very high vs historical) | Options expensive; prefer selling premium; expect post-event crush |
| IV Rank < 20 (IV very low) | Options cheap; cheap to hedge or speculate |
| IV spiking before earnings | Market pricing in large move — actual move may disappoint |
| IV collapsing after earnings ("IV crush") | Expected — not a signal by itself |

---

## 5. Institutional Ownership Changes (13F Filings — Quarterly)

Search: `"TICKER" institutional ownership 13F` or look at recent SEC 13F filings

| Signal | Interpretation |
|---|---|
| Major new position opened by top fund | Strong positive (e.g., Berkshire, Druckenmiller, top quant funds) |
| Existing major position added to | Positive — high-conviction holder increasing bet |
| Major fund fully exited | Negative signal — investigate reason |
| Broad institutional ownership increase | Positive — institutions catching up to the thesis |
| Concentration in very few institutions | Risk — if one exits, it can cause sharp drop |

---

## Sentiment Score (0–5)

| Score | Conditions |
|---|---|
| 5 | Analyst upgrades + institutional buying + moderate short interest (squeeze potential) + smart money call flow |
| 4 | Positive analyst sentiment + low short interest + neutral social |
| 3 | Mixed signals across all domains |
| 2 | Analyst downgrades OR high social hype (FOMO) at highs |
| 1 | Multiple downgrades + institutional selling + bearish options flow |
| 0 | Analyst consensus Sell + major fund exit + high put flow |
