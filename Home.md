---
tags: [moc, index]
---
# 🧠 Home — Trader Brain

The map of content for the **upstox-trading-bot** knowledge base. Everything links
back here.

## 🚦 The hard gates (read before ever going live)
- [[Going-Live-Checklist]] — the checklist that must be 100% green
- [[Risk-Rules]] — account-level limits that can't be bypassed
- [[Trading-Rules]] — when the bot may and may not act
- [[SEBI-Algo-Trading-Rules]] — the regulatory boundary

## 🧩 How it works
- [[System-Overview]] — what the bot is (and isn't)
- [[Components]] — every module and its job
- [[Data-Flow]] — signal → risk → approval/execute → journal
- [[Config-Reference]] — every knob in `config.yaml` and `.env`

## 📈 Strategies
- [[Opening-Range-Breakout]] — the current example agent (unvalidated)
- [[Strategy-Template]] — copy this to document a new agent

## 🛠️ Operations
- [[Morning-Runbook]] — the daily start sequence
- [[Token-Refresh]] — the daily Upstox OAuth token
- [[Docker]] — containerized run
- [[Incident-Response]] — when something goes wrong mid-session

## 📚 Reference
- [[Upstox-API]] — verified SDK methods (v2.28.0) and endpoints
- [[Glossary]] — terms used across these notes

## 📓 The review loop
- [[Journal/README|Journal — how the review loop works]]
- [[Daily-Review-Template]] — end-of-day review
- [[Trade-Postmortem-Template]] — per-trade autopsy

---
### Current status
- Phase: **1 — sandbox only** (see [[System-Overview#Roadmap]])
- SDK pinned: `upstox-python-sdk==2.28.0`
- Safety latches: `sandbox: true` AND `live_confirmed: false` → **cannot place real orders**
