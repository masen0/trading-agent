---
file: 06_risk_management.md
purpose: Position sizing, stop-losses, exit rules, portfolio limits, and when NOT to trade
---

# Risk Management

## The Prime Directive

**Never risk more than you can afford to lose on a single trade. Capital preservation enables future opportunities.**

For a small account like this, every dollar matters more. Aggressive sizing on poor setups is the fastest way to wipe out the ability to trade at all.

**Definition used throughout this file:** *account value* = the total value of all positions plus cash (the `total_value` reported by the portfolio tool). Every percentage cap below is a percentage of account value unless stated otherwise.

---

## Position Sizing

### Sizing Rules

Position size depends on the composite score, the Fundamental sub-score, and the market regime. A new entry or add normally requires a composite score of at least 14 (11 under the momentum-breakout exception) — the thresholds are defined in `07_decision_framework.md` Step 2.

Sizing is a **two-step calculation**: compute a candidate size, then clamp it by every portfolio cap. **The final size is the smallest of all of them — the caps always win.**

**Step 1 — candidate size (% of available buying power):**

| Conviction band | Score | Candidate size | Initial stop multiple (Exit Rule A) |
|---|---|---|---|
| High conviction | 17–20 | 40% of buying power | 2.5 × ATR14 |
| Medium conviction | 14–16 | 25% of buying power | 2.0 × ATR14 |
| Starter (momentum-breakout exception only) | 11–13 with a confirmed breakout (`07` Step 2) | 10% of buying power | 1.5 × ATR14 |

**Adjustments — apply every one that fits:**

- **Fundamental cap**: if the Fundamental sub-score is ≤ 2, the setup may not use the high-conviction band. Size it — and set its stop multiple — as medium conviction at most, even if the composite is 17+ (`02_fundamental.md`).
- **Regime** (defined in `00_overview.md`):
  - Bull — Strong: no change.
  - Bull — Cautious (VIX 18–25): halve the candidate size.
  - Bull — Defensive (VIX 25–30): only setups scoring ≥ 17 qualify (`07` Step 2), and the candidate size is capped at 10% of buying power.
  - Bear, Trendless, or VIX > 30: no new entries or adds.

**Step 2 — clamp by the portfolio caps (all measured after the trade):**

- **Single stock**: no single stock above **25%** of account value
- **Sector**: no sector above **40%** of account value (sectors per the Canonical Sector Map in `01_universe.md`)
- **Combined semiconductors**: Memory + Chips + Equipment together no more than **50%** of account value
- **Portfolio heat**: total open loss-risk no more than **6%** of account value (defined below)
- **Cash reserve**: at least **$20** of buying power must remain
- **Minimum size**: if the final size is below **$20**, skip the trade rather than placing a token position

```text
final_size = min(
    candidate_size,
    0.25 × account_value − current value held in this stock,
    0.40 × account_value − current value held in this sector,
    0.50 × account_value − current combined semiconductor value   (semiconductor candidates only),
    (0.06 × account_value − current_heat_dollars) / stop_pct,
    buying_power − $20
)
```

`stop_pct` is the new position's initial stop distance as a fraction of the entry price (Exit Rule A). At entry, price equals cost, so a new position adds `size × stop_pct` of heat.

**Why the clamp matters:** the candidate size is a percentage of *buying power* while the caps are percentages of *account value*, so on a small account the candidate regularly exceeds a cap. Example: account value $400, all in cash, score 18 → candidate = 40% × $400 = **$160**, but the 25% single-stock cap allows only **$100**. The correct size is **$100**. Never take the larger number.

**Worked example (normal case)**: $200 buying power in a $400 account, Bull — Strong, score 15 → candidate = 25% × $200 = $50. Single-stock headroom $100; sector headroom fine; stop 8.4% → the position adds $4.20 of heat, which fits. Final size = **$50**.

### Portfolio Heat — the aggregate risk cap

Concentration caps limit how much sits in any one name or sector; they say nothing about **how much would be lost if every stop hit at once.** Portfolio heat closes that gap.

```text
position_risk  = max(0, min(current_price, avg_cost) − active_stop) × quantity
portfolio_heat = Σ position_risk / account_value
```

**Heat may not exceed 6% of account value.** If a proposed entry or add would push heat above 6%, reduce its size until it fits, or skip it.

Two things to understand about this definition:

- It measures **risk of an actual realized loss**, not give-back of unrealized gains. A position whose active stop has ratcheted to breakeven or above contributes **zero heat** — it cannot produce a loss. This is deliberate: measuring from the *current price* would make a large winner look like the riskiest thing in the book and would throttle new entries precisely because the strategy is working.
- It is a **planning estimate, not a guarantee.** Logical stops are checked only at scheduled sessions, so a gap can carry a position through its stop and realize more than the budgeted loss.

