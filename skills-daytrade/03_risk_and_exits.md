---
file: 03_risk_and_exits.md
purpose: Sizing, the stop replay, exits, daily limits, kill switch
---

# Risk & Exits

Risk rules override everything.

*Account value* = `total_value` (`get_portfolio`). *Buying power* = `buying_power.buying_power` (settled cash). *Day position* = `intraday_quantity > 0`.

## Sizing (dollars)
```
stop_pct = (ask − initial_stop) / ask
size     = (2% × account value) ÷ (stop_pct + 1%)
size     = min(size, 50% × account value, buying power)    # round down to the cent
```
Skip if size < $10. Never count on earlier sale proceeds (T+1).
Example: account $430, ask $373.12, OR $359.41–$371.33 → OR_mid $365.37 → stop_pct 2.08% → 8.60 ÷ 0.0308 = $279.50 → capped at 50% = **$215.00**.

## Exit A: stop replay (at every full session and every guard run)
No stop order exists at the broker. Rebuild the stop from scratch each time. Never read it from a log.

**Inputs:**
- `entry`, `fill_time`: today's filled buy for the symbol.
- `initial_stop` = `OR_mid` = (`OR_high` + `OR_low`) / 2. If that is less than 0.75% below `entry`, use `initial_stop` = entry × (1 − 0.0075).
- `R` = entry − initial_stop.
- `bars` = today's **5-minute** regular-session bars **starting with the first bar that begins after `fill_time`**, up to now (`get_equity_historicals`, `interval: 5minute`, `start_time` = the fill time). Skip `interpolated` bars.

```
stop = initial_stop; best_R = 0
for bar in bars (time order):
    if bar.low <= stop: STOP HIT → stop here
    best_R = max(best_R, floor((bar.high − entry) / R))
    if best_R >= 2: stop = max(stop, entry + (best_R − 1) × R)
```
Then: if the current last price (`get_equity_quotes`) is ≤ `stop`, that is also a hit.

**Hit → sell the full day quantity at market now, even if the price has recovered above the stop.**

## Exit B: ratchet
Built into the replay. Below +2R the stop stays at `initial_stop`. After reaching N whole R (N ≥ 2), the stop is entry + (N − 1) × R. It only rises.

## Exit C: flatten
At the flatten session, sell every day position at market. No exceptions. The guard repeats this from 15:50 as a backup.

## Exit D: overnight positions
At **any full session** that finds one, before any new entry, sell at market **every** position held from a previous day: the full `quantity − intraday_quantity` of each symbol, including any not bought by this system. Confirm each fill. These sales count as leftovers in the daily log, not as day trades, so they do not affect the daily limits. Their proceeds settle T+1 and are not buying power today.

No other exits: no discretionary selling, no early profit-taking.

## Daily limits
No new entries for the rest of the day after **2 losing round trips** or **day-trade P&L ≤ −4% of account value** (realized plus unrealized). Keep managing open positions.

## Kill switch
**Account value ≤ $300 → no new entries.** Manage open positions to their exits. The daily log opens with "KILL SWITCH" and the drawdown until the user resets the floor. Floor: **$300** (only the user changes this).
