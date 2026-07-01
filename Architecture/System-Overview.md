---
tags: [architecture, overview]
---
# System Overview

A **sandbox-first, risk-managed intraday trading bot** for Upstox. It's a foundation
to build on — **not** a finished, profitable strategy. The example
[[Opening-Range-Breakout]] agent is unvalidated and must not be trusted with real
money as-is.

## What it is / isn't
- Trades **equities / futures / options** via the Upstox API — **not mutual funds**
  (those settle once daily at NAV and can't be traded intraday).
- **"Multiple agents"** = multiple strategy modules sharing **one account-level**
  [[Risk-Rules|risk manager]], so their loss limits add up correctly — not multiple
  independent bots each spending the daily loss budget separately.
- Built around a clear preference: small trades auto-execute, larger ones queue for
  manual approval, and the bot **sits out** when confidence is low or India VIX is
  high. See [[Trading-Rules]].

## Two safety latches
Live trading requires **both** `config.yaml` flags:
`mode.sandbox: false` **AND** `mode.live_confirmed: true`. Either left safe → the
`UpstoxClient` refuses to place real orders. See [[Config-Reference]] and
[[Going-Live-Checklist]].

## Big picture
```
agents → risk_manager → approval_queue / execution → journal
              ↑                                          ↓
        config.yaml + .env                     the review loop (you)
```
Full trace in [[Data-Flow]]; module-by-module in [[Components]].

## Roadmap
Don't skip steps.
1. **Phase 1 (current):** single equity-cash agent, **sandbox only**, risk manager validated.
2. **Phase 2:** same agent, live with **small size**, `approval_threshold_inr` low so
   you approve most trades manually.
3. **Phase 3:** add F&O/options once Phase 2 has weeks of trusted [[Journal/README|journal]]
   data — raise `max_leverage` deliberately.
4. **Phase 4:** add more agents (one per instrument/strategy), each independently proven;
   the shared risk manager keeps combined risk bounded.

## Related
[[Components]] · [[Data-Flow]] · [[SEBI-Algo-Trading-Rules]] · [[Home]]
