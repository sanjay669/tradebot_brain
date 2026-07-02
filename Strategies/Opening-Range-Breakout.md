---
tags: [strategy, agent]
status: invalidated
instrument: equity_cash
---
# Opening-Range Breakout (example agent)

> 🛑 **INVALIDATED (2026-07-02).** A 24-month walk-forward across 5 liquid stocks shows
> **no net-of-cost edge** — negative in every fold (see [[2026-07-02-walkforward]]).
> **Do not trade this with real money.** Kept as a reference / sandbox example only.
> Implemented in `agents/equity_intraday_agent.py`.

## Idea
The **opening range** is the high/low of the first 15 minutes after the open
(09:15–09:30 IST). A move that breaks decisively above the range high is taken as a
long breakout signal.

## Entry rule
- Go **long** when `ltp > opening_range_high × (1 + breakout_pct)`.
- `breakout_pct` default **0.2%** (the buffer that filters marginal breaks).
- Reported `confidence = 0.6` (must be ≥ `min_signal_confidence`, see [[Trading-Rules]]).

## Exit rule (current code)
- **Stop:** `opening_range_low`.
- **Take-profit:** `entry + reward_risk_ratio × (entry − stop)` (R-multiple, default **2.5R**).
- **EOD square-off** at `square_off_time` (default 15:20 IST).
- Exits are **bot-managed** (LTP watched each tick, MARKET exit) — see orchestrator note
  in [[Components]]. For live, migrate to broker GTT/OCO. [[Risk-Rules]]

## Data dependencies
- `MarketDataFeed` builds the opening range locally from 09:15–09:30 LTP samples →
  **start the bot near the open** or the range is incomplete. See [[Upstox-API]].

## Parameters to tune (evidence-driven)
| Param | Where | Default | Notes |
|-------|-------|---------|-------|
| `breakout_pct` | agent ctor | 0.2% | Higher = fewer, cleaner breaks |
| `confidence` | agent | 0.6 | Static today; make it regime-aware |
| stop | agent | OR low | Consider ATR-based instead |

## Validation status — FAILED
- [x] Backtested over 24 months / 5 stocks with **walk-forward** validation.
- **Result: net-negative every fold** (~−0.14%/trade after ~0.15% costs; gross win ~50%
  but winners don't clear costs). Gross edge exists (+0.04%/trade) but is **smaller than
  transaction costs.** See [[2026-07-02-walkforward]], [[2026-07-02-param-sweep]].
- [ ] Paper-traded in sandbox — moot until a config with a positive OOS net edge exists.
- **Verdict: no durable edge. Not live-eligible.**

## Open questions
- Does the 0.2% buffer over-filter on calm days?
- Long-only leaves half the breakouts on the table — add short side?
- Is a fixed OR-low stop too wide on volatile names?

## Related
[[Strategy-Template]] · [[Trading-Rules]] · [[Trade-Postmortem-Template]]
