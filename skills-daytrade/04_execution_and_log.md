---
file: 04_execution_and_log.md
purpose: Orders, broker reconciliation, logging, weekly review
---

# Execution & Logging

Account **`494502131`** only. **Every order: `type: market`, `market_hours: regular_hours`, `time_in_force: gfd`.** Fresh UUID `ref_id` per order. Re-use it only to retry a transport failure of the same order.

## Step 1: reconcile (start of every full session)
1. `get_portfolio`: account value, buying power.
2. `get_equity_positions`: day part = `intraday_quantity`; overnight part = the rest (sold by `03` Exit D).
3. `get_equity_orders`, `created_at_gte` = today: fills, and any open orders. Market orders should leave none: cancel any open order.

## Buy
1. `side: buy`, `dollar_amount` = size with 2 decimals (e.g. `"184.15"`).
2. Confirm `filled` (up to 3 checks over ~1 minute). If rejected or unfilled, cancel the remainder. The daily log reports it from the order history. Never retry at a higher price.
3. Record the fill price (= entry), quantity, and time. R = entry − initial stop.
4. No stop order. The stop replay protects the position.

## Sell
`side: sell`, `quantity` = exactly `intraday_quantity` for a day position, or exactly the overnight part for an Exit D sale (up to 6 decimals). Confirm the fill and sell any remainder.

## Logging
Logs are for review only; no run reads them to make a trading decision. **One daily log per trading day, written by the 15:45 flatten session after the flatten** (always, even with no trades). Other runs send nothing, except the **STUCK POSITION** alert: a sell that failed twice. Name the symbol, quantity, and error. It repeats on each failing run until resolved, which is intended.

- **Daily log → Gmail draft, never sent:** create a draft (Gmail `create_draft`) addressed to the user's own address, subject `DayTrade-Log [date]`, then apply the label `DayTrade-Log`. Create the label once if it doesn't exist.
- **STUCK POSITION alert → real email to the inbox:** send it (Gmail `send_message`) to the user's own address, subject `DayTrade ALERT: STUCK POSITION [TICKER] [HH:MM]`, label `DayTrade-Log`. This is the only email that is ever sent.

The daily log is rebuilt from the broker at 15:45. Nothing needs to be carried during the day.
- Trades: today's filled buys and sells (`get_equity_orders`).
- Opening range: the first 30-min bar of each traded symbol (`01` step 2).
- Problems: any rejected or cancelled orders today.

```
DAY [date] | account $[x] | BP $[x]
Trades [n] | W [n] L [n] | P&L $[x] ([x.x]%) | total R [x.x]
[TICKER] buy $[size] @ $[entry] [HH:MM] | OR $[low]–$[high], initial stop $[x] ([x.x]%), R $[x]
   sell @ $[fill] [HH:MM] | [stop | flatten] | P&L $[x] = [+/-x.x]R | slippage vs stop [x.xx]%
LEFTOVER [TICKER] [qty] sh sold @ $[fill] [HH:MM] | vs avg cost $[x]   (positions from a previous day; not counted as trades)
Problems: [none | rejected/cancelled orders, data issues]
Running since start: trades [n] | win [x]% | avg R [x.xx] | avg stop slippage [x.xx]% | P&L $[x]
```
- **Exit type** of a day trade: sold before the flatten session = **stop**; sold by the flatten session = **flatten**. Any day position still open after the flatten goes under Problems (the guard's backup retries at 15:55).
- **Slippage vs stop** = (stop − fill) / stop. Report it for stop exits whose stop never moved up (the trade never reached +2R). Otherwise write `n/a`.
- **Running totals** carry forward from the previous daily log. Recompute them from the trade list if they look inconsistent.

## Weekly review (Friday's daily log)
The Friday flatten session reads this week's daily logs (drafts labelled `DayTrade-Log`) and appends a review: expectancy (average R), win rate, average win and loss in R, average and worst stop slippage (if the average is > 1%, the sizing buffer is too small), and any problems. Propose at most **one** change with reasons. Never apply it yourself.
