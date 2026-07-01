---
tags: [journal, process]
---
# Journal — the review loop

This folder is the human side of the bot: **learning from mistakes, honestly.** The
code deliberately never rewrites its own strategy — self-modifying live strategies are
a well-known way for small bugs or a shifted regime to become large losses. Every
change goes through you, here.

## Where the raw data is
`upstox-trading-bot/trade_journal.csv` (or `./data/` under [[Docker]]) — one row per
**proposed** trade (executed, rejected, or queued) with its reasoning, confidence, and
risk-check result. Written by `journal.py`. See [[Data-Flow]].

## The loop
1. Run in **sandbox / live-small** for a stretch ([[System-Overview#Roadmap]]).
2. **Review** the journal (you, or hand the CSV to Claude) for patterns:
   - Which conditions actually preceded wins vs. losses?
   - Is the `confidence` score well-calibrated?
   - Is the regime filter too loose or too tight? ([[Trading-Rules]])
3. **Propose** specific parameter/rule changes with evidence.
4. **Approve** the change explicitly, then edit `config.yaml` / the agent.

## How to use this folder
- One note per trading day: `YYYY-MM-DD.md` from [[Daily-Review-Template]].
- One note per notable trade: from [[Trade-Postmortem-Template]].
- Link findings back to the rule/strategy they affect ([[Risk-Rules]],
  [[Opening-Range-Breakout]]) so decisions have a paper trail.

## Cadence
- **Daily** after close: quick review ([[Morning-Runbook]] step "After close").
- **Weekly:** scan for patterns across days before proposing config changes.

## Related
[[Home]] · [[Daily-Review-Template]] · [[Trade-Postmortem-Template]]
