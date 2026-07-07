# 2026-07-07 — Swing strategies walk-forward + roster expansion

## Context
User directive: "continue with other strategies, learn from mistakes, improve app,
try other stocks, check relevant news." The 1-min intraday conclusion was already
robust (4 families, all ~−0.15%/trade OOS = the cost hurdle), so "other strategies"
meant moving UP the timeframe, where moves are larger than costs.

## What was tested (new walk-forward harness, daily candles)
- **Universe**: 25 liquid NSE large caps, 36 months of daily candles
  (2023-07 → 2026-07) via Upstox History API.
- **Folds**: rolling 12mo-train / 6mo-test × 4; params picked on train only.
- **Costs**: honest DELIVERY round trip at Rs.10k position = **0.89%**
  (Rs.20×2 brokerage + 0.1%×2 STT + DP Rs.20 + stamp/exchange/GST —
  the Rs.67.5 of fixed charges dominate small accounts).
- Scripts committed to `TraderBot/research/` (reusable harness).

## Results — all three families REJECTED out-of-sample
| family | OOS result | folds +ve |
|---|---|---|
| Donchian momentum breakout (20/55-day) | −2.0 to −4.1%/trade, 22–29% win rate | 0/4 |
| RSI(2) mean reversion above SMA200 | −0.08 to −2.0%/trade | 0/4 |
| Monthly top-5 momentum rotation | lagged equal-weight benchmark −2.8 to −8.7% per half-year | 0/4 |

Benchmark context: equal-weight buy-and-hold of the same 25 stocks over the
2-year OOS window ≈ **+1.5% total** — a flat/choppy regime. Long-only momentum
bleeds in such regimes; its OOS losses (≈2–4%/trade) far exceed the 0.89% cost,
so unlike intraday (cost-bound), the swing momentum SIGNAL itself was negative.

## Cumulative verdict (now 8 families across 2 timeframes)
ORB, VWAP reversion, RS-filtered breakout, volume-confirmed breakout (1-min);
stale-exit overlay (neutral); Donchian swing, RSI-2 swing, monthly rotation (daily).
**Nothing survives walk-forward net of retail costs.** Stop testing TA variants —
each new failure now adds little information. The durable assets are (1) the
walk-forward harness, (2) the growing news-vs-price dataset
(`data/news_history.csv`, +500 headlines today after adding the new stocks),
which is the only untested alpha source in hand.

## App improvements shipped
- **Strategy Research panel** on the dashboard (`/api/research` +
  `data/research_results.json`): every family, its OOS result, and verdict —
  the negative results are now permanently visible instead of buried in journals.
- **Roster expanded 5 → 10 paper agents**: added Hindalco, Bajaj Finance,
  Infosys, Wipro, SBI — picked by data (highest avg daily range % among stocks
  affordable at qty 1), because the all-PSU roster was too quiet: on 07-07,
  4/5 stocks failed the 1.3% opening-range filter and the 5th never broke out
  (a correct no-trade day). News pipeline covers the new names automatically;
  today's check: no negative-news blocks on any of the 10.

## Lessons
1. **Changing timeframe does not manufacture an edge** — it just changes which
   constraint binds (intraday: costs; swing: weak/regime-dependent signal).
2. **Fixed charges are brutal on small delivery positions**: Rs.67.5 fixed ≈
   0.68% of a Rs.10k position before the variable costs. Any swing idea for a
   small account must clear ~1% per round trip.
3. The no-trade day was the system working, not failing — selectivity is the
   only thing that made intraday ~breakeven.

Links: [[2026-07-02-walkforward]], [[2026-07-03-richer-edge-results]],
[[2026-07-04-stale-exit-backtest]], [[Opening-Range-Breakout]]
