---
tags: [journal, backtest, walk-forward, validation, results]
date: 2026-07-02
---
# 2026-07-02 — 24-Month Walk-Forward Validation (ORB)

The definitive test, following [[2026-07-02-oos-validation]]. **24 months** (2024-07 →
2026-06), **5 liquid stocks** (NTPC, ITC, ONGC, COALINDIA, POWERGRID), 1-min candles.
Rolling **6-month train → 3-month test**, step 3 months (6 folds). On each TRAIN window
the best `(breakout, RR)` is picked by net-of-cost expectancy, then applied UNCHANGED to
the next unseen TEST window. Metric = net of ~0.15% round-trip cost.

## Per-fold out-of-sample results
| Fold | Train | Test | Chosen | Train net% | Test net%/trade | Test total% |
|---|---|---|---|---|---|---|
| 0 | 24-07..24-12 | 25-01..25-03 | bp0.3 RR3.0 | −0.209 | −0.084 | −12.1 |
| 1 | 24-10..25-03 | 25-04..25-06 | bp0.3 RR2.5 | −0.194 | −0.109 | −12.8 |
| 2 | 25-01..25-06 | 25-07..25-09 | bp0.3 RR2.5 | −0.077 | −0.224 | −21.3 |
| 3 | 25-04..25-09 | 25-10..25-12 | bp0.3 RR2.0 | −0.160 | −0.235 | −26.3 |
| 4 | 25-07..25-12 | 26-01..26-03 | bp0.1 RR2.5 | −0.227 | −0.090 | −13.1 |
| 5 | 25-10..26-03 | 26-04..26-06 | bp0.3 RR2.0 | −0.145 | −0.114 | −11.7 |

**Every train AND every test fold is net-negative.**

## Aggregate (all out-of-sample folds combined)
- **Adaptive (retuned each fold):** 717 trades, net **−0.136%/trade**, net total **−97%**.
- **Fixed (breakout 0.2, RR 2.5):** 805 trades, net **−0.152%/trade**, net total **−122%**.
- Gross win rate ~50%, but net-positive-trade rate only ~38% → winners don't clear the
  ~0.15% cost hurdle.

## Verdict — settled
**The opening-range-breakout strategy has NO net-of-cost edge.** It loses ~0.14%/trade
consistently across 2 years and every regime tested. Retuning per fold did not rescue it
(and full-period tuning would just curve-fit). The encouraging June-2026 numbers were
**noise / a favorable month**, exactly as the earlier OOS split warned.

## Consequence
- [[Opening-Range-Breakout]] marked **invalidated**; **not live-eligible**.
- Do NOT trade real money on ORB. Sandbox/reference only.
- To find a real edge: a **different signal source** (ORB is too weak on liquid large-caps),
  tested with this same walk-forward discipline; and/or lower frequency so each trade
  clears costs. The framework + validation harness are now in place to vet the next idea
  honestly.

## Related
[[2026-07-02-oos-validation]] · [[2026-07-02-param-sweep]] · [[2026-07-02-ntpc-backtest]] · [[Opening-Range-Breakout]]
