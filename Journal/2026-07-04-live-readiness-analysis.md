---
date: 2026-07-04
type: research
status: recorded
---

# Live-readiness analysis — user wants real money Monday 2026-07-06

User asked to "analyse past market and improve strategies" to start live Monday.
Full 24-month diagnostic run on the cached dataset (5 stocks, 1-min, current
config: bp=0.2, RR=2.5, stale=150, net of 0.15% cost).

## Diagnostic: where the losses come from

- **1090 trades, −0.171%/trade, negative in 22 of 24 months.**
- By entry time: 09:30–10:00 loses least (−0.104%); 11:00–12:30 worst (−0.309%).
  Late breakouts lose ~3x more.
- By opening-range width: only the widest quartile (≥1.30% of price) approaches
  break-even (−0.094%); everything narrower bleeds.

## Improvement found (and its honest limit)

Joint filter — breakout before 10:00 IST **and** OR width ≥ 1.3%:

| Variant | Trades | Win% | Net mean/trade |
|---|---:|---:|---:|
| Unfiltered (current live config) | 1090 | 35.7% | −0.171% |
| entry<10:00 only | 378 | 40.7% | −0.104% |
| OR≥1.3% only | 275 | 41.1% | −0.097% |
| **Both filters** | 116 | 47.4% | **−0.004%** |
| Both filters, OOS (6 test folds) | 84 | 48.8% | **+0.008%** (positive 2/6 folds) |

**Reading**: the filters remove ~90% of trades and take the strategy from
losing to BREAK-EVEN — not to profit. ~5 trades/month across 5 stocks; on one
stock with qty 1 it's ~1 trade/week. Fold-level results swing −0.79% to +0.40%.

APPLIED to the bot (TraderBot): `strategy.entry_filter.no_entries_after: "10:00"`
(orchestrator gate) + `min_or_width_pct: 1.3` (agent-side). Break-even beats
bleeding in sandbox, and fewer/better trades is the right posture either way.

## Recommendation on going live Monday: NO

1. Best validated expectancy after every improvement: **~0% minus execution
   slippage** (unmodelled) → still negative in practice.
2. Ops blocker regardless: Upstox rejects orders without the **static-IP
   whitelist** — not configured. Live would fail at the first order.
3. On Rs.1000 with qty 1 NTPC, even a perfect day is a few rupees; the realistic
   outcome is noise around zero with occasional −Rs.15 stops.
4. What WOULD justify live: a signal with net-positive OOS expectancy in ≥5/6
   walk-forward folds. Nothing tested (7 variants) has come close. The
   original project vision — news/sentiment as the entry signal, TA only for
   execution — is the untested alpha source; that is the research priority.

Latches remain `sandbox: true`, `live_confirmed: false`. Flipping them is the
user's decision and action alone; this analysis is the record of why the
recommendation is against it.

Related: [[2026-07-04-stale-exit-backtest]], [[2026-07-02-walkforward]],
[[Learning-Loop]], [[Opening-Range-Breakout]]