Heat is recomputed every session, because active stops move.

### Adding to a Position — Winners Only

Consistent with the trend-following strategy (`00_overview.md`), you add ONLY to winners, never to losers.

**An add is a NEW ENTRY for every gate and limit.** It creates fresh long exposure, so it must clear every eligibility gate in `07_decision_framework.md` Step 0, the score threshold in Step 2, and every sizing limit in this file — exactly as a brand-new position would. It also uses up that symbol's one permitted trade for the day. **Satisfying the winners-only conditions below never waives any of those.**

**Adding to winners (pyramid up)** — permitted only when all of the above pass AND:
- The stock is above your average cost (the position is already working)
- The stock is above a rising 50-day SMA (the trend is intact)
- The add is ≤ 50% of the original position size (avoid overweighting late entries at higher prices)
- Before sizing, recompute the blended average cost and the position's heat, since the add changes both

**Adding to losers (averaging down)** — FORBIDDEN.
- If a position is below your cost, the trend thesis is failing. Do not add under any circumstances.
- The only actions on a losing position are: hold under the Exit Rules, or exit under them. Never add.
- Rationale: averaging down inverts the trend-following asymmetry — it grows your losers and shrinks the capital available for winners. This was the primary driver of losses in the 2026-07-13 week.

---

## Exit Rules — Consolidated (single source of truth)

**Every exit in this system is one of the rules below. No other file may authorize a sale** — other files only feed these rules with information.

### How and when stops are actually evaluated (read this first)

**These are *logical* stops, not live broker-side stop orders.** Nothing is resting at the exchange. A stop exists only as a recorded price level that the agent checks when it runs. This has consequences you must not paper over:

- The agent runs only at its **scheduled sessions during regular market hours**. A stop can be acted on only at one of those moments — never in between.
- **No scheduled session coincides with the market close.** At the day's first session the most recent completed close is the *previous* trading day's; at any later intraday session, no close exists for the current day. So a strict closing-basis test is evaluable only at the **first session of the day, against the prior day's close**.
- Between sessions — and overnight — nothing is watching. **Logical stops do not bound overnight gap risk or intraday moves.** A position can trade far below its stop and be exited only at the next session, at whatever price then prevails.

**The stop rule, stated honestly:**

1. **Primary (closing basis)**: at the **first session of each trading day**, compare the prior day's official close to the active stop. If it closed at or below the stop → exit in full at market, regular hours.
2. **Secondary (intraday breach)**: at **any** session, if the live price is at or below the active stop by a **material margin (≥ 2%)**, exit in full without waiting for a close. A modest wick just below the stop is not actionable intraday; a decisive breach is.
3. Record which of the two triggered the exit.

Because stops cannot protect against gaps, **position sizing — not the stop — is the real risk control.** Size assuming the stop may be jumped.

**The governing principle** (`00_overview.md`): losers are cut fast at a pre-set stop; winners are exited only by a rule in this section — the trailing stop, a confirmed trend break, or a verified thesis or event stop — **never by discretion.** Never sell a healthy, trending position on high RSI, a lower score, or because it is "up a lot."

**Mechanical vs. judgment exits.** Every exit below is **mechanical** — executed without debate — except the **thesis stop**, which requires verifying that the thesis is actually broken. The thesis stop is the only exit to which the bull-case debate in `07_decision_framework.md` Step 3 applies. Deliberating over a triggered mechanical stop is how losers get held.

**Precedence.** If a full-exit rule and a partial-exit rule fire together, the full exit wins. Never submit overlapping sell orders; after each fill, re-read the remaining quantity before applying another rule.

### A. Initial stop (for every new position) — set at entry, never widened

The initial stop is fixed at the moment you enter, using ATR(14) and the multiple for the conviction band actually applied (see the sizing table above: 2.5× high, 2.0× medium, 1.5× starter).

**How to compute the stop (mandatory — do NOT use a rounded or eyeballed number):**

1. Pull ATR(14) for the stock (`03_technical.md`).
2. `raw_stop = entry_price − (multiple × ATR)`.
3. Convert to a percentage: `stop_pct = (entry_price − raw_stop) / entry_price`.
4. Clamp: if `stop_pct < 5%`, set the stop at exactly 5% below entry; if `stop_pct > 20%`, set it at exactly 20% below entry. Otherwise use the ATR-derived level unchanged.
5. Record the exact dollar stop, the resulting %, and the ATR value used.

**Worked example**: entry $100, medium conviction (2.0×), ATR = $4.20 → raw_stop = 100 − (2.0 × 4.20) = $91.60 → stop_pct = 8.4%. That is within the 5–20% band, so the stop is **$91.60 (−8.4%)** — not "−8%", not "−10%", not any rounded figure.

