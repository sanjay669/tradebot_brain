---
tags: [ops, runbook, auth]
---
# Token Refresh

Upstox **access tokens expire every day** (~03:30 IST), so a fresh one is needed before
each session. Handled by `refresh_token.py`.

## Why the flow uses a hosted redirect page (not localhost)
Upstox's app form runs a **URL security filter** that **rejects localhost / raw-IP
redirect URIs** ("This URL isn't allowed by our security policy") — it requires a
stable, established HTTPS domain with a valid SSL cert. So the OAuth redirect points at
a small static page we host on **GitHub Pages**, and you paste the returned code back
into the script. There is no localhost callback server anymore.

- **Redirect page repo:** `sanjay669/upstox-auth` (public; contains only a static
  `index.html`, no secrets).
- **Live redirect URL:** `https://sanjay669.github.io/upstox-auth/`
- This exact URL must be set in **both** `.env` (`UPSTOX_REDIRECT_URI`) **and** the
  Upstox app's Redirect URL field — byte-for-byte identical, trailing slash included.

## What `refresh_token.py` does now (paste-code flow)
1. Opens the Upstox login dialog in your browser (uses `UPSTOX_API_KEY` +
   `UPSTOX_REDIRECT_URI`).
2. You log in with 2FA. Upstox redirects to the hosted page, which **displays the auth
   `code`** (with a copy button).
3. You **paste the code** (or the whole redirected URL) at the terminal prompt.
4. It exchanges the code for a token (using `UPSTOX_API_SECRET`) and **rewrites the
   `UPSTOX_ACCESS_TOKEN` line in `.env`** in place.

## Run it
```
python refresh_token.py
```

## Prerequisites in `.env` ([[Config-Reference]])
- `UPSTOX_API_KEY`, `UPSTOX_API_SECRET`
- `UPSTOX_REDIRECT_URI` = `https://sanjay669.github.io/upstox-auth/` — **must exactly
  match** the URL registered in the Upstox app settings.

## Why it's not fully unattended
Upstox requires an **interactive login with 2FA**, so this can't run headless without
extended-token/TOTP setup. Practical consequence:

- **Docker users:** run `refresh_token.py` on the **host** (needs a browser), then
  `docker compose up` — the container reads the fresh token via `env_file: .env` at
  start. See [[Docker]].

## Troubleshooting
- *"This URL isn't allowed by our security policy"* (Upstox app form) → you tried a
  localhost/IP/dynamic-DNS/tunnel URL. Use the github.io HTTPS page above.
- *Code paste fails / token exchange 400* → the `redirect_uri` sent must match the
  registered one exactly; also the code is single-use and expires fast — re-run.
- *401 / token errors when trading* → token expired or stale; re-run the refresh.
- Static IP: orders are only accepted from the **whitelisted IP**
  ([[SEBI-Algo-Trading-Rules]]).

## Related
[[Morning-Runbook]] · [[Upstox-API]] · [[Config-Reference]]
