---
type: operations
created: 2026-07-04
status: active
---

# Learning Loop — daily review, lessons, and approved rules

How the bot "learns from mistakes" without ever mutating its own live strategy:
a propose → approve → apply pipeline built into the TraderBot repo (2026-07-04).

## The pipeline

1. **Trade** — every entry/exit/rejection is journaled to `data/trade_journal.csv`
   (unchanged; see [[Data-Flow]]).
2. **Review** — after the close, run `python review_day.py` (host or
   `docker compose run --rm bot python review_day.py`). `learning.py`:
   - scores the day (win rate, net P&L, avg win/loss, profit factor, P&L by
     exit reason and by instrument);
   - detects **mistakes**: avg loss > avg win (reward:risk inverted), losing
     end-of-day square-offs, losses clustered at marginal confidence, an
     instrument that lost ≥3 times, using the full trade allowance on a red day;
   - detects **correct moves**: take-profit exits net positive, green day with
     profit factor ≥ 1.5, risk-manager rejections that saved a red day;
   - appends lessons (deduped by id) to `data/lessons.json`;
   - writes `Journal/YYYY-MM-DD-review.md` into THIS vault.
3. **Propose** — lessons that imply a config change land in `data/proposals.json`
   as `pending`: `block_instrument`, `raise_min_confidence`, `reduce_max_trades`.
4. **Approve (human)** — `python apply_proposals.py` lists them;
   `--approve <id>` moves the rule into `lessons.json["approved_rules"]`,
   `--reject <id>` discards it. Nothing activates without this step.
5. **Apply** — on next start, `RiskManager` loads `approved_rules` and enforces
   them: blocked instruments are rejected outright, the confidence floor and the
   max-trades cap take the STRICTER of config vs. learned rule. Learned rules can
   only tighten, never loosen (`test_overrides_never_loosen_config`).

## Also added 2026-07-04

- **Trailing stop** in the orchestrator (config `strategy.trailing_stop`): once a
  long is up 1R the stop moves to breakeven, beyond that it trails 1R below the
  high-water mark; exits journal as `trailing-stop` when the ratcheted stop is at
  or above entry. Stops only ever move up.
- **Dashboard**: `/api/daily` per-day P&L (bar chart + table) and `/api/lessons`
  (learning panel: lessons, rules in force, pending proposals).
- **Stale exit** (added after the first backfilled reviews, config
  `strategy.stale_exit`, default 150 min): positions that hit neither target nor
  stop within `max_hold_minutes` are flattened with reason `stale-exit`. Motivated
  by the 2026-06-29..07-02 reviews: every loss in those four sandbox days came from
  dead trades held to the 15:20 square-off, while all take-profits resolved sooner.

## Honest limits (unchanged — see [[2026-07-03-richer-edge-results]])

No mechanism here **ensures** daily profit — nothing can. Four signal families
(ORB, VWAP reversion, RS-filtered and volume-confirmed breakouts) all walk-forward
net-negative vs the ~0.15% round-trip cost hurdle; the ORB strategy remains
**invalidated**. The learning loop reduces repeated mistakes and enforces loss
discipline (daily loss cap, stops, trailing stops); it does not conjure an edge.
Stay in sandbox until a strategy passes the walk-forward harness net of costs.

Related: [[Morning-Runbook]], [[System-Overview]], [[Data-Flow]]
