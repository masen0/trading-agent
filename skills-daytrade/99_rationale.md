---
file: 99_rationale.md
purpose: Why the rules are what they are, the research behind them, change history, and ideas for later. NOT read during trading sessions or guard runs; read it only when reviewing or changing the rules.
---

# Rationale, Research & History

## Honest expectations
- Most day traders lose money. In Brazil, 97% of people who day traded for 300+ days lost money (Chague, De-Losso & Giovannetti, 2020). In Taiwan, under 1% earned predictable profits after fees (Barber, Lee, Liu & Odean). The usual ways to lose: over-trading, letting losers run, breaking rules after a loss.
- Expected shape of results: a win rate around 30–45%, many small losses near 1R (a bit more when a stop is detected late), and occasional large winners. Many days have zero trades.
- We are weaker than the research: long-only (cash account), ~520 stocks instead of 7,000, and minutes of latency.

## Why this setup (`02`)
- **Opening Range Breakout on Stocks in Play** (Zarattini, Barbon & Aziz, 2024; 7,000+ US stocks, 2016–2023): breakouts of the opening range, traded only in stocks with abnormal opening volume, gave a Sharpe ratio of about 2.8 after costs. The same rule without the volume filter was weak (Sharpe about 0.5). Replications (e.g. QuantConnect) found the effect robust across parameters, though weaker. **The volume filter is the strategy. Never relax it to get more trades.**
- **Intraday momentum** (Gao, Han, Li & Zhou, *Journal of Financial Economics*, 2018): the market's first-half-hour direction tends to persist to the close, more so on high-volume, volatile days. Hence a 30-minute range and the requirement that QQQ agree.
- **VWAP as a trend filter** (Zarattini & Aziz, 2023): above or below the day's VWAP is a simple read of who controls the day.
- A **30-minute** range (the paper used 5 minutes) suits runs that are minutes apart.
- Not a dip-buying or news-judging setup. The volume filter already captures that "something happened".

## Why the thresholds
- **60% opening volume** (was 40% until 2026-10-11): the first 30 minutes normally carry 15–28% of a day's volume (measured on NVDA, AMD, PLTR, and QQQ, 2026-09/10). 40% is about 2× normal; 60% about 3×. In the Feb–Jul 2026 replay, every threshold from 40% to 80% was profitable with the mid-range stop, and 50–80% had smaller drawdowns (1.4–4.8% vs ~9%). PLTR's 2026-10-08 news day hit 134%.
- **Stop at the opening-range midpoint** (was the range low until 2026-10-11): with the low, the stop sat ~3.4% away, so 93% of trades just drifted to the 15:45 flatten and gains rarely reached the +2R ratchet (replay: −1.2% over Feb 23–Oct 9). A closer stop makes R smaller, so normal moves become meaningful in R and size goes up within the caps. On unseen Feb–Jul data, every stop closer than the range low (midpoint, 1.5%, 2%) was profitable (+2.7% to +13.2%); the range low was ~0.
- **Green opening candle and long only:** a cash account cannot short. A red opening candle signals the short side.
- **Chase limit (OR_high + 0.5 × range):** beyond it, the risk to the stop grows and the remaining move shrinks.
- **Max 5% stop distance:** with logical stops, a late-detected stop-out on a wide range costs far more than planned.
- **Min 0.75% stop distance:** tighter stops get hit by normal noise.
- **Ratchet starts at +2R:** moving to breakeven too early turns eventual winners into scratches. The research let trades run to the close.
- **Daily limit (2 losses or −4%):** two stop-outs usually mean the day does not suit breakouts. Losing traders make their worst decisions chasing a recovery.
- **50% position cap:** leaves room for a second trade. Cash used once is gone for the day (T+1 settlement).
- **1% sizing buffer:** stops are checked every 5–10 minutes, so stop-outs fill below the stop. Check it against measured slippage at each weekly review.
- **Kill switch $300:** about 30% below the $430 start. Capital is for experiments, but an automated system should never quietly lose everything.
- **Sell even if the price recovered above a touched stop:** a broker stop would already have sold. "It came back, so hold" is how small losses become large ones.
- **Check a bar's low before its high in the stop replay:** within one bar the order is unknown, so the conservative order is used. A stop can never be hit at a level it had not yet been raised to.

## Why logical stops and fractional shares
Robinhood allows fractional shares only on market orders, so broker-side stop orders need whole shares. At ~$430, whole shares made most stocks untradable (one share of a $373 stock with a 3.7% stop already risked more than the 2% budget). Logical stops plus the guard trade a little stop slippage for access to every stock.

