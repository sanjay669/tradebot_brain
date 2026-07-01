---
tags: [strategy, template]
status: draft
instrument:
---
# <Strategy Name>

> Copy this note to document a new agent before you write it. A strategy you can't
> explain in one paragraph isn't ready to trade. Keep it white-box ([[SEBI-Algo-Trading-Rules]]).

## Idea
_One paragraph: what edge does this capture, and why should it exist?_

## Instrument(s)
_Which instrument keys, which segment (equity_cash / futures / options)._

## Entry rule
- _Precise, testable condition(s)._
- Reported `confidence`: _how is it computed?_ (must clear `min_signal_confidence`).

## Exit rule
- **Stop:** _mandatory — never propose an unprotected entry ([[Risk-Rules]])._
- **Target / time exit:** _._

## Data dependencies
- _What does `generate_signal()` need in `market_data`? Wire it in `MarketDataFeed`._

## Parameters
| Param | Default | Notes |
|-------|---------|-------|
| | | |

## Validation checklist
- [ ] Backtested (`backtest.py`) over a meaningful period.
- [ ] Paper-traded in sandbox.
- [ ] Results reviewed in [[Journal/README|journal]]; win rate/expectancy trusted.
- [ ] Added to `config.yaml → agents_enabled` and `orchestrator.build_agents()`.

## Open questions
- _._

## Related
[[Opening-Range-Breakout]] · [[Trading-Rules]] · [[System-Overview#Roadmap]]