- **Never substitute a round number** (−8%, −10%, −15%) for the ATR-derived level. Rounding tighter than ATR implies stops you out of intact positions on normal volatility (this happened with the NOW exit on 2026-07-24); rounding wider over-risks capital. The ATR math is the stop.
- **Floor** 5%, so normal noise does not shake you out. **Ceiling** 20% — AI/tech names can move 8–15% in a session, so a tighter ceiling would stop out intact positions.
- **Record once, never change**: the initial stop and `R = entry_price − initial_stop` are fixed at entry and carried in every session log (`07` Step 5). The trailing stop moves the *active* stop — never the initial stop, and never R.
- The active stop starts equal to the initial stop and only ever moves **up**. Never lower it to "give it room."

### B. Trailing stop (for winners) — measured in R

`R = entry_price − initial_stop` — the amount risked per share at entry. A position at "+2R" has gained twice what it risked.

Track the **highest completed daily close since entry**. The active stop ratchets upward and is **never** lowered:

| Progress | Active stop |
|---|---|
| Below +2R | The initial stop |
| At or above +2R | Raised to at least **breakeven** (average cost) |
| At or above +3R | `max(existing_stop, highest_close − 3 × ATR14, breakeven)` |

**Worked example**: entry $100, initial stop $92 → **R = $8**. At $116 the position is +2R (+16%) → stop moves to breakeven ($100). At $124 it is +3R (+24%) → with ATR14 of $4 and a highest close of $124, the candidate is $124 − $12 = $112, so the stop becomes $112 (locking in +12%).

**Why R-multiples instead of fixed percentages:** a fixed ladder (+20% / +40%) ignores both the stock's volatility and how much the trade actually risked. A low-volatility name with a 5% stop reaching +16% has earned **3R** and deserves protection; a high-volatility name with a 20% stop at +16% is not yet at **1R** and needs room. This also fixes a failure we observed: winners repeatedly stalled at +14–17% and **never reached a +20% trigger**, so their gains went unprotected. In R terms those positions were at +2R and would have been ratcheted to breakeven.

**Never take partial profit at an arbitrary percentage gain.** A "sell 25% at +X%" rule is a profit target, and profit targets cap the upside this strategy depends on — the asymmetry only works if a rare very large winner runs without a ceiling. Protect large gains with the trail, never a scheduled sale. Recompute the trailing level from **completed daily bars only**, and never lower a stored stop when ATR expands or price pulls back.

### C. Trend-break exits

- **Trend break**: a completed daily close below the 50-day SMA on above-average volume → exit in full. Because it is a closing-basis signal, it is evaluated at the day's first session against the prior day's completed bar.
- **Underwater death cross**: the 20-day SMA crosses below the 50-day SMA while the position is more than 10% below its average cost → sell 50% of `shares_available_for_sells`. A death cross on a position that is *not* more than 10% underwater is a watch item, not an exit.
- A bearish MACD divergence may be noted as confirmation alongside a trend break — never as an exit on its own.

### D. Thesis, event and time stops

- **Thesis stop** *(the only judgment exit)*: verified evidence that the original thesis is broken by a material event — earnings miss with a guidance cut, loss of a major customer, a disruptive competitor, regulatory action → exit in full. Verify the event from a primary source (filing or company release, not a headline) and run the bull-case debate (`07` Step 3) first.
- **Earnings gap-down**: the position gaps down more than 8% on its earnings day on above-average volume → exit in full at the first session where the gap is observed. Do not wait for the reaction to "settle."
- **Deterioration stop**: composite score ≤ 5 on a position held more than 5 trading days → exit in full. This is the only score-based exit in the system.
- **Time stop**: the position is at or below its average cost after 20 trading days, and no scheduled catalyst (such as earnings) falls within the next 10 trading days → exit in full to redeploy the capital. **Never applies to a position above its cost.**

### E. Portfolio controls

- **Sector ETF breakdown**: the sector's proxy ETF (Canonical Sector Map, `01_universe.md`) closes below its 50-day SMA on volume above 1.5× its 30-day average → **blocks new entries and adds** in that sector and triggers a review of its holdings. It does **not** authorize a sale; each holding still exits only under its own rules.
- **Rebalancing trim**: a sector is above 40% of account value AND a candidate from a different sector has passed every gate and scored ≥ 14 → sell from the over-limit sector's **lowest-scoring** holding, only enough to bring the sector back to 40%, to fund that entry.
- **Opportunity swap**: a holding scores ≤ 10 AND a candidate that has passed every gate scores ≥ 16 → sell the weak holding to fund the stronger one.
- **Guard for both the trim and the swap: never sell a position that is above its average cost AND above a rising 50-day SMA.** An extended winner can score low purely because extension depresses its technical sub-score; selling it to fund something else would be cutting a winner early through the back door. If every holding in an over-limit sector is a healthy winner, no trim happens — concentration that comes from appreciation is tolerated, and the sector cap simply blocks further additions there.
- **Loss backstop**: any position more than 20% below its average cost → exit in full. The initial stop's 20% ceiling should prevent this, so if it ever fires, flag it in the log as a missed or gapped stop.

