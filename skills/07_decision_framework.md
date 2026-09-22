---
file: 07_decision_framework.md
purpose: How to turn signals into entry decisions, and how decisions are documented
---

# Decision Framework

## Overview

The order is fixed:

1. **Eligibility gates** (Step 0) — cheap, binary checks, applied before any deep analysis
2. **Analysis and scoring** (Step 1) — only for candidates that passed Step 0
3. **Score thresholds** (Step 2)
4. **Bull/bear debate** (Step 3)
5. **Sizing within the risk limits** (`06_risk_management.md`)
6. **Documentation, then execution** (Step 5)

Existing holdings are always analyzed, but only to evaluate the Exit Rules in `06_risk_management.md`. An add to a holding is a new entry and goes through the full sequence above.

**Decision hierarchy**: risk management overrides everything. A perfect score means nothing if the trade breaks a hard limit.

---

## Step 0 — Eligibility Gates (checked BEFORE analysis and scoring)

Apply these to every candidate for a new entry or add before spending any effort on deep analysis. A candidate is ineligible unless ALL are true:

1. **Regime**: SPY is more than ~1.5% above a rising 50-day SMA — not Bear, not Trendless (`00_overview.md`)
2. **Market conditions**: VIX ≤ 30, and SPY is not down more than 2% from its prior close at the time of the session
3. **Trend filter**: the stock is above its own **rising 50-day SMA** and above its **200-day SMA** (`03_technical.md`)
4. **Earnings**: the stock does not report within 3 trading days, and did not report within the last full session (`05_news_macro.md`)
5. **Re-entry lockout**: not sold at a loss within the last 5 trading days — unless price has since reclaimed its 50-day SMA on above-average volume, or shows a confirmed bullish RSI divergence
6. **Once per symbol per day**: no trade already executed in this symbol today
7. **Sector**: the stock's sector proxy ETF is not in breakdown (`06_risk_management.md` Exit Rule E)
8. **Data quality**: at least 200 daily bars of price and volume history are available, current, and internally consistent — no stale reads and no mismatched bar cutoffs

**If any gate fails: record the reason and stop.** Do not analyze or score the candidate, and never let a high score compensate for a failed gate.

Two further hard checks come later because they depend on the analysis — the score thresholds (Step 2) and the risk limits at sizing (`06`). They are just as binding; Step 0 is the first filter, not the only one.

---

## Step 1 — Composite Score

Each of the four domains produces a score from 0 to 5. Combine them into a weighted total, then normalize it to 0–20:

| Domain | Score (0–5) | Weight |
|---|---|---|
| Fundamental (`02_fundamental.md`) | ___/5 | 1.5× |
| Technical (`03_technical.md`) | ___/5 | 1.0× |
| Sentiment (`04_sentiment.md`) | ___/5 | 0.75× |
| News/Macro (`05_news_macro.md`) | ___/5 | 1.25× |

**Weighted total** = (Fundamental × 1.5) + (Technical × 1.0) + (Sentiment × 0.75) + (News × 1.25). The maximum is 22.5.

**Final Score** = (Weighted Total / 22.5) × 20

**Why these weights?**
- Fundamental is highest because it drives long-term value
- News/Macro is second because near-term catalysts and regime can override fundamentals temporarily
- Technical is third because price action reflects consensus; useful for timing
- Sentiment is lowest because it is the most subject to noise and manipulation

### No double-counting — each fact belongs to exactly ONE domain

The composite is a **ranking aid, not a probability forecast**, and it is only meaningful if the four domains stay independent. One event that lifts three domains at once inflates the score threefold on a single piece of information.

The worst offender is earnings. A beat can naively lift Fundamental (*the beat*), News (*positive catalyst*), **and** Sentiment (*the upgrades that follow*) — one fact counted three times, systematically inflating composites after every report. Ownership:

| Fact | Owned by | Explicitly NOT counted in |
|---|---|---|
| Earnings results, guidance, margins, EPS/revenue surprise | **Fundamental** | News, Sentiment |
| Insider buying and selling (Form 4) | **Fundamental** | News, Sentiment |
| Analyst rating and price-target changes | **Sentiment** | News, Fundamental |
| Short interest, options flow, social sentiment, 13F flows | **Sentiment** | News |
| The market's *reaction* to an event (gap, sell-the-news, breakout) | **Technical** | News |
| Non-earnings company events: contracts, M&A, regulatory, product, offerings | **News** | Fundamental |
| Macro and sector-level developments | **News** | all others |

