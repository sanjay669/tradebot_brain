---
tags: [journal, template, postmortem]
date:
instrument:
agent:
outcome:
---
# Trade Post-mortem — {{instrument}} {{date}}

> Copy to `Journal/` for any trade worth dissecting (big win, avoidable loss, near-miss,
> or a rule/stop that misbehaved). Part of [[Journal/README|the review loop]].

## The trade
| Field | Value |
|-------|-------|
| Agent | [[Opening-Range-Breakout]] / … |
| Side / qty | |
| Entry | |
| Stop (planned) | |
| Exit (actual) | |
| Notional | |
| Confidence | |
| P&L | |

## What the bot saw
- Signal reason (from journal): 
- Market context (VIX, opening range, tone): 

## What actually happened
- _Blow-by-blow: fill quality, did the stop trigger correctly, slippage, timing._

## Verdict
- Was the **entry** justified by the rules? y/n — why
- Was the **stop** placed and honored? ([[Risk-Rules]], [[Incident-Response]])
- Was this **process-good** (right by our rules) regardless of P&L? y/n

## Lessons
- _Separate luck from skill. A winning trade can be a bad decision and vice versa._

## Actions (DO NOT auto-apply)
- [ ] _Change_ — affects: [[Trading-Rules]] / [[Config-Reference]] / strategy note
