---
file: 01_universe.md
purpose: What to trade for mean-reversion — liquid, volatile, range-prone names plus tactical inverse ETFs
---

# Universe — Mean-Reversion Watchlist

**KEY CONCEPT — mean-reversion wants *liquid, volatile, oscillating* names, not the "best companies."** The edge comes from a stock overshooting and snapping back, so what matters is: does it move enough (volatility) and does it revert (range-prone), and can you get in/out cleanly (liquidity)? Fundamental quality is only a secondary filter to avoid buying dips that never bounce.

The candidate list is the same AI-beneficiary universe as the trend-following book (so the two strategies are compared on the same names), but ranked here by **reversion suitability**, not by thesis.

---

## Tier 1 — Holdings

Every symbol currently owned (`get_equity_positions`, quantity > 0). Managed first every session — check target / stop / time-stop before anything else.

## Tier 2 — Watchlist (screened equally, sector-neutral)

The full AI universe (see the trend-following `skills/01_universe.md` for the sector tables — Memory/Chips/Equipment/Mega-cap/Software/Cloud/Hardware/Power/Networking/Space). For mean-reversion, prioritize within it by:

- **High average true range (ATR%)** — needs enough amplitude to produce a tradable bounce (ATR ≥ ~3% of price is ideal)
- **Clear historical range behavior** — the stock bounces off identifiable support repeatedly rather than trending in a straight line
- **High liquidity** — tight spreads, large-cap or heavily-traded, so entries/exits don't slip

Names like NVDA, AMD, MU, SKHY, ASML, SMCI, PLTR, NET, DDOG tend to be high-ATR and reversion-friendly. Low-volatility slow movers are poor mean-reversion vehicles.

---

## Tactical Inverse ETFs (optional, short-term only)

Available as tools when a *sector* is stretched, not just a single stock:

| Ticker | Exposure | Use |
|---|---|---|
| SOXS | −3× semis (SMH/SOXX) | When semis are extremely *overbought* and rolling over, or to hedge a semi-heavy book in a confirmed sector downdraft |
| SQQQ | −3× Nasdaq-100 | Broad tech overbought / risk-off hedge |
| SPXU | −3× S&P 500 | Broad-market hedge |

**Hard rules for inverse ETFs (see `06`):** these are −3× *daily* instruments that decay in chop. Treat them as **short-duration tactical positions (days, not weeks)**, size them small (they move 3×), and always attach a hard stop and a tight time-stop. They are a hedge/short-proxy tool, never a hold.

---

## Screening Method (sector-neutral)

Two tiers only — Holdings (managed first) and Watchlist (screened equally). Each session:
1. Quick-screen the whole watchlist for **oversold + at support** candidates (RSI, distance below 20-SMA / lower Bollinger).
2. Advance a name to deep analysis only if it is oversold AND still above its 200-day SMA (uptrend intact) AND has no thesis-breaking news.
3. Rank qualifying setups by reward:risk (`07`), not by sector.
