---
tags: [compliance, sebi, regulatory]
---
# SEBI Algo-Trading Rules

> Not legal advice. SEBI's retail algo-trading framework is still settling in — verify
> current requirements directly with Upstox/your broker before trading live.

Context: SEBI's **February 2025 retail algo-trading circular** is in force. What it
means for a personal bot like this one:

## What's required
- **No open/unrestricted API access.** Upstox requires a client-specific API key and a
  **whitelisted static IP**, with OAuth + 2FA. → home connections without a static IP
  need a VPS. See [[Token-Refresh]] and [[Going-Live-Checklist]].
- **App type:** create an **Algo Trading App** in Upstox (not a plain app).

## What's allowed without registering the algo
- A **"tech-savvy retail investor"** running their own algo for their **own account**,
  under **10 orders/second**, does **not** need to register the algorithm with the
  exchange. This bot stays far under that — `max_trades_per_day_total: 15` by default
  (see [[Risk-Rules]]), nowhere near 10/sec.

## Constraints if you ever did register
- A registered algo may only be used for **yourself, spouse, and dependent
  children/parents** — never offered to other people.

## Good-practice expectations
- **White-box, not black-box.** Keep strategy logic transparent and explainable —
  both for the regulator and so you can read *why* it traded in the
  [[Journal/README|journal]]. See [[Opening-Range-Breakout]].

## Our posture
- Order rate: guaranteed low by [[Risk-Rules]] caps.
- Scope: single account, personal use only.
- Transparency: every proposed trade logged with reasoning — see [[Data-Flow]].

## Related
[[Going-Live-Checklist]] · [[Risk-Rules]] · [[System-Overview]]
