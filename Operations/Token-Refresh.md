---
tags: [ops, runbook, auth]
---
# Token Refresh

Upstox **access tokens expire every day** (~03:30 IST), so a fresh one is needed before
each session. Handled by `refresh_token.py`.

## What it does
1. Opens the Upstox login dialog in your browser (uses `UPSTOX_API_KEY` +
   `UPSTOX_REDIRECT_URI`).
2. Catches the auth code on a localhost callback server.
3. Exchanges it for an access token (using `UPSTOX_API_SECRET`).
4. **Rewrites the `UPSTOX_ACCESS_TOKEN` line in `.env`** in place.

## Run it
```
python refresh_token.py
```

## Prerequisites in `.env` ([[Config-Reference]])
- `UPSTOX_API_KEY`, `UPSTOX_API_SECRET`
- `UPSTOX_REDIRECT_URI` — **must exactly match** the URI registered in the Upstox app
  settings (default `http://127.0.0.1:8765/callback`).

## Why it's not fully unattended
Upstox requires an **interactive login with 2FA**, so this can't run headless without
extended-token/TOTP setup. Practical consequence:

- **Docker users:** run `refresh_token.py` on the **host** (needs a browser), then
  `docker compose up` — the container reads the fresh token via `env_file: .env` at
  start. See [[Docker]].

## Troubleshooting
- *"Did not receive an auth code"* → redirect URI mismatch. Make `.env` and the Upstox
  app settings identical.
- *401 / token errors when trading* → token expired or stale; re-run the refresh.
- Static IP: orders are only accepted from the **whitelisted IP**
  ([[SEBI-Algo-Trading-Rules]]).

## Related
[[Morning-Runbook]] · [[Upstox-API]] · [[Config-Reference]]
