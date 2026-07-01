---
tags: [rules, risk]
---
# Risk Rules

Account-level limits enforced by `risk_manager.py`. **Every** agent's proposed trade
passes through `RiskManager.check_trade()` before it can execute — this is what turns
"trade as much as possible" into something that can't blow up the account in one bad
session. See [[Data-Flow]] and [[Components]].

## The hard limits (from `config.yaml` → `account`)
| Rule | Key | Default | Meaning |
|------|-----|---------|---------|
| Daily loss cap (₹) | `daily_loss_cap_inr` | 2000 | All agents halt for the day once hit |
| Daily loss cap (%) | `daily_loss_cap_pct` | 2.0 | Whichever cap (₹ or %) triggers first wins |
| Approval threshold | `approval_threshold_inr` | 5000 | Trades above this notional queue for manual approval |
| Max leverage | `max_leverage` | 2.0 | Ceiling on margin use — a cap, not a target |
| Max trades/day (all) | `max_trades_per_day_total` | 15 | Across all agents combined |
| Max trades/day (agent) | `max_trades_per_agent_per_day` | 5 | Per agent |

## Non-negotiables
1. **No unprotected entries.** Every entry is paired with an SL-M stop immediately
   (`orchestrator.py`). If the stop fails to place, the entry is flattened. See
   [[Incident-Response]].
2. **The daily loss cap halts everything.** Once `record_fill()` pushes daily P&L past
   the effective cap (`min(₹cap, %cap)`), `halted = True` and no agent trades again
   that day. Counters reset only on a new calendar day.
3. **The shared risk manager is account-level, not per-agent.** Multiple agents share
   one budget so their loss limits add up correctly — see [[System-Overview]].
4. **Regime filter can veto.** Low confidence or high India VIX blocks trades — see
   [[Trading-Rules]].

## Change process
Never edit a risk limit mid-session on a whim. Propose the change here with a reason,
back it with [[Journal/README|journal]] evidence, then edit `config.yaml`. Tightening
is low-risk; loosening requires evidence.

## Related
[[Trading-Rules]] · [[Going-Live-Checklist]] · [[Config-Reference]] · [[SEBI-Algo-Trading-Rules]]
