---
tags: [journal, backtest, validation, results]
date: 2026-07-02
---
# 2026-07-02 — Out-of-Sample Validation (ORB filters)

Iteration 3, following [[2026-07-02-param-sweep]]. Tested whether entry-quality filters
(time-of-day cutoff, min opening-range width) can lift the ORB edge above trading costs,
with a **train/test split to guard against overfitting**. 7 stocks; Train = Apr+May 2026,
Test = June 2026; breakout 0.2%, RR 2.5; metric = NET-of-cost (gross − 0.15%/trade).

## Result — no robust edge
- **TRAIN (Apr+May): every filter combo is net-NEGATIVE** (−0.19% to −0.31%/trade). The
  time-of-day and OR-width filters did **not** improve net expectancy — the least-bad
  configs were effectively no-filter.
- **TEST (June): the same configs look net-POSITIVE** (+0.06 to +0.10%/trade, 59–62% win,
  +3% to +5% net total).

## Interpretation (the important part)
The strategy **loses on Apr+May and wins on June** → that's **regime dependence / luck,
not a durable edge.** The earlier single-month NTPC backtest and the June-only numbers
looked encouraging precisely because June was a good month. **Across a longer window the
net edge is negative.** The out-of-sample discipline caught a false positive we'd
otherwise have acted on.

## Decision (autonomous)
- **Stop parameter tuning here.** More sweeping over 3 months × 7 stocks = curve-fitting
  a small, noisy sample. Chasing it would manufacture a fake backtest winner.
- **Keep** `reward_risk_ratio: 2.5` (best *gross* config, harmless) but the honest status
  is: **simple ORB has no net-of-cost edge — DO NOT trade real money on it.**
- Stay in **sandbox / paper** only. [[Going-Live-Checklist]] stays red.

## What real improvement would require (not one-session tweaks)
1. **More data + walk-forward** validation (1–2 yrs, rolling train/test) — 3 months can't
   confirm an edge.
2. **A different/stronger signal**, not just opening-range breakout — the raw ORB edge is
   below costs on liquid large-caps.
3. Lower trade frequency / higher conviction so the per-trade edge clears ~0.15% cost.

## Related
[[2026-07-02-param-sweep]] · [[2026-07-02-ntpc-backtest]] · [[Opening-Range-Breakout]] · [[Risk-Rules]]
