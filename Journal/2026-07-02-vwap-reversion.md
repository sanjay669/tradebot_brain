---
tags: [journal, backtest, walk-forward, research, results]
date: 2026-07-02
---
# 2026-07-02 — New Signal #1: VWAP Mean-Reversion (walk-forward)

After [[2026-07-02-walkforward|ORB was invalidated]], prototyped a **structurally
different** signal: intraday **VWAP mean-reversion** — go long when price dislocates
≥ dev% below the day's volume-weighted average price; exit on reversion to VWAP or a
stop% below entry, else EOD. Same rigor: 24 months, 5 liquid stocks, 1-min **with
volume**, rolling 6mo-train/3mo-test, tune (dev, stop) on train, apply to unseen test.
Net of ~0.15% round-trip cost.

## Out-of-sample result — also net-negative
| Fold | Test | chosen | Test win% | Test net%/trade |
|---|---|---|---|---|
| 0 | 25-01..03 | dev0.8 stop1.0 | 65.9 | −0.033 |
| 1 | 25-04..06 | dev0.8 stop1.0 | 47.1 | −0.298 |
| 2 | 25-07..09 | dev0.8 stop1.5 | 65.4 | −0.087 |
| 3 | 25-10..12 | dev0.3 stop1.0 | 66.3 | −0.128 |
| 4 | 26-01..03 | dev0.5 stop1.5 | 63.0 | −0.165 |
| 5 | 26-04..06 | dev0.8 stop1.5 | 56.1 | −0.280 |

**Aggregate OOS: 709 trades, net −0.157%/trade, net total −111%.** Every train and test
fold negative. Interesting: **high gross win rate (54–66%)** but still a net loser —
reversion winners are small (snap back to the mean) while stops are wider, and costs
erase the thin edge.

## Combined conclusion across two signal families
Two structurally opposite intraday signals — **momentum breakout (ORB)** and
**mean-reversion (VWAP)** — both fail the same net-of-cost walk-forward. The binding
constraint is not the signal; it's that **per-trade edges from simple 1-min price/volume
TA on liquid large-caps are smaller than the ~0.15% round-trip cost.** This matches market
reality (that edge is largely arbitraged away / captured by market makers).

## Implication / strategic fork (needs a direction call)
Grinding more simple intraday-TA variants will likely keep hitting the same cost wall and
risks overfitting. Real options:
1. **Lower frequency / bigger moves** (multi-day swing) so cost becomes a negligible % —
   but that departs from the "intraday" mandate.
2. **A richer edge source** than price/volume TA (e.g. events, cross-sectional ranking,
   order-flow) — a bigger project.
3. **Accept intraday-on-liquid-large-caps is not net-profitable** with retail costs; keep
   the bot as a sandbox/learning platform only.

## Related
[[2026-07-02-walkforward]] · [[2026-07-02-param-sweep]] · [[Opening-Range-Breakout]] · [[Trading-Rules]]
