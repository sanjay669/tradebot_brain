---
tags: [ops, docker]
---
# Docker

The bot is containerized. **Secrets are injected at runtime, never baked into the
image**, and the journal/approval queue persist on a mounted `./data` volume.

## Files
- `Dockerfile` — `python:3.12-slim`, installs pinned deps, default `CMD` runs
  `run_market_hours.py`. Sets `DATA_DIR=/app/data`.
- `docker-compose.yml` — service `bot`; `env_file: .env`; volume `./data:/app/data`;
  `restart: "no"` (the intraday runner exits at 15:30 — don't restart into a closed market).
- `.dockerignore` — excludes `.env`, `.git`, caches, and runtime data.
- `tzdata` is installed so `Asia/Kolkata` gating is correct regardless of host clock.

## Daily workflow
```
python refresh_token.py     # on the HOST — needs a browser (see [[Token-Refresh]])
docker compose build        # one-time, and after code/dep changes
docker compose up           # runs the bot; waits for 09:15 IST, exits at 15:30
```

## One-offs (same image)
```
docker compose run --rm bot python backtest.py --csv data/candles.csv
docker compose run --rm bot python approve_trades.py
```

## Notes
- Rebuild after changing `requirements.txt` (e.g. SDK pin — see [[Upstox-API]]).
  Verify: `docker run --rm upstox-trading-bot pip show upstox-python-sdk`.
- `trade_journal.csv` / `pending_approvals.json` land in `./data` on the host via
  `DATA_DIR`. See [[Components]].
- Token refresh stays on the host because OAuth needs a browser — the container just
  reads the resulting `.env`.

## Related
[[Morning-Runbook]] · [[Token-Refresh]] · [[Config-Reference]]
