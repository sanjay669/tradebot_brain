---
tags: [rules, checklist, gate]
---
# Going-Live Checklist

**Do not flip the live latches until every box is checked.** Live trading needs BOTH
`config.yaml` → `mode.sandbox: false` AND `mode.live_confirmed: true`. This is
deliberate friction so one accidental edit can't arm real orders.

## Infrastructure
- [ ] `upstox-python-sdk==2.28.0` installed (pinned; see [[Upstox-API]]).
- [ ] `.env` filled with real `UPSTOX_API_KEY` / `_SECRET` / `_REDIRECT_URI`.
- [ ] `.env` is gitignored and never committed. (Verify: `git status` shows no `.env`.)
- [ ] Upstox app is the **Algo Trading App** type; **static IP whitelisted**.
- [ ] `python refresh_token.py` produces a working access token — see [[Token-Refresh]].

## Correctness
- [ ] Ran `python -m pytest tests/` — risk manager tests pass.
- [ ] Ran in **sandbox** long enough that `trade_journal.csv` shows a trusted pattern,
      not a few lucky days.
- [ ] Backtested the strategy (`backtest.py`) — see [[Opening-Range-Breakout]].
- [ ] Confirmed the **stop-loss leg** places correctly in sandbox (check the SL order
      appears right after each entry). This is [[Risk-Rules]] rule #1.
- [ ] Confirmed LTP re-keying works (no `No LTP for ...` warnings) — see [[Upstox-API]].

## Risk config sanity ([[Config-Reference]])
- [ ] `total_capital_inr` = your ACTUAL deployable capital.
- [ ] `daily_loss_cap_inr` / `_pct` set to a loss you can accept.
- [ ] `approval_threshold_inr` set LOW at first (Phase 2) so you approve most trades.
- [ ] `max_leverage` within current exchange/segment limits.

## Compliance ([[SEBI-Algo-Trading-Rules]])
- [ ] Order rate well under 10/sec (default caps guarantee this).
- [ ] Strategy logic is white-box / explainable.
- [ ] Usage is for your own account only.

## The flip
- [ ] Set `mode.sandbox: false`
- [ ] Set `mode.live_confirmed: true`
- [ ] Start with the SMALLEST viable size for the first live week.

## Related
[[Risk-Rules]] · [[Trading-Rules]] · [[Morning-Runbook]] · [[System-Overview#Roadmap]]
