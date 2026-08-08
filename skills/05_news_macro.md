---
file: 05_news_macro.md
purpose: News analysis, macroeconomic context, sector catalysts, and insider activity
---

# News & Macro Analysis

## Data Sources

All news and macro data comes from web search. Run targeted searches at the start of each session:
- Company-specific: `"TICKER" news today` and `"TICKER" news last 48 hours`
- Sector: `AI semiconductor news today`, `cloud computing news today`
- Macro: `Fed news today`, `inflation data`, `10-year treasury yield`
- Insider: `"TICKER" insider buying SEC Form 4` (90-day window)

---

## 1. Company-Specific News

### High-Impact Events (Act Immediately)

| Event | Impact | Response |
|---|---|---|
| Earnings beat + guidance raise | Strong positive | Consider adding; validate technicals first |
| Earnings miss + guidance cut | Strong negative | Reassess thesis; consider trimming or exiting |
| Major contract win (government, hyperscaler) | Positive | Validate size and strategic fit before acting |
| Product launch or tech breakthrough | Positive (if material) | Assess whether it changes competitive position |
| CEO/CFO departure (unplanned) | Negative | Reduce position; wait for clarity |
| DOJ/SEC investigation, major lawsuit | Negative | Reduce exposure; monitor development |
| Acquisition announcement (acquirer) | Often negative short-term (premium paid) | Evaluate strategic logic |
| Acquisition announcement (target) | Strong positive | Premium typically 20–40% above market price |
| Share buyback announcement (large, >5% of float) | Positive | Management confidence; floor under stock |
| Dividend cut | Negative | Signals financial stress |

### Medium-Impact Events (Consider in Context)

