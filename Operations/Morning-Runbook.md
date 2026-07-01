---
tags: [ops, runbook]
---
# Morning Runbook

The daily start sequence. You start the bot **manually** each trading morning (the OS
scheduler was intentionally skipped). All times IST.

## Pre-open (before 09:15)
1. **Confirm it's a trading day** — weekday and not an NSE/BSE holiday (the code only
   checks weekday; holidays are a [[Trading-Rules|known gap]]).
2. **Refresh the daily token** (Upstox tokens expire ~03:30 daily):
   ```
   python refresh_token.py
   ```
   Opens a browser, you log in (2FA), then **paste the code** shown on the hosted
   redirect page back at the prompt; token is written into `.env`. See [[Token-Refresh]].
3. **Sanity-check config** — `config.yaml` mode latches are where you expect them.
   Phase 1 = `sandbox: true`. See [[Config-Reference]] and [[Going-Live-Checklist]].

## Start the bot
Local:
```
python run_market_hours.py
```
Or Docker (after refreshing the token on the host):
```
docker compose up
```
It **waits for the 09:15 open**, loops until **15:30**, then exits. Start it any time
before/around the open.

## During the session
- Watch logs for `No LTP for ...` warnings (data/re-keying issue — see [[Upstox-API]]).
- Review queued trades when they appear:
  ```
  python approve_trades.py        # or: docker compose run --rm bot python approve_trades.py
  ```
- If something looks wrong → [[Incident-Response]].

## After close (15:30)
1. The runner exits on its own.
2. Do the [[Daily-Review-Template|end-of-day review]] against `trade_journal.csv`.
3. File any [[Trade-Postmortem-Template|post-mortems]] for notable trades.

## Related
[[Token-Refresh]] · [[Docker]] · [[Incident-Response]] · [[Journal/README]]
