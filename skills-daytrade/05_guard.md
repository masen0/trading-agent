---
file: 05_guard.md
purpose: Guard routine — sell day positions whose stop was hit, plus the backup flatten. Nothing else.
---

# Guard Routine

**Runs:** every 10 minutes at minutes ending in 5, 10:05–15:55 ET, except 10:15, 10:45, 11:15, and 15:45. Account **`494502131`**. **Never buys, scans, or changes rules. Keep it fast.**

1. `get_equity_positions`. Keep only `intraday_quantity > 0`. **None → end now. No log, no email.**
2. **Backup flatten:** at 15:50 ET or later (12:50 or later on early-close days), sell every day position at market (`04` Sell), then go to step 5.
3. For each day position, run **`03_risk_and_exits.md` → Exit A** exactly (read that section). Inputs: today's filled buy (`get_equity_orders`, `created_at_gte` = today, `symbol`); `OR_high` and `OR_low` from the first 30-min bar (`OR_mid` = their average) (`start_time` 9:30 ET, `end_time` 10:00 ET); 5-minute bars after the fill; the last price (`get_equity_quotes`).
4. **Stop hit → sell at market now** (`04` Sell) and confirm the fill.
5. **No email.** The daily log at 15:45 reports the guard's sales from the broker's order history.

**Errors:**
- Data missing (no bars, no fill record, no OR bar): sell if the price is ≤ `OR_mid` (or below entry if the opening range is unknown). Otherwise hold. The next run retries.
- Sell rejected because the shares are already gone (another run sold them): not an error. Stop.
- Any other rejection: retry once with a fresh `ref_id`. If it fails again, send the **STUCK POSITION** alert (`04`). The next run retries.
