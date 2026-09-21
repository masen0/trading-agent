---
file: 02_fundamental.md
purpose: Fundamental analysis — what to collect, how to interpret it, thresholds for AI growth stocks
---

# Fundamental Analysis

## Data Sources

Use the available Robinhood MCP tool for basic equity fundamentals. For financial statements and guidance, prefer company filings, earnings releases, and investor-relations materials; use web search only to locate or corroborate them. Record the source and as-of date. Never score an unsourced search snippet.

Fundamentals are slow-moving. Refresh after earnings, material filings, or major company events, and otherwise no more than weekly. Reuse a timestamped cached assessment during intraday sessions.

---

## Metrics to Collect Per Stock

### Valuation
| Metric | Source | What It Tells You |
|---|---|---|
| P/E (trailing) | Robinhood fundamentals | Current earnings multiple |
| Forward P/E | Web search (analyst estimates) | Expected earnings multiple |
| PEG ratio | Compute consistently using forward P/E ÷ expected EPS growth rate | A rough growth-adjusted valuation measure; compare only when inputs use compatible periods |
| P/S ratio | Web search | More useful for pre-profit or high-growth companies |
| EV/EBITDA | Web search | Enterprise value vs operating earnings; useful for comparing across capital structures |

### Earnings Quality
| Metric | Source | What It Tells You |
|---|---|---|
| EPS (last 4 quarters) | Web search | Trend direction matters more than absolute level |
| Revenue growth (YoY, QoQ) | Web search | Is the top line accelerating or decelerating? |
| Gross margin trend | Web search | Expanding margins = pricing power or efficiency; contracting = competitive pressure |
| Free Cash Flow (FCF) | Web search | Real cash generation; companies can manage EPS but not FCF |
| Earnings surprise (last 2 quarters) | Web search | Beat/miss magnitude and direction vs consensus |
| Revenue surprise (last 2 quarters) | Web search | Revenue surprise often more predictive than EPS |
| Guidance raised/lowered | Web search | What management said about next quarter |

### Financial Health
| Metric | Source | What It Tells You |
|---|---|---|
| Debt/Equity ratio | Web search | Leverage; high D/E + rising rates = risk |
| Current ratio | Web search | Short-term liquidity; <1.0 = potential stress |
| ROE (Return on Equity) | Web search | Efficiency of shareholder capital; >15% generally strong |
| ROIC (Return on Invested Capital) | Web search | Better than ROE for capital-intensive firms; >10% generally good |

---

## Interpretation Thresholds for AI Growth Stocks

AI and semiconductor stocks often trade at premium multiples justified by growth rates. Do not apply traditional "P/E > 30 = expensive" rules blindly.

### Valuation Framework

| P/E / PEG Context | Interpretation |
|---|---|
| PEG < 1.0 with durable positive EPS growth | Valuation support; not a buy trigger |
| PEG 1.0–1.5 | Neutral to reasonable relative to expected growth |
| PEG 1.5–2.5 | Demanding; require stronger quality and trend evidence |
| PEG > 2.5 or not meaningful | High expectation risk; do not infer an automatic sell |

Never compare P/E directly with revenue growth and call the result PEG. For loss-making or cyclically depressed companies, PEG is not meaningful; use revenue growth, gross margin, FCF trajectory, and peer-relative EV/sales instead.

### Earnings Signals

| Signal | Action |
|---|---|
| EPS beat >10% + guidance raised | Strong positive catalyst — consider adding |
| EPS beat <5% + guidance in-line | Neutral — monitor |
| EPS miss + guidance lowered | Negative catalyst — reassess thesis; consider trimming |
| EPS miss + guidance raised | Mixed — dig into the reason for miss before acting |
| Two consecutive misses | Thesis may be broken — consider full exit |
| Revenue miss (even with EPS beat) | More concerning — management may be cutting costs to hit EPS while top line weakens |

### Margin Trends

| Signal | Interpretation |
|---|---|
| Gross margin expanding | Pricing power or scale — positive |
| Gross margin contracting >2pp YoY | Competitive pressure or input costs — negative |
| Operating margin expanding | Efficiency improving — positive |
| FCF margin > 20% | Strong cash generation — quality business |
| FCF negative on declining revenue | Red flag — sustainability risk |

---

## Insider Activity (90-Day Window)

Search SEC Form 4 filings or a site like OpenInsider.com for each stock.

| Signal | Interpretation |
|---|---|
| Cluster of insider buys (3+ execs) | Strong bullish signal — insiders know the business best |
| Single insider buy, large size (>$500k) | Moderately bullish |
| Routine insider sells (10b5-1 plan) | Neutral — pre-scheduled, ignore |
| Large unscheduled insider sell | Moderately bearish — investigate reason |
| CEO/CFO sale of >20% of holdings | Significant concern — re-evaluate position |

---

## Fundamental Quality Score (0–5)

At the end of fundamental analysis, assign a score:

| Score | Meaning |
|---|---|
| 5 | Positive revenue and FCF trend, stable/expanding margins, sound balance sheet, and raised or conservatively achievable guidance |
| 4 | Strong quality with at most one minor weakness; guidance intact |
| 3 | Mixed but stable; no material deterioration |
| 2 | One material deterioration: guidance cut, significant margin compression, balance-sheet stress, or revenue miss |
| 1 | Multiple material deteriorations or repeated misses |
| 0 | Fundamental thesis invalidated by verified evidence |

Document each point with a dated fact. The same earnings or guidance event may be scored here or as a news catalyst, but not both. A score below 3 blocks a new position; valuation alone never forces an exit from an otherwise valid trend.
