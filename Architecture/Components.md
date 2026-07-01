---
tags: [architecture, components]
---
# Components

Module-by-module map of `upstox-trading-bot`. See the runtime trace in [[Data-Flow]].

| File | Role |
|------|------|
| `run_market_hours.py` | Entry point. Gates the loop to 09:15–15:30 IST (weekdays), polls `MarketDataFeed`, calls `orchestrator.run_once()` each tick, exits at close. See [[Morning-Runbook]]. |
| `orchestrator.py` | Wires it together: agents → risk check → journal → approve/queue or execute. Places the **SL-M stop** right after each entry; flattens if the stop fails. |
| `agents/base_agent.py` | Abstract `BaseAgent` — the strategy interface (`generate_signal`). |
| `agents/equity_intraday_agent.py` | Example [[Opening-Range-Breakout]] agent (unvalidated). |
| `risk_manager.py` | Account-level `RiskManager.check_trade()` / `record_fill()`. The [[Risk-Rules]] enforcer. |
| `market_data.py` | `MarketDataFeed`: LTP + India VIX from `MarketQuoteApi`; accumulates the 09:15–09:30 IST opening range locally. See [[Upstox-API]]. |
| `upstox_client_wrapper.py` | Thin `UpstoxClient` over the SDK. Enforces the two safety latches; `place_order()`. |
| `approval_queue.py` | Parks above-threshold trades in `pending_approvals.json`. |
| `approve_trades.py` | CLI to review/approve/reject queued trades. |
| `journal.py` | Appends every proposed trade + reasoning to `trade_journal.csv`. Feeds [[Journal/README|the review loop]]. |
| `refresh_token.py` | Daily Upstox OAuth token refresh → writes `.env`. See [[Token-Refresh]]. |
| `backtest.py` | Offline replay of the agent over historical candles. |
| `config.yaml` | Risk / mode / strategy config. See [[Config-Reference]]. |
| `.env` | Secrets (gitignored). See [[Config-Reference]]. |
| `Dockerfile` / `docker-compose.yml` | Containerized run. See [[Docker]]. |

## Persistence
- `trade_journal.csv` — every proposed trade (executed/rejected/queued).
- `pending_approvals.json` — the approval queue.
- Both honor `DATA_DIR` so they live on the mounted `./data` volume under [[Docker]].

## Related
[[System-Overview]] · [[Data-Flow]] · [[Home]]
