---
file: 05_news_macro.md
purpose: Non-earnings company news, macroeconomic context, sector catalysts, and the earnings calendar
---

# News & Macro Analysis

## Data Sources

All news and macro data comes from web search. Run targeted searches at the start of each session:
- Company-specific: `"TICKER" news today` and `"TICKER" news last 48 hours`
- Sector: `AI semiconductor news today`, `cloud computing news today`
- Macro: `Fed news today`, `inflation data`, `10-year treasury yield`

**What this file does NOT score** (ownership table, `07_decision_framework.md` Step 1): earnings results and guidance, and insider activity, are scored in `02_fundamental.md`; analyst rating changes in `04_sentiment.md`; and the market's price reaction to any event in `03_technical.md`. Scoring them here as well would count the same fact twice.

---

## 1. Company-Specific News

### High-Impact Events (score inputs — not exit authority)

The non-earnings events below set the **News sub-score** and inform the thesis. None of them authorizes a sale on its own: every exit must come from an Exit Rule in `06_risk_management.md`. A severe event can meet the **thesis stop** definition there — invoke that rule by name rather than improvising a "reduce exposure" trade.

| Event | Impact | Response |
|---|---|---|
| Earnings results and guidance | — | **Scored in `02_fundamental.md`, not here.** A miss with a guidance cut meets the thesis-stop definition in `06`. |
| Major contract win (government, hyperscaler) | Positive | Validate its size and strategic fit before scoring |
| Product launch or technology breakthrough | Positive (if material) | Assess whether it changes the competitive position |
| CEO/CFO departure (unplanned) | Negative | Lower the News score; verify the primary source. It is a thesis stop only if it breaks the thesis — not automatically |
| DOJ/SEC investigation, major lawsuit | Negative | Verify via the primary filing, then assess it against the **thesis stop** test in `06` |
| Acquisition announcement (company is the acquirer) | Often negative short-term (premium paid) | Evaluate the strategic logic |
| Acquisition announcement (company is the target) | Strong positive | Premium typically 20–40% above market price |
| Share buyback **authorization** (large, >5% of float) | Mildly positive | Signals management confidence. **An authorization is not execution and creates no price floor** — do not treat it as downside protection |
| Dividend cut | Negative | Signals financial stress |

### Medium-Impact Events (Consider in Context)