| Event | Impact |
|---|---|
| Analyst day / investor day | Positive if guidance raised or new products unveiled |
| Supply chain disruption | Negative for manufacturers; positive for some competitors |
| Partnership announcement | Positive if material; many are PR |
| Share offering / dilution | Negative — dilutes existing shareholders |
| Stock split | Neutral (doesn't change value; may improve retail accessibility) |
| Index inclusion (S&P 500, Nasdaq 100) | Positive — forced buying from index funds |
| Index exclusion | Negative — forced selling |

---

## 2. Macroeconomic Context

Check at start of each session. Macro shifts the regime for ALL stocks.

### Federal Reserve & Interest Rates

| Condition | Impact on AI/Tech Stocks |
|---|---|
| Rate cut or dovish pivot signal | Strongly positive — growth stocks re-rate higher (lower discount rate) |
| Rate hike or hawkish surprise | Negative — compresses multiples on growth stocks |
| Rates stable / "higher for longer" | Mildly negative; requires earnings growth to drive stock appreciation |
| 10-year treasury > 5% | Challenging environment for high-multiple growth stocks |
| 10-year treasury < 4% | More supportive of growth stock valuations |

**Rule**: When 10-year yield is rising fast (>30bps in a month), be more cautious on adding to high-P/E AI names.

### Inflation Data (CPI/PPI)

| Signal | Impact |
|---|---|
| CPI below expectations | Positive — supports rate cuts; tech stocks rally |
| CPI above expectations | Negative — delays rate cuts; tech sells off |
| PPI declining | Positive for margins of tech hardware companies |

### GDP & Employment

| Signal | Impact |
|---|---|
| Strong GDP growth | Positive — enterprise spending on AI continues |
| GDP contraction or recession signals | Negative — IT budgets cut; AI projects delayed |
| Strong jobs report | Mixed — good for economy but may delay Fed cuts |
| Weak jobs report | Mixed — recession concern vs. rate cut hope |

---

## 3. Sector-Specific AI Catalysts

Monitor these recurring themes that can move entire sectors:

| Catalyst | Affected Stocks | Direction |
|---|---|---|
| Hyperscaler (MSFT/GOOGL/AMZN/META) capex guidance raised | NVDA, AMD, AMAT, LRCX, SMCI, VRT | Strongly positive |
| Hyperscaler capex guidance cut | Same as above | Strongly negative |
| New Nvidia GPU architecture announcement | NVDA (positive), AMD (pressure), HBM suppliers MU/SNDK | |
| TSMC capacity expansion | AMAT, LRCX, ASML, SNPS, CDNS | Positive |
| US-China chip export restrictions tightened | NVDA, AMD, AMAT (negative short-term); domestic beneficiaries | |
| New AI model release (OpenAI, Google, Anthropic) | Positive for AI infrastructure stocks broadly | |
| Enterprise AI adoption slowdown reports | Negative for software AI plays (PLTR, SNOW, DDOG) | |
| Data center power constraints | CEG, VST, NRG (positive); limits hyperscaler growth (negative) | |

---

## 4. Insider Activity (90-Day Window)

Search: `"TICKER" insider buying SEC Form 4` or `openinsider.com TICKER`

Look for:
- Any buy transactions (voluntary, not pre-scheduled 10b5-1)
- The role of the buyer (CEO > CFO > VP in significance)
- The dollar size relative to their compensation

| Pattern | Signal |
|---|---|
| CEO or CFO open-market buy > $500K | Strong positive — high conviction from top executive |
| Multiple executives buying in same month | Cluster buying — very bullish signal |
| 10b5-1 scheduled plan sales | Neutral — ignore, pre-planned |
| Large unscheduled sale by CEO | Mild negative — investigate, but may be personal reasons |
| Collective net buying (buys > sells over 90 days) | Positive |
| Collective net selling (sells >> buys over 90 days) | Mild negative |

---

## 5. Earnings Calendar

**Mandatory per-session check for ALL current holdings.** At the start of every session, pull the earnings date for each stock you currently hold — use the available Robinhood MCP earnings-calendar tool (see [Robinhood's tool documentation](https://robinhood.com/us/en/support/articles/trading-with-your-agent/)), falling back to web search if needed. For each holding, compute how many trading days until its next report and flag it:

| Days to earnings | Flag | Action |
|---|---|---|
| Reports today or next session (0–1 days) | **HIGH gap risk** | Note it prominently in the log; the position carries binary event risk into the report. This is a proactive warning — the ATR stop still handles the aftermath, but you must not be surprised by it. |
| 2–3 trading days out | Elevated | Flag in the log; do NOT add to the position (see below) |
| 4+ trading days out | Normal | No action |

Also, at the start of each week, check the broader calendar (e.g. `AI semiconductor earnings this week`) so upcoming reports across the watchlist are known in advance.

**Earnings risk management:**
- Do NOT add to a position within 3 trading days of earnings (IV is elevated; risk is binary)
- Do NOT open a NEW position within 3 trading days of its earnings (`06_risk_management.md` Do-Not-Trade checklist)
- Consider trimming 25% of oversized positions before earnings to manage event risk
- After earnings: wait for the initial reaction to settle (usually by end of day) before reacting
- A beat-and-raise that sells off after initial pop = institutional distribution — consider trimming
- An earnings gap-down >8% on above-average volume is a hard exit trigger (`06_risk_management.md` Exit Rules — D)

**Why this is mandatory every session:** several positions (DDOG on 2026-08-06; MU/ASML/KLAC/META/AMD in the prior month) were lost to same-day earnings gaps. The stop mechanism handled the aftermath correctly, but proactive per-holding earnings awareness lets you see the gap risk coming rather than only reacting to it.

---

## News Score (0–5)

| Score | Conditions |
|---|---|
| 5 | Major positive catalyst (contract win, earnings beat, hyperscaler capex raise) + macro tailwind |
| 4 | Positive company news + neutral macro |
| 3 | No significant news; macro neutral |
| 2 | Negative company news OR macro headwind (rate hike, recession signal) |
| 1 | Material negative news (miss + guidance cut OR investigation) |
| 0 | Thesis-breaking news; immediate reassessment required |
