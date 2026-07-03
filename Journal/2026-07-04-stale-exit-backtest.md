---
date: 2026-07-04
type: research
status: recorded
---

# Stale-exit rule backtest — 5 stocks, Apr–Jul 2026 (in-sample)

Backtested the new `strategy.stale_exit` rule (see [[Learning-Loop]]) on 1-min
candles, 2026-04-01 → 2026-07-03, ORB agent with config parity (reward_risk 2.5).
`backtest.py` now models the same exit ladder as the live orchestrator
(stop → target → stale → day-close) and prints a per-exit-reason breakdown.

## Results (gross P&L per share, before ~0.15% round-trip costs)

| Stock    | Baseline | Stale @150min | Change |
|----------|---------:|--------------:|--------|
| NTPC     |    +4.65 |        +18.40 | better |
| INFY     |   −16.30 |        +17.40 | better |
| TCS      |   −24.20 |         +1.50 | better |
| SBIN     |    +0.95 |        +34.25 | better |
| RELIANCE |   +89.90 |        +24.30 | **worse** |
| **Total**|  +55.00  |       +95.85  | +74%   |

Sensitivity on NTPC: 90min → +13.65, 150min → +18.40, 210min → +14.45.
150 min held up best of the three tested.

## Reading it honestly

1. **The rule does what the daily reviews predicted**: on 4 of 5 stocks the
   losses were dead trades drifting into the close; cutting them at 150 min
   flipped INFY and TCS from negative to positive and tripled SBIN.
2. **The cost is real**: RELIANCE was the one stock that trended after entry,
   and the time stop cut those slow winners (+89.90 → +24.30). Time stops trade
   tail capture for bleed reduction — that is the deal, not a free lunch.
3. **Still not a validated edge.** In per-trade percent terms only NTPC
   (~0.21%/trade gross) clears the ~0.15% round-trip cost hurdle in this window;
   SBIN is near breakeven, the rest below. This is ONE in-sample window with no
   walk-forward — the same trap [[2026-07-02-walkforward]] warned about.
   Conclusion unchanged: ORB remains invalidated for real money; the stale exit
   makes the *sandbox* bot bleed less and hold risk for less time, which is why
   it stays enabled.
4. Also notable: at reward_risk 2.5 the take-profit **never fired once** in
   3 months on any of the 5 stocks — the 2.5R target is effectively out of reach
   for this signal; exits are decided entirely by stop/stale/close.

## Follow-up: 1.5R target (same 5 stocks, same window)

Since 2.5R never fired, retested with `--rr 1.5` (new backtest flag; config untouched):

| Stock    | 1.5R baseline | 1.5R stale@150 | (2.5R stale@150) |
|----------|--------------:|---------------:|-----------------:|
| NTPC     |         +4.65 |         +18.40 |           +18.40 |
| INFY     |         −1.85 |         +17.40 |           +17.40 |
| TCS      |        −19.05 |          +1.50 |            +1.50 |
| SBIN     |        +17.37 |         +39.40 |           +34.25 |
| RELIANCE |        +83.20 |         +23.15 |           +24.30 |
| **Total**|       +84.32  |        +99.85  |          +95.85  |

Findings:
1. At 1.5R the take-profit fires only 1–3 times per stock in 3 months (0 on NTPC)
   — even the nearer target is rarely reached before stop/stale/close.
2. **With the stale rule active, the target level barely matters**: 18–21 of ~25
   trades per stock exit stale; 1.5R vs 2.5R differs by ~4% total (99.85 vs
   95.85), well within one-window noise.
3. DECISION: keep `reward_risk_ratio: 2.5` — changing config on a ~4% in-sample
   difference is exactly the overfitting trap flagged in [[2026-07-02-walkforward]].
   The exit engine for this signal is stop + stale + close; the R-target is a
   minor tail-capture lever, not the edge.

Related: [[Learning-Loop]], [[Opening-Range-Breakout]], [[2026-07-03-richer-edge-results]]
