---
tags: [journal, backtest, results]
date: 2026-07-02
instrument: NTPC (NSE_EQ|INE733E01010)
---
# 2026-07-02 — NTPC Backtest Results (ORB agent)

First recorded results for the [[Opening-Range-Breakout]] agent, run because we missed
the 09:30 IST opening-range window for a live sandbox session (token had expired; by the
time it was refreshed it was 09:47). Backtested instead — see [[2026-07-03-trial-plan]].

**Setup:** NTPC, June 2026, 1-minute candles (8,250 candles) via Upstox History API.
No fees/slippage modelled (results are optimistic).

## Results
| Exit model | Days traded | Win rate | Total P&L/share | Avg win | Avg loss |
|---|---|---|---|---|---|
| Stop or day-close (old) | 8 | 50% | **−3.15** | +1.55 | −2.34 |
| **+0.5% take-profit (live)** | 8 | 50% | **−3.32** | +1.51 | −2.34 |

## Diagnosis — negative expectancy, bad reward:risk
- The +0.5% take-profit **caps winners at ~₹1.51** while the stop (opening-range **low**)
  lets **losers run to ~₹2.34 ≈ 0.65%**. Reward:risk ≈ **1.5:1 against**.
- At a 50% hit rate that must lose money. The tight fixed take-profit made it *worse*,
  not better — classic "cut winners, let losers run to stop."

## Proposed changes (for user approval — do NOT auto-apply)
1. **Make the target a multiple of the stop distance** (e.g. 1.5–2R) instead of a fixed
   0.5%, so winners can exceed the risk taken. Edit `take_profit_pct` → an R-multiple in
   the agent. [[Risk-Rules]]
2. Consider a **tighter/structural stop** (e.g. below the breakout candle, not the whole
   opening range) to improve R:R.
3. Add filters (time-of-day, min range width, VIX) — [[Trading-Rules]].

## Caveats (important)
- **8 trades is not a statistically meaningful sample** — one month, one stock. Do NOT
  draw strong conclusions; this is a first read, not a verdict.
- Extend to several months and multiple instruments before trusting any number.
- No fees/slippage modelled.

**Conclusion:** do not go live with the current parameters. Next iteration = fix the
reward:risk (change 1), re-backtest over a longer window, then reassess.

## Related
[[Opening-Range-Breakout]] · [[Journal/README]] · [[2026-07-03-trial-plan]]