---

## Portfolio-Level Risk Controls

### Drawdown Limits

Account drawdown is measured from the **high-water mark of account value, adjusted for deposits and withdrawals**, so that funding changes are not mistaken for performance.

| Scenario | Response |
|---|---|
| A position more than 12% below its average cost | Mandatory review — confirm its active stop is recorded; no adds |
| A position more than 20% below its average cost | Loss backstop (Exit Rule E) |
| Account more than 15% below its high-water mark | No new entries or adds; exits continue under the Exit Rules |
| Account more than 25% below its high-water mark | No new entries or adds; flag prominently in the log for owner review. Exits continue only under the Exit Rules — the agent does not sell discretionarily to "raise cash." |

### Rebalancing Authority

The agent may sell any holding — including Tier 1 positions — whenever an Exit Rule or portfolio control above requires it; no holding is exempt from those rules. Conversely, **no sale is permitted outside them**: a wish to free capital, or a hunch that something else is better, is not by itself a reason to sell.

### Correlation Risk

Before any new entry, check the sector distribution of current holdings (map each symbol with the Canonical Sector Map in `01_universe.md`).

- If 2 or more current holdings are in the same sector, that sector is **concentrated** — a candidate from it needs a score of **≥ 15** (`07` Step 2)
- Prefer entries that broaden the book to at least 3 sectors before deepening any one
- Correlated positions move together: a negative event for one will likely drag the others. The combined-semiconductor cap and portfolio heat exist because correlated positions are not independent risks

### Do Not Trade Checklist

Every item must be clear before any new entry or add. The items are grouped by *when* they are checked; `07_decision_framework.md` is authoritative for the first three groups.

**Before analysis — market (`07` Step 0)**
- [ ] SPY is below its 50-day SMA (Bear), or within ~1.5% of it or with a flat 50-day SMA (Trendless)
- [ ] VIX > 30
- [ ] SPY is down more than 2% from its prior close at the time of the session

**Before analysis — the stock (`07` Step 0)**
- [ ] The stock is not above a **rising 50-day SMA**
- [ ] The stock is below its **200-day SMA**, or fewer than 200 daily bars are available
- [ ] The stock reports earnings within 3 trading days, or reported within the last full session (`05_news_macro.md`)
- [ ] The stock already had a trade today (once per symbol per day)
- [ ] The symbol was sold at a loss in the last 5 trading days without a confirmed reversal (re-entry lockout)
- [ ] The stock's sector proxy ETF is in breakdown (Exit Rule E)
- [ ] Price or volume data is stale, incomplete, internally inconsistent, or drawn from mismatched bar cutoffs

**After scoring (`07` Step 2)**
- [ ] Composite below 14 (below 11 under the momentum-breakout exception)
- [ ] Composite below 15 while the sector is concentrated
- [ ] Composite below 17 in a Bull — Defensive regime
- [ ] Fundamental sub-score of 0

**At sizing (this file)**
- [ ] No valid ATR-derived stop can be computed
- [ ] The trade would breach any cap — single stock 25%, sector 40%, combined semiconductors 50%, heat 6%, or the $20 cash reserve — or the final size is below $20

---

## Position Review Cadence

| Holding duration | Action |
|---|---|
| < 5 trading days | Check the thesis and the active stop; no bias toward exiting |
| 5–20 trading days | Re-validate the thesis with a fresh fundamental and technical check (the intraday reuse rules in `02_fundamental.md` still apply within a day) |
| > 20 trading days | Re-validate the **trend and thesis**: if price is above a rising 50-day SMA and the thesis is intact, HOLD — regardless of how extended the position is. A position still at or below its cost may be subject to the time stop (Exit Rule D). |
| > 60 trading days | Mandatory thesis statement review; exit only if an Exit Rule fires |

**What a review is NOT:** do not ask "would I buy this today at this price?" as an exit test. A winning position that has run is, by construction, extended — it would fail that test precisely *because* it is working, and the rule would systematically sell your best trades (`00_overview.md` tenet 3). Entry and holding are different questions: you need a good *price* to enter, but only an intact *trend* to hold. A review can exit a position only through an Exit Rule above.
