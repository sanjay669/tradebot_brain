---
tags: [ops, incident]
---
# Incident Response

What to do when something goes wrong mid-session. **When in doubt, flatten and stop.**
A missed opportunity is cheap; an unmanaged position is not.

## Naked position (stop-loss failed to place)
The orchestrator already flattens the entry if the SL-M fails ([[Data-Flow]]). But verify:
1. Check the broker terminal for open positions vs. `trade_journal.csv`.
2. If a position has **no protective stop**, square it off manually **now**.
3. Log a [[Trade-Postmortem-Template|post-mortem]] — why did the stop fail?

## Bot won't start
| Symptom | Cause | Fix |
|---------|-------|-----|
| `UPSTOX_ACCESS_TOKEN not set` | token missing/expired | [[Token-Refresh]] |
| `Refusing to start: sandbox=false but live_confirmed=false` | one latch flipped | fix `config.yaml` ([[Config-Reference]]) — this is intentional |
| SDK import / attribute error | wrong SDK version | reinstall pinned `2.28.0` ([[Upstox-API]]) |

## `No LTP for <token>` warnings
Data feed returning nothing for an instrument. Check:
- Instrument key format correct? (`NSE_EQ|ISIN`).
- LTP response re-keying assumption still holds — see [[Upstox-API]].
- Market actually open and instrument tradable today.

## Daily loss cap hit
`RiskManager` halts all agents for the day (`halted = True`). This is **working as
designed** ([[Risk-Rules]]). Don't override it — do the [[Daily-Review-Template|review]]
instead and decide changes calmly after close.

## Order rejected by Upstox
- Static IP not whitelisted, insufficient margin, or instrument not allowed. Check the
  API error text; see [[SEBI-Algo-Trading-Rules]] for the IP/whitelist requirement.

## Kill switch
- Local: Ctrl-C the runner. Docker: `docker compose down`.
- Then manually flatten any open positions in the broker terminal.

## Related
[[Morning-Runbook]] · [[Risk-Rules]] · [[Trade-Postmortem-Template]]
