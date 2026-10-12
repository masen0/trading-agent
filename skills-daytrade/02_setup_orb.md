---
file: 02_setup_orb.md
purpose: The one entry setup — 30-minute Opening Range Breakout on in-play stocks, long only
---

# Setup: ORB-30, Long Only

The **only** way this system enters. No match on every condition, no trade.

## Entry conditions (ALL true at the session)
1. Time is **10:00–11:30 ET**.
2. **QQQ above its VWAP** today.
3. **QQQ not down more than 1.0%** from its previous close.
4. **In play** (`01`).
5. **Green opening candle:** `OR_close > OR_open`.
6. **Breakout:** current price **> `OR_high`**.
7. **No chasing:** current price **≤ `OR_high` + 0.5 × (`OR_high` − `OR_low`)**. Otherwise the trade is missed.
8. Current price **above the stock's VWAP** today.
9. **First entry in this symbol today.** No re-entry, win or lose.
10. Size from `03` is **≥ $10** and the stop distance passes the check below.
11. Daily limits and kill switch (`03`) not hit.
12. Fewer than **2** day positions open.

Skip any name with a trading halt or a pending merger or delisting.

## Initial stop
- **Initial stop = `OR_mid` = (`OR_high` + `OR_low`) / 2**, the middle of the opening range.
- Stop distance = (ask − stop) / ask:
  - **> 5% → skip.**
  - **< 0.75% → size with 0.75%.** The exact initial stop and R are set from the fill (`03` Exit A).

## Ranking
Higher opening RVOL first; tie-break on the smaller stop distance. Take names in order until 2 positions are open or buying power runs out.

## Data (one pass, ≤ 10 symbols per call)
- VWAP: `get_equity_technical_indicators`, `type: vwap`, `interval: 5minute`, `output: latest`, `start_time` = today 9:30 ET. QQQ plus candidates.
- Price and QQQ change: `get_equity_quotes`. Change = (last − `adjusted_previous_close`) / `adjusted_previous_close`.
- Opening range: `01` step 2.
