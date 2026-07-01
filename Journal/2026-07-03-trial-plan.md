---
tags: [journal, plan, trial]
date: 2026-07-03
---
# 2026-07-03 — Trial Day Plan (Rs.1000)

**Goal (user):** run a Rs.1000 intraday trial, "many small trades / small margins,"
target Rs.200–500 profit, bot to trade autonomously.

## Config set (2026-07-02) — see [[Config-Reference]]
- `total_capital_inr: 1000`, `daily_loss_cap_inr/pct: 200 / 20%` → halt at **−Rs.200**
- `approval_threshold_inr: 1000` (auto-execute within budget), `max_leverage: 1.0`
- `max_trades_per_day_total: 5`
- Mode still **sandbox: true** (NOT live) — deliberate; go-live not confirmed.

## Open blockers / honest constraints (must resolve before this is real)
1. **Transaction costs dominate on Rs.1000.** Upstox intraday brokerage ≈ Rs.20/order
   (or 0.05%), so a buy+sell round trip ≈ **Rs.40+ before STT/GST ≈ ~4%+ of Rs.1000**.
   "Maximum trades, small margins" therefore loses to costs by arithmetic — the more
   small trades, the more certain the loss. Few high-conviction trades only, or treat
   as a paper exercise. This is the decisive point.
2. **Instrument price.** Agent trades ONE hardcoded stock (RELIANCE, >Rs.1000/share) at
   fixed qty 1 → exceeds the Rs.1000 budget. Need a sub-Rs.1000 liquid instrument set.
3. **No take-profit / no square-off.** ORB agent enters + attaches SL-M stop only; it
   cannot *target* Rs.200–500 and doesn't flatten at 15:30. Needs a take-profit leg.
4. **Strategy is unvalidated** (labelled "STARTER EXAMPLE ONLY" in code). Real money on
   it is high-risk. [[Opening-Range-Breakout]] · [[Going-Live-Checklist]].
5. **Autonomy limit.** Claude is not a live intra-day process; the *bot* runs
   autonomously via `docker compose up`. Claude cannot watch ticks and discretionarily
   trade through the day, nor guarantee profit.

## Decisions pending (user)
- Sandbox (recommended first) vs go live (flip both latches — explicit confirm needed).
- Which sub-Rs.1000 instrument.
- Add take-profit exit? Add intraday square-off at 15:30?

## Related
[[Morning-Runbook]] · [[Token-Refresh]] · [[Risk-Rules]] · [[Daily-Review-Template]]
