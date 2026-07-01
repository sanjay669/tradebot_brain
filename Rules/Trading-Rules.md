---
tags: [rules, trading]
---
# Trading Rules

When the bot **may** and **may not** act. Enforced across `risk_manager.py`
(regime filter) and each agent's `generate_signal()`.

## The bot sits out when...
- **Confidence is low.** Agents self-report a `confidence` 0–1; below
  `regime_filter.min_signal_confidence` (default 0.6) no trade is proposed.
  *"If the market seems unpredictable, don't trade."*
- **Volatility is elevated.** If India VIX > `india_vix_block_above` (default 20.0),
  all trades are blocked. VIX ~13–14 is calm; >20 is choppy.
- **Limits are reached.** Any [[Risk-Rules]] cap (trade count, loss cap) blocks the trade.
- **Outside market hours.** `run_market_hours.py` only runs the loop 09:15–15:30 IST,
  weekdays. See [[Morning-Runbook]]. (Holiday calendar not yet wired — see TODO below.)

## When the bot trades small vs. asks
- **Auto-execute:** notional ≤ `approval_threshold_inr` → the order goes through.
- **Queue for approval:** notional > threshold → parked in `pending_approvals.json`,
  reviewed via `approve_trades.py`. This matches the stated preference: small trades
  auto, larger ones get a human. See [[Data-Flow]].

## Position discipline
- Intraday only (`product="I"`), `validity="DAY"`.
- Every entry has a stop ([[Risk-Rules]] rule 1).
- One trade proposal per agent per tick — a good agent passes far more than it trades.

## Known gaps to close before scaling
- [ ] NSE/BSE **holiday calendar** — the weekday check doesn't skip holidays yet.
- [ ] **Position sizing** currently `quantity=1` in the example agent; should move to
      the orchestrator and size off capital + stop distance.
- [ ] **End-of-day square-off** of any open intraday position before 15:30.

## Related
[[Risk-Rules]] · [[Opening-Range-Breakout]] · [[Config-Reference]]
