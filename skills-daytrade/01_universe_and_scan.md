---
file: 01_universe_and_scan.md
purpose: Universe and the in-play scan
---

# Universe & In-Play Scan

**Universe:** S&P 500 ∪ Nasdaq-100 with 10-day average daily volume ≥ 1M shares (~315 stocks). Share price does not matter.

Screening runs **on Robinhood's servers**. Never pull quotes or bars for the whole universe.

**In play** = the first 30-minute candle (9:30–10:00 ET) traded **≥ 60% of the 10-day average daily volume.** Never lower this threshold.

## Step 1: scan
Run the saved scan **`DT - In Play`** (`get_scans` → `run_scan`). If it does not exist, create it once with `create_scan` (validate with `preview_scan` first), with these filters and no extra columns:

1. `expression: symbol`, `PREDICATE_IN_LIST`, `["SP500", "NDX"]`
2. `expression: volumeAvg10Day(session="regular")`, `PREDICATE_GREATER_THAN_OR_EQUAL`, `["1000000"]`, `display_title: Avg vol 10d`
3. `expression: dayVolume(session="regular") / volumeAvg10Day(session="regular")`, `PREDICATE_GREATER_THAN_OR_EQUAL`, `["0.6"]`, `display_title: Vol vs 10d avg`
4. `expression: price(session="regular") > dayOpenPrice(session="regular")`, `PREDICATE_EQUAL`, `["True"]`, `display_title: Above open`
5. `expression: price(session="regular") > dayVwap(session="regular")`, `PREDICATE_EQUAL`, `["True"]`, `display_title: Above VWAP`

Every filter is a necessary condition for a trade, so none can wrongly drop a setup. Volume is cumulative, so step 2 is still required.

**Take at most the top 20 rows by `Vol vs 10d avg`.**

## Step 2: exact check
For those rows, fetch **only the first 30-minute bar**: `get_equity_historicals`, `interval: 30minute`, `start_time` = today 9:30 ET, **`end_time` = today 10:00 ET** (in UTC), 10 symbols per call. From that bar: `opening_volume`, `OR_high`, `OR_low`, `OR_open`, `OR_close`.

**In play** if `opening_volume ≥ 0.60 × Avg vol 10d`. Record the ratio (opening RVOL). If the bar is missing, flagged `interpolated`, or does not begin at 9:30 ET, the name is not tradable today.
