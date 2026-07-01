---
tags: [reference, config]
---
# Config Reference

Every knob, and where it lives. Two files: `config.yaml` (behavior) and `.env` (secrets).

## `.env` — secrets (gitignored, never commit)
| Key | What |
|-----|------|
| `UPSTOX_API_KEY` | App API key (client id), from the Algo Trading App settings |
| `UPSTOX_API_SECRET` | App secret — treat like a password |
| `UPSTOX_ACCESS_TOKEN` | Daily token; written by `refresh_token.py` ([[Token-Refresh]]) |
| `UPSTOX_REDIRECT_URI` | OAuth redirect; hosted HTTPS page `https://sanjay669.github.io/upstox-auth/` (Upstox blocks localhost/IP). Must match the app's Redirect URL exactly — [[Token-Refresh]] |
| `DATA_DIR` (Docker) | Where journal/approvals are written; `/app/data` in the container |

## `config.yaml`
### `mode` — the safety latches
| Key | Default | Meaning |
|-----|---------|---------|
| `sandbox` | `true` | `false` required to place real orders |
| `live_confirmed` | `false` | `true` also required — BOTH needed to go live |

### `account` — [[Risk-Rules]]
| Key | Default | Meaning |
|-----|---------|---------|
| `total_capital_inr` | 100000 | Your actual deployable capital |
| `daily_loss_cap_inr` | 2000 | Hard stop (₹) — all agents halt |
| `daily_loss_cap_pct` | 2.0 | Hard stop (%); tighter of the two wins |
| `approval_threshold_inr` | 5000 | Above this notional → manual approval |
| `max_leverage` | 2.0 | Margin ceiling |
| `max_trades_per_day_total` | 15 | Across all agents |
| `max_trades_per_agent_per_day` | 5 | Per agent |

### `regime_filter` — [[Trading-Rules]]
| Key | Default | Meaning |
|-----|---------|---------|
| `enabled` | `true` | Master switch for the regime checks |
| `india_vix_block_above` | 20.0 | Block trades when VIX above this |
| `min_signal_confidence` | 0.6 | Agents below this don't trade |

### `instruments_allowed`
Start with `equity_cash` only; add `futures` / `options` per [[System-Overview#Roadmap]].

### `agents_enabled`
Which agents run. Must also be constructed in `orchestrator.build_agents()`.

## Related
[[Risk-Rules]] · [[Going-Live-Checklist]] · [[Upstox-API]]