When a fact could plausibly sit in two domains, score it in its owner and treat it as **neutral** in the other.

**Missing data:** if a domain's inputs are genuinely unavailable (for example, thin sentiment coverage on a small name), score that domain a **neutral 3** and note reduced confidence in the log. **Do not invent evidence** to fill a gap.

---

## Step 2 — Score Thresholds

### For new entries and adds (after passing Step 0)

| Final score | Zone | Decision |
|---|---|---|
| 17–20 | Strong Buy | Eligible — high-conviction sizing (`06`) |
| 14–16 | Buy | Eligible — medium-conviction sizing |
| 11–13 | Weak Buy | Not eligible — **unless the momentum-breakout exception applies** |
| ≤ 10 | — | Not eligible |

**Momentum-breakout exception**: if the most recent completed daily close was a new 52-week high on volume ≥ 1.5× its 30-day average, a score of 11–13 is eligible at starter size. A breakout on volume signals institutional conviction, and the overbought RSI that accompanies it must not veto the entry (`03_technical.md`). The exception lowers the score threshold only — every other check still applies.

**Threshold adjustments — all binding. When more than one applies, the highest minimum wins** (for example, a breakout scoring 12 in a concentrated sector needs 15, so it is not eligible):
- **Concentrated sector** (2 or more holdings already in the candidate's sector, per `01_universe.md`): the minimum score is **15**
- **Bull — Defensive regime** (VIX 25–30): the minimum score is **17**
- **Fundamental sub-score of 0** (thesis broken): not eligible at any composite score
- A **Fundamental sub-score ≤ 2** does not block the entry, but caps its sizing at medium conviction (`06`)

### For existing holdings — the score is diagnostic, never an exit order

| Final score | Meaning |
|---|---|
| 11–20 | Healthy — hold |
| 6–10 | Watch — re-examine the thesis; no action on the score alone |
| 0–5 | Deteriorating — if the position has been held more than 5 trading days, the **deterioration stop** fires (`06` Exit Rule D); otherwise diagnostic |

Exits are governed **exclusively** by the Exit Rules in `06_risk_management.md`. The deterioration stop is the only score-based exit, and it exists so a genuinely broken holding cannot linger — not as a license to act on every score dip.

Why this matters: a winner that has run will often see its technical sub-score fall *because* it is extended. If a low score alone could force a sale, the framework would systematically sell its best trades — the exact inversion `00_overview.md` tenet 3 forbids.

---

## Step 3 — Bull/Bear Debate

The debate applies to exactly two situations:
- **Every new entry or add** that has passed Step 0 and met its Step 2 threshold
- **The thesis stop** — the one judgment-based exit in `06`

**Mechanical exits are executed without debate**: the initial stop, the trailing stop, the trend break, the underwater death cross, the earnings gap-down, the deterioration stop, the time stop, the loss backstop, and the portfolio controls. Deliberating over a triggered stop is how losers get held (`00_overview.md` tenet 2).

### Before an entry — argue the bear case
- What is the strongest argument that this stock goes DOWN from here?
- Is there a catalyst that could break the thesis in the next 30 days?
- What must the stock do to justify its valuation? Is that realistic?
- Who is on the other side of this trade, and why might they be right?

### Before a thesis-stop exit — argue the bull case
- Is the negative event verified from a primary source (a filing or company release), or is it only a headline?
- Does it genuinely break the original thesis, or is it noise an intact trend can absorb?
- Is price still above a rising 50-day SMA? The market's own verdict counts.

(This debate never authorizes *buying* weakness: the trend filter in Step 0 still forbids entries below the 50-day SMA.)

**Rule**: if you cannot clearly refute the opposing case, **do nothing** — for an entry, no trade; for a thesis stop, keep holding under the mechanical stops. Half-sizing does not resolve an unrefuted bear case; it just takes a smaller version of a trade you could not justify. Uncertainty is not a reason to trade at any size (`00_overview.md`: "When in doubt, do nothing").

---

## Step 4 — Final Decision Matrix

| Situation | Action |
|---|---|
| Candidate passes Step 0, meets its Step 2 threshold, and the bull case clearly dominates | Size per `06` and execute |
| Candidate passes Step 0 and its threshold, but a material bear argument remains unrefuted | No trade |
| Candidate fails any gate, threshold, or sizing limit | No trade — record the reason |
| Holding: a mechanical Exit Rule fired | Exit per `06` — no debate |
| Holding: thesis-stop evidence is present | Bull-case debate; exit only if the thesis is verifiably broken |
| Holding: no Exit Rule fired | Hold — regardless of the score |

---

## Step 5 — Documentation (Required)

**For every trade**, write before placing the order:

```
Trade: [BUY/SELL] $[amount] of [TICKER]
Exit rule (sells only): [which Exit Rule in 06 fired]
Score: [X]/20 (F:[x] T:[x] S:[x] N:[x])
Fundamentals: [FRESH | REUSED from an earlier session today] — [2–3 drivers, e.g. "Q3 beat +12%, guidance raised, PEG 1.8"]
Thesis: [1–2 sentences on why this trade makes sense]
Bull case: [strongest supporting argument]
Bear case: [strongest opposing argument, and why it is still right to act]
Invalidation: [what would make this trade wrong]
Initial stop: $[price] ([X]% below entry; ATR14 used: $[x]; multiple: [x]×)
R: $[entry − initial stop]
```

**For every holding, in EVERY session log** (whether or not it traded), record its state:

```
[TICKER] entry [YYYY-MM-DD] | avg cost $[x] | initial stop $[x] | R $[x] | active stop $[x] | highest close since entry $[x] | progress [x.x]R
```

Sessions are stateless, and the broker does not store these values — without them, the R-multiple trailing stop, portfolio heat, and the time stop cannot be computed. Carry each value forward from the previous log, and update the active stop only upward. If a holding's initial stop cannot be recovered from the logs (for example, a position opened before this format existed), reconstruct it with the ATR method as of the entry date; if even that is impossible, set it from the current ATR, and say so in the log.

---

## Step 6 — Weekly Learning

At the **last session of each trading week**, add a **Weekly Review** to the log:

1. Which trades this week were profitable? What signal patterns led to them?
2. Which trades lost money? Was the score high at entry? What was missed?
3. Are there recurring patterns in what is working and what is not?
4. Should any skills rule be adjusted based on recent performance?

This is how the agent improves over time — not by predicting better, but by recognizing patterns in its own decision quality.

---

## Quick Reference

*Summary only. Step 0, Step 2, and `06_risk_management.md` are authoritative; if this summary ever disagrees with them, they win.*

**To enter or add, all of these must hold:**
- Every Step 0 gate passes
- The Step 2 threshold is met: ≥ 14 normally; ≥ 15 in a concentrated sector; ≥ 17 in Bull — Defensive; ≥ 11 under the momentum-breakout exception; never with a Fundamental sub-score of 0
- The bull case clearly beats the bear case
- The trade fits every sizing cap in `06`, including portfolio heat
- An add is a new entry for all of the above

**The only ways a position is sold (`06` Exit Rules):**
- Initial stop (A)
- R-multiple trailing stop (B): breakeven at +2R; ATR trail from +3R
- Trend break — full exit (C)
- Underwater death cross — 50% (C)
- Thesis stop — the only judgment exit (D)
- Earnings gap-down (D)
- Deterioration stop: score ≤ 5, held more than 5 trading days (D)
- Time stop: at or below cost after 20 trading days (D)
- Rebalancing trim or opportunity swap — never on a healthy winner (E)
- Loss backstop: more than 20% below cost (E)

**Never reasons to sell on their own:** high RSI, extension above the 50-day SMA, a lower score (other than the deterioration stop), an analyst downgrade, a sector ETF breakdown, or being "up a lot."

**No new entries — but every exit still fires:** a Bear or Trendless regime, VIX above 30, or SPY down more than 2% on the day.

**Hold** any position for which no Exit Rule has fired.
