---
tags: [journal, backtest, tuning, results]
date: 2026-07-02
---
# 2026-07-02 — Autonomous Parameter Sweep (ORB)

Follow-up to [[2026-07-02-ntpc-backtest]]. Applied the reward:risk fix (take-profit =
entry + R×(entry−stop) instead of a fixed 0.5%) and swept parameters across **7 liquid
stocks × 3 months** (NTPC, ITC, ONGC, COALINDIA, POWERGRID, WIPRO, BANKBARODA; Apr–Jun
2026; 1-min; 22,500 candles each). Trade returns aggregated in **% (comparable across
instruments)**.

## Sweep result (gross, before costs)
The R-multiple fix flipped the strategy **gross-positive** at every RR ≥ 1.5. Best combos:

| breakout% | RR | trades | win% | mean%/trade | total% |
|---|---|---|---|---|---|
| 0.2 | **2.5** | 167 | 50.3 | **0.044** | 7.31 |
| 0.1 | 2.5 | 193 | 47.7 | 0.042 | 8.20 |
| 0.1 | 3.0 | 193 | 47.7 | 0.042 | 8.02 |

Low win rate (~50%) but positive expectancy because winners run to 2.5R while losers
stop out at 1R. **Confirmed: the fix works directionally.** Applied `reward_risk_ratio:
2.5` (breakout stays 0.2%).

## The verdict that matters: NET of costs it still LOSES
- Upstox intraday round-trip cost ≈ **~0.15%** (0.1% brokerage + STT 0.025% + GST +
  exchange/stamp).
- Best gross edge = **+0.044%/trade** → net **≈ −0.11%/trade** → +7.31% gross becomes
  **≈ −18%** after costs over the sample.
- **The ORB edge is smaller than the cost of trading it.** Not viable for real money as-is.

## Honest conclusions
1. Fixing reward:risk was necessary and correct, but **insufficient** — the raw ORB
   signal is too weak to clear costs.
2. To be viable the strategy needs a **materially bigger edge per trade** (fewer,
   higher-quality entries), not more trades. Candidate levers: min opening-range width
   filter, time-of-day filter, volume/VIX filter, trend alignment.
3. **Overfitting risk is real** — 167–193 trades over 3 months is a small sample. Any
   further tuning MUST be validated out-of-sample (different months/stocks), or we're
   just curve-fitting noise.

## Next (autonomous, offline)
Add an entry-quality filter, re-sweep, and **hold out** a test period to check it
generalizes — only keep changes that survive out-of-sample. Do NOT go live until net-of-
cost expectancy is positive out-of-sample. [[Going-Live-Checklist]] · [[Risk-Rules]]

## Related
[[Opening-Range-Breakout]] · [[2026-07-02-ntpc-backtest]] · [[Journal/README]]
