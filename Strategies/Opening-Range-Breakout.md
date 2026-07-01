---
tags: [strategy, agent]
status: unvalidated
instrument: equity_cash
---
# Opening-Range Breakout (example agent)

> ⚠️ **Unvalidated starter example.** Included to show the agent interface, not as a
> strategy to trust. Backtest and paper-trade extensively before real capital.
> Implemented in `agents/equity_intraday_agent.py`.

## Idea
The **opening range** is the high/low of the first 15 minutes after the open
(09:15–09:30 IST). A move that breaks decisively above the range high is taken as a
long breakout signal.

## Entry rule
- Go **long** when `ltp > opening_range_high × (1 + breakout_pct)`.
- `breakout_pct` default **0.2%** (the buffer that filters marginal breaks).
- Reported `confidence = 0.6` (must be ≥ `min_signal_confidence`, see [[Trading-Rules]]).

## Exit rule
- **Stop:** `opening_range_low` (placed as an SL-M immediately after entry — [[Risk-Rules]]).
- No profit target yet; end-of-day square-off is a [[Trading-Rules|known gap]].

## Data dependencies
- `MarketDataFeed` builds the opening range locally from 09:15–09:30 LTP samples →
  **start the bot near the open** or the range is incomplete. See [[Upstox-API]].

## Parameters to tune (evidence-driven)
| Param | Where | Default | Notes |
|-------|-------|---------|-------|
| `breakout_pct` | agent ctor | 0.2% | Higher = fewer, cleaner breaks |
| `confidence` | agent | 0.6 | Static today; make it regime-aware |
| stop | agent | OR low | Consider ATR-based instead |

## Validation status
- [ ] Backtested (`backtest.py`) over ≥ 3 months — record results in [[Journal/README|journal]].
- [ ] Paper-traded in sandbox for weeks.
- [ ] Win rate / avg-win-vs-loss reviewed and trusted.

## Open questions
- Does the 0.2% buffer over-filter on calm days?
- Long-only leaves half the breakouts on the table — add short side?
- Is a fixed OR-low stop too wide on volatile names?

## Related
[[Strategy-Template]] · [[Trading-Rules]] · [[Trade-Postmortem-Template]]
