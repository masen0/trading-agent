---
file: 02_fundamental.md
purpose: Fundamental analysis — what to collect, how to interpret it, thresholds for AI growth stocks
---

# Fundamental Analysis

## Data Sources

Use the available Robinhood MCP tool for equity fundamentals (P/E, market cap, 52-week range, dividend yield) — see [Robinhood's tool documentation](https://robinhood.com/us/en/support/articles/trading-with-your-agent/) for the current tool name.
Use web search for quarterly earnings details, EPS estimates, analyst consensus, and insider activity.

---

## Intraday Reuse — Do Not Re-Derive Fundamentals Within the Same Day

Fundamental data changes on a quarterly-to-multi-day cadence, not an intraday one. Re-running the full fundamental workup at every session of the same trading day wastes the most search-expensive domain in the framework for no informational gain.

**The rule:**

1. **First session of a trading day** — perform the full fundamental analysis in this file for every symbol entering deep analysis. Record the resulting score *and its drivers* in the session log (see below).
2. **Any later session of the same day** — for a symbol already covered in an earlier log **from that same trading day**, reuse the recorded fundamental score and drivers. Do not re-search.
3. **Symbol not covered earlier today** — perform the full analysis normally. A symbol that only passed the quick screen earlier, without deep analysis, counts as *not covered*.
4. **No earlier log exists for today** (first session, or an earlier session was missed or failed) — perform the full analysis normally.

**Mandatory cache invalidation — refresh fundamentals despite an earlier log if ANY of these occurred for that symbol since the prior session:**

- The company reported earnings, pre-announced, or revised guidance
- A material filing or corporate action: 8-K, share offering/dilution, M&A announcement, credit or regulatory event
- Any other **material company-specific event** listed in the High-Impact Events table of `05_news_macro.md` (for example an investigation, an unplanned CEO/CFO departure, or a major contract). Market-wide or macro news does **not** invalidate the cache — it changes the News score, not the company's fundamentals.

*Why invalidation matters:* a stale fundamental score silently props up a composite at the worst possible moment. If a name scored F:4 in the morning and cuts guidance midday, that guidance cut is a fundamental fact — so if the cached F:4 were reused, the composite would still carry a bullish fundamental input right after the fundamentals broke, and nothing else in the score would reflect it. When the story changes, the fundamentals are no longer cached data; they are wrong data.

**Logging requirement (this is what makes reuse possible):** whenever fundamental analysis is performed, the session log must record the score **plus the 2–3 drivers behind it**, not a bare digit — e.g. `F:4 — Q3 beat +12%, guidance raised, PEG 1.8`. A score with no drivers is not reusable and cannot be audited or argued against in a later bull/bear debate.

**Scope:** this reuse applies to **fundamentals only.** Technical, news, and sentiment analysis must be performed fresh at every session — price, RSI, volume, SMA distance, headlines and analyst actions all genuinely move intraday.

---

## Metrics to Collect Per Stock

### Valuation
| Metric | Source | What It Tells You |
|---|---|---|
| P/E (trailing) | Robinhood fundamentals | Current earnings multiple |
| Forward P/E | Web search (analyst estimates) | Expected earnings multiple |
| PEG ratio | Compute: forward P/E ÷ forward **EPS** growth rate | Accounts for growth; PEG < 1.0 = potentially undervalued. Always EPS growth — never revenue growth. |
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

**PEG convention (use this consistently):** `PEG = forward P/E ÷ forward EPS growth rate (%)`. Use **EPS** growth, not revenue growth — mixing the two produces materially different values for the same stock. If EPS growth is unavailable or meaningless (pre-profit company), skip PEG and rely on P/S plus the growth trend instead; do not silently substitute revenue growth.

Valuation affects the **fundamental sub-score and new-entry quality only**. It never triggers a sell — exits are governed solely by `06_risk_management.md`.

| PEG (P/E ÷ EPS growth) | Interpretation | Effect |
|---|---|---|
| < 1.0 | Potentially undervalued | Supports a higher fundamental score |
| 1.0–1.5 | Fairly valued | Neutral-positive. **An add is permitted only under the winners-only pyramid rules in `06`** — never "on a dip" below the 50-day SMA. |
| 1.5–2.5 | Moderately expensive | Neutral-to-negative input to the fundamental score |
| > 2.5 | Expensive | Lowers the fundamental score. It is **not** an entry ban — the eligibility gates are only those in `07_decision_framework.md` Step 0, and trend leaders often carry high PEGs. Nor is it an exit reason: a high PEG is the normal state of a stock that has run, and selling on it is cutting a winner early (`00_overview.md` tenet 3). |

**Exception**: If revenue growth is accelerating quarter-over-quarter, a high P/E is more defensible.

### Earnings Signals

Earnings results are scored **here, and only here** — not in News or Sentiment (ownership table, `07_decision_framework.md` Step 1). These rows set the fundamental score; none of them is by itself a buy or sell instruction.

| Signal | Effect |
|---|---|
| EPS beat >10% + guidance raised | Strong positive — raises the fundamental score. Any add still goes through the winners-only rules in `06` and every gate in `07`, including the one-session wait after a report (`05`). |
| EPS beat <5% + guidance in-line | Neutral |
| EPS miss + guidance lowered | Strong negative — lowers the score, and meets the **thesis stop** definition in `06_risk_management.md` Exit Rule D. Any exit happens through that rule. |
| EPS miss + guidance raised | Mixed — find the reason for the miss before scoring |
| Two consecutive misses | Strong negative — lowers the score and is evidence for the **thesis stop** test in `06`. Any exit happens through that rule. |
| Revenue miss (even with EPS beat) | More concerning — management may be cutting costs to hit EPS while the top line weakens |

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

Insider activity is scored **in this file only** — it is not counted again in News or Sentiment (`07_decision_framework.md` Step 1). Search SEC Form 4 filings or a site like OpenInsider.com for each stock. Weight open-market transactions by the role of the insider (CEO > CFO > other officers) and by their size relative to the person's compensation.

| Signal | Effect on the fundamental score |
|---|---|
| Cluster of insider buys (3+ executives) | Strong positive — insiders know the business best |
| Single large open-market buy (>$500k), especially by the CEO or CFO | Moderately positive |
| Routine insider sells (10b5-1 plan) | Neutral — pre-scheduled, ignore |
| Large unscheduled insider sell | Moderately negative — look for the reason |
| CEO/CFO sale of >20% of holdings | Significant negative — lowers the score. It is not an exit instruction on its own. |

---

## Fundamental Score (0–5)

At the end of fundamental analysis, assign a score:

| Score | Meaning |
|---|---|
| 5 | Beat + raised guidance + expanding margins + insider buying |
| 4 | Beat + in-line guidance OR strong fundamentals, no recent earnings |
| 3 | In-line results + stable margins — neutral |
| 2 | Miss OR declining margins OR guidance cut |
| 1 | Multiple misses + deteriorating fundamentals |
| 0 | Thesis broken — negative FCF, collapsing margins, repeated misses |

**How a low fundamental score is applied (concretely):**
- A fundamental score **≤ 2** caps the setup at medium conviction: it cannot use the high-conviction size or stop multiple even if the composite reaches 17+ (`06_risk_management.md`).
- A fundamental score of **0** (thesis broken) makes a new entry or add ineligible at any composite score (`07_decision_framework.md` Step 2).

Note the asymmetry: a weak fundamental score restricts *buying*, but it is **not** an exit trigger for a position you already hold. Exits come only from `06_risk_management.md`.
