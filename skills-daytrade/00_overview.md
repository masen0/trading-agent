---
file: 00_overview.md
purpose: Goal, account constraints, schedule, session workflow
---

# Day-Trading Agent — Overview

**Goal:** grow the Agentic account (`account_number='494502131'`) by day trading. Every position is opened and closed the same day.

**Full sessions read files 00–04. The guard reads only `05` (plus the sections of `03` and `04` it names).** `99_rationale.md` holds the reasoning and history. Do not read it during sessions.

| File | Purpose |
|---|---|
| `00_overview.md` | Constraints, schedule, workflow |
| `01_universe_and_scan.md` | Universe and the in-play scan |
| `02_setup_orb.md` | The one entry setup |
| `03_risk_and_exits.md` | Sizing, stop replay, exits, limits |
| `04_execution_and_log.md` | Orders and logging |
| `05_guard.md` | The 10-minute guard routine |

## Strategy
After 10:00 ET, find S&P 500 / Nasdaq-100 stocks whose first 30 minutes traded unusually heavy volume ("in play") and closed green. Buy (in dollars, fractional) only on a break above that opening range's high, with the stock and QQQ above VWAP. The stop is the middle of the opening range, kept as a logical stop and rebuilt from price history at every run. Raise it as the trade works. Sell everything at about 15:37. Each trade risks about 2% of the account.

## Account constraints
1. **Cash account:** long only, no margin. PDT rules do not apply.
2. **T+1 settlement:** `buying_power` is settled cash only. Each dollar funds **one round trip per day**. Never trade with unsettled funds.
3. **Fractional shares, market orders only.** Never place `limit`, `stop_market`, or `stop_limit` orders. Stops are logical (`03`). A stop-out can fill below the stop, and nothing protects a position between runs or overnight, so **the end-of-day flatten is never skipped.**
4. **This system owns the whole account. Nothing is held overnight.** Any position held from a previous day (`quantity − intraday_quantity > 0`) is sold at market by the first full session that finds it (`03` Exit D). That includes failed flattens and holdings left by any other strategy.

## Schedule (ET; ET = UTC−5 from 2026-11-01 until 2027-03-14, otherwise UTC−4)
Times avoid the top of the hour and :00–:30 morning slots, where the scheduler starts runs late. Runs can still start a few minutes late, so the rules use the actual ET time.
- **Full sessions (8/day):** 10:07, 10:19, 10:37, 10:49, 11:07, 11:19, 11:26, **15:37 (flatten)**.
- **Guard (35/day):** at :03, :13, :23, :33, :43, :53 past each hour, 10:13–15:53 (the :03 slot starts at 11:03).
- **Entries only 10:00–11:30.** The first full session at or after 15:35 flattens.
- **Early-close days** (13:00 close: day after Thanksgiving, Christmas Eve): add a one-off full session at 12:45 that flattens. The guard's backup flatten applies from 12:50.
- **Holidays:** if quotes show no regular session today, stop (no email).

## Full-session workflow (in order)
1. Note the ET time and window (entry window / flatten).
2. Reconcile with the broker (`04` Step 1).
3. **Run the stop replay for every day position** (`03` Exit A). Sell any whose stop was touched, before anything else.
4. Sell **every** position held from a previous day (`03` Exit D). Normally only the first session finds any.
5. Check daily limits and the kill switch (`03`). If either is hit, skip to step 7.
6. In the entry window with buying power: scan (`01`), check entries (`02`), size (`03`), execute (`04`).
7. Flatten if it is the flatten session (`03` Exit C).
8. Flatten session only: send the daily log (`04`). Other runs send no email except the STUCK POSITION alert (`04`).

When in doubt, do nothing. The stop check is never optional.

## Continuity: the broker is the memory
Recompute everything each run. Never read trading state from a log:
- Day positions: `intraday_quantity > 0`. Entry price and time: today's filled buys.
- Initial stop and R: from today's 9:30–10:00 bar (`01`, `03`).
- Current stop and whether it was hit: the stop replay (`03`).
- Today's P&L and loss count: today's filled orders.

## Changing rules
Never change rules mid-day or because of one trade. Review weekly (`04`). Change one thing at a time, recorded in `99_rationale.md` → Change history. If the running average R is below 0 after 30+ trades, say so at the top of the daily log and recommend that the user pause the schedule. Sessions never pause themselves.
