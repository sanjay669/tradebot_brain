---
tags: [reference, glossary]
---
# Glossary

Terms used across this vault.

- **Agent** — one strategy module implementing `generate_signal()`. Never talks to the
  broker directly; the orchestrator does. See [[Components]].
- **Approval threshold** — notional value above which a trade queues for manual
  approval instead of auto-executing. See [[Trading-Rules]].
- **Daily loss cap** — the hard ₹/% loss that halts all agents for the day.
  See [[Risk-Rules]].
- **India VIX** — NSE's volatility index (key `NSE_INDEX|India VIX`). High = choppy;
  the regime filter blocks trades above a threshold.
- **Instrument key / token** — Upstox identifier, format `EXCHANGE|ISIN`
  (e.g. `NSE_EQ|INE002A01018` = RELIANCE). See [[Upstox-API]].
- **Intraday (`product="I"`)** — positions squared off same day; vs. delivery (`"D"`).
- **LTP** — Last Traded Price. See [[Upstox-API]].
- **Notional** — `price × quantity`; the trade's rupee value used for the approval check.
- **Opening range (OR)** — high/low of the first 15 min (09:15–09:30 IST). Basis of the
  [[Opening-Range-Breakout]] strategy; built locally by `MarketDataFeed`.
- **Regime filter** — the "don't trade when unpredictable" checks (confidence + VIX).
  See [[Trading-Rules]].
- **Safety latches** — the two `config.yaml` flags (`sandbox`, `live_confirmed`) that
  both must be set to trade live. See [[System-Overview]].
- **Sandbox** — Upstox's fake-fill environment; no real money. Phase 1 runs here only.
- **SL / SL-M** — Stop-Loss (limit) / Stop-Loss-Market order. Protective stops use
  `SL-M` with a `trigger_price`. See [[Risk-Rules]].
- **The review loop** — the human process of reading the journal and proposing changes.
  See [[Journal/README]].

## Related
[[Home]] · [[Upstox-API]] · [[Config-Reference]]