## Why S&P 500 ∪ Nasdaq-100
The research ranked the whole market; replications used the ~1,000 most liquid stocks. The original 49 AI names produced only 2 trades in a two-week replay. The full Nasdaq Composite (~3,300 names) was excluded: many illiquid names, and the scanner has no list code for it. Screening runs server-side, so the larger universe barely changes token cost.

## Why this schedule
Token use is driven by the number of runs (each run pays a fixed startup cost), not by universe size. Entries happen only 10:00–11:30, so full sessions cluster there. Afternoon stop checks are left to the guard, every 10 minutes.

## Change history
- 2026-10-10: v1 created (49 AI names, whole shares, broker-side stops).
- 2026-10-11: fractional shares with logical stops, plus a 5-minute guard. Reason: whole shares blocked nearly every trade.
- 2026-10-11: universe widened to S&P 500 ∪ Nasdaq-100 (10-day ADV ≥ 1M). Reason: only 2 trades in two weeks on the 49 names.
- 2026-10-11: stop moved from OR_low to OR_mid; in-play threshold raised from 40% to 60%. Reason: chosen on Jul–Oct replay data and confirmed on unseen Feb 13–Jul 8 data (+8.3% vs −0.7% for the old rules; QQQ +16.8%). Not adopted: chase 0.25 and a +3R target (small effects; kept the rules simple).
- 2026-10-11: second consistency pass. Overnight positions are sold by any full session that finds them (stateless). The 0.75% minimum stop is defined once, from the fill. The daily log lists positions still open after the flatten. Dropped the "send yesterday's missing log" rule.
- 2026-10-11: consistency pass. On half days the guard's backup sale starts at 12:50, so it can't collide with the 12:45 sale session. The kill switch and the negative-expectancy warning go in the daily log, and the user pauses the schedule (sessions are stateless and must not pause themselves). The weekly review is appended to Friday's log. Stop distance uses the ask throughout. A rejected sell of already-sold shares is not an error.
- 2026-10-11: no more "legacy holdings". Every position held from a previous day, including the six fractional trend-strategy positions, is sold at the first full session. Reason: the user wants the account fully dedicated to day trading. The trend routine must not run on this account anymore.
- 2026-10-11: logging cut to one daily email at the 15:45 flatten, plus immediate alerts for the kill switch and stuck positions. Reason: logs never guide trading (the broker is the memory), and everything in the per-trade emails can be rebuilt from order history at 15:45.
- 2026-10-11: token reduction. Guard every 10 minutes (32 runs/day), afternoon full sessions removed (8/day), and rationale moved to this file.
- 2026-10-12: LEFTOVER lines report realized P&L from Robinhood's realized-P&L history, because average cost is no longer visible at 15:45 (the first live log showed "vs avg cost n/a").

## Replay results (for reference)
- 2026-09-24 to 10-09, 49 AI names, fractional rules: 30 in-play stock-days, 19 with green candles, 7 broke out by 11:30, 2 trades (NVDA 10/02 −1.1R, WDC 10/05 −0.47R), −$6.56. Twelve of the 19 green in-play names never broke their opening-range high. Too small a sample to judge the edge.

- Feb 23–Oct 9 2026, S&P 500 ∪ Nasdaq-100, OLD rules (OR_low stop, 40%): 107 trades, 49% wins, avg −0.03R, −1.2%, max drawdown 9.0%; QQQ +23.4% (drawdown 11.3%). 99 of 107 exits were the 15:45 flatten.
- Same period, NEW rules (OR_mid stop, 60%; with chase 0.25 and +3R target as tested): unseen Feb–Jul +8.3% (50 trades, drawdown 3.5%); full period +14.4% (69 trades). At 4% risk / 100% cap: unseen +16.7% vs QQQ +16.8%. Caveats: one unseen period, today's index members, 5-minute-bar prices, 0.05% cost per side assumed.
- Robinhood intraday history only reaches back to 2026-01-30 (earlier bars are zero-volume placeholders).

## Parking lot (ideas deliberately left out)
- Failed-breakout early exit (price back below both VWAP and OR_high → exit before the stop).
- Letting the guard also take entries 10:00–11:30, to catch breakouts between full sessions (the replay added 2 trades: +$0.99, −$2.18).
- A midday VWAP-pullback setup for afternoon trades.
- Skipping entries ahead of scheduled 14:00 FOMC announcements.
- SPY instead of, or in addition to, QQQ for the market filter.
- Russell 1000 universe.
- Running the guard on a smaller model.