| Event | Impact |
|---|---|
| Analyst day / investor day | Positive if new products are unveiled or targets raised (guidance itself is scored in `02`) |
| Supply chain disruption | Negative for manufacturers; positive for some competitors |
| Partnership announcement | Positive if material; many are PR |
| Share offering / dilution | Negative — dilutes existing shareholders |
| Stock split | Neutral (doesn't change value; may improve retail accessibility) |
| Index inclusion (S&P 500, Nasdaq 100) | Positive — forced buying from index funds |
| Index exclusion | Negative — forced selling |

---

## 2. Macroeconomic Context

Check at the start of each session. Macro conditions affect every stock, so they enter each candidate's News score through its macro component. (The market *regime* — whether new entries are allowed at all — is set separately by SPY and VIX in `00_overview.md`.)

### Federal Reserve & Interest Rates

| Condition | Impact on AI/Tech Stocks |
|---|---|
| Rate cut or dovish pivot signal | Strongly positive — growth stocks re-rate higher (lower discount rate) |
| Rate hike or hawkish surprise | Negative — compresses multiples on growth stocks |
| Rates stable / "higher for longer" | Mildly negative; requires earnings growth to drive stock appreciation |
| 10-year treasury > 5% | Challenging environment for high-multiple growth stocks |
| 10-year treasury < 4% | More supportive of growth stock valuations |

**Rule**: a 10-year yield rising fast (more than 30bps in a month) is a macro headwind — score it as such in the News score of high-P/E AI names.

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
| Strong jobs report | Mixed — good for the economy but may delay Fed cuts |
| Weak jobs report | Mixed — recession concern vs. rate-cut hope |

---

## 3. Sector-Specific AI Catalysts

Recurring themes that can move entire sectors. For a supplier, another company's announcement (for example, a hyperscaler raising capex) is a sector development, so it is scored here.

| Catalyst | Affected Stocks | Direction |
|---|---|---|
| Hyperscaler (MSFT/GOOGL/AMZN/META) capex guidance raised | NVDA, AMD, AMAT, LRCX, SMCI, VRT | Strongly positive |
| Hyperscaler capex guidance cut | Same as above | Strongly negative |
| New Nvidia GPU architecture announcement | NVDA (positive), AMD (pressure), HBM suppliers MU/SNDK/SKHY | |
| TSMC capacity expansion | AMAT, LRCX, ASML, SNPS, CDNS | Positive |
| US-China chip export restrictions tightened | NVDA, AMD, AMAT (negative short-term); domestic beneficiaries | |
| New AI model release (OpenAI, Google, Anthropic) | Positive for AI infrastructure stocks broadly | |
| Enterprise AI adoption slowdown reports | Negative for software AI plays (PLTR, SNOW, DDOG) | |
| Data center power constraints | CEG, VST, NRG, GEV (positive); limits hyperscaler growth (negative) | |

---

## 4. Earnings Calendar

**Mandatory every session for ALL current holdings.** At the start of every session, pull the next earnings date for each stock you hold — use the Robinhood MCP earnings-calendar tool (see [Robinhood's tool documentation](https://robinhood.com/us/en/support/articles/trading-with-your-agent/)), falling back to web search. For each holding, count the trading days until its report and flag it:

| Days to earnings | Flag | Action |
|---|---|---|
| Reports today or next session (0–1 days) | **HIGH gap risk** | Note it prominently in the log; the position carries binary event risk into the report. This is a warning, not an exit — the stop rules still handle the aftermath, but you must not be surprised by it. |
| 2–3 trading days out | Elevated | Flag it in the log; no adds (see below) |
| 4+ trading days out | Normal | No action |

At the start of each week, also check the broader calendar (e.g. `AI semiconductor earnings this week`) so upcoming reports across the watchlist are known in advance.

**Earnings rules:**
- **No new position and no add within 3 trading days before a report** — IV is elevated and the risk is binary (`07_decision_framework.md` Step 0, gate 4).
- **No new position and no add until one full session after a report** has passed, so the initial reaction can settle.
- **The waiting rules delay entries only — never exits.** The stops and the earnings gap-down exit fire immediately.
- **Earnings gap-down**: a gap down of more than 8% on the earnings day on above-average volume is a hard exit (`06_risk_management.md` Exit Rule D).
- **There is no discretionary pre-earnings trim.** The defined pre-earnings controls are the no-add and no-entry rules above and the gap-down exit. A size-based pre-earnings trim would have to be added to `06` as an explicit rule with a numeric threshold — it is not improvised here.
- A beat-and-raise that sells off after the initial pop is a **price reaction**, so it is scored in Technical (`03`), and the holding remains subject to the trend-break rule. It is not an exit on its own.

**Why this check is mandatory:** several positions (DDOG on 2026-08-06; MU/ASML/KLAC/META/AMD in the prior month) were lost to earnings gaps. The stop mechanism handled the aftermath correctly, but per-holding earnings awareness lets you see the risk coming rather than only reacting to it.

---

## News Score (0–5)

Covers non-earnings company events, sector catalysts, and the macro backdrop. Earnings and insider activity are scored in `02`.

| Score | Conditions |
|---|---|
| 5 | Major positive non-earnings catalyst (large contract win, acquisition target, index inclusion, a strong sector tailwind such as a hyperscaler capex raise) + supportive macro |
| 4 | Positive company or sector news + neutral macro |
| 3 | No significant news; macro neutral |
| 2 | Negative company or sector news OR a macro headwind (rate hike, recession signal, fast-rising yields) |
| 1 | Material negative non-earnings event (investigation, loss of a major customer, dilutive offering) |
| 0 | A non-earnings event that breaks the thesis — assess it against the thesis stop in `06` |
