---
tags: [journal, research, walk-forward, results, conclusion]
date: 2026-07-03
---
# 2026-07-03 — Richer Intraday Edges: RS + Volume (walk-forward)

Pursuing the chosen "richer intraday edge" direction, added two market-context features to
the ORB breakout and validated each with the standard 24-month walk-forward (5 stocks,
1-min, 6mo-train/3mo-test, net of ~0.15% cost, out-of-sample).

## Results — both richer features also net-negative
| Experiment | OOS trades | OOS net/trade | Every fold neg? |
|---|---|---|---|
| Breakout + **relative strength vs NIFTY** | 447 | **−0.144%** | yes |
| Breakout + **volume-surge confirmation** | 685 | **−0.156%** | yes |

The RS filter cut trades roughly in half (447 vs 805) but did not lift net expectancy;
volume confirmation likewise. Neither crossed zero net-of-cost in any test window.

## Program-level conclusion (4 approaches now)
| Approach | OOS net/trade |
|---|---|
| ORB momentum breakout | −0.136% |
| VWAP mean-reversion | −0.157% |
| Breakout + relative strength | −0.144% |
| Breakout + volume confirmation | −0.156% |

**Four structurally distinct intraday signals — momentum, mean-reversion, and two
context-filtered variants — all land within a whisker of the −0.15% cost line.** The gross
edges are real but uniformly **smaller than the round-trip cost of trading them**. This is
a robust, out-of-sample result across 2 years and every regime, not a single-month fluke.

**Verdict: there is no net-of-cost edge to be found in simple/enriched 1-min intraday TA
on liquid large-caps at retail costs.** Continuing to add TA features is very likely to
keep hitting the same wall and mainly risks overfitting.

## Recommended re-evaluation (the fork, informed)
1. **Accept & keep as a sandbox/research platform** (recommended given the evidence) — the
   validated harness is the real asset; use it to vet any *future* idea honestly.
2. **Lower-frequency / swing** — where a multi-percent move dwarfs the 0.15% cost. Departs
   the intraday mandate but is where a durable edge is more plausible.
3. **A non-TA alpha source** (events/earnings drift, cross-sectional fundamentals,
   order-flow data) — a genuinely different, larger project; needs new data.

## Related
[[2026-07-02-vwap-reversion]] · [[2026-07-02-walkforward]] · [[2026-07-02-param-sweep]] · [[Opening-Range-Breakout]]
