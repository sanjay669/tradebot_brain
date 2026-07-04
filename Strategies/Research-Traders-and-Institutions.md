---
date: 2026-07-04
type: research
status: recorded
---

# What Indian traders & institutions actually do — verified research

Requested by user ("research famous traders' vlogs + institutional strategies for
stable daily profit, minimum risk"). Findings below separate REGULATOR DATA from
marketing claims.

## The regulator's numbers (SEBI, 2024 studies)

- **71% of individual intraday equity-cash traders made a NET LOSS in FY23**
  (sample: clients of top-10 brokers ≈ 86% of all individual clients).
- **Loss rate RISES to 80% for very frequent traders** (>500 trades/year) —
  direct, regulator-verified refutation of "many small trades sum to a big win".
- Loss-makers paid an EXTRA 57% of their losses as trading costs; even
  profit-makers paid 19% of profits as costs. Costs are the house edge.
- Younger traders (<30) lose more often (76%).
- F&O is worse: **93% of individual traders lost money FY22–FY24**, aggregate
  losses > ₹1.8 lakh crore.

## What the popular strategies are (vlogs/courses) — and our test results

The strategies famous Indian trading educators teach are precisely the families
this project already walk-forward tested on 24 months of data:

| Popular strategy | Our walk-forward verdict |
|---|---|
| Opening Range Breakout | net-negative every fold ([[2026-07-02-walkforward]]) |
| VWAP pullback/reversion | net-negative every fold |
| Volume-confirmed breakout | net-negative |
| "Trade 9:15–10:30 only" | CONFIRMED — our diagnostic found the same (pre-10:00 filter, applied) |
| "Risk max 1% per trade" | risk rule, not alpha — we implement stricter |

Pattern: the *risk-management* advice from serious traders is real and we apply
it; the *entry-signal* advice does not survive honest out-of-sample testing at
retail costs. Educators profit from courses, not disclosed track records.

## How institutions actually make money (and why retail can't copy it)

1. **Execution/cost advantage** — institutions pay a fraction of retail costs and
   use VWAP/TWAP algos to minimize impact. (Ironically, retail "VWAP strategies"
   exist because institutions benchmark THEIR EXECUTION to VWAP - it is an
   execution tool, not an entry signal.)
2. **Speed** — HFT/arbitrage desks capture spreads in microseconds; co-located
   servers at the exchange. Not accessible at retail latency.
3. **Information breadth** — analyst teams, alternative data, flow visibility.
   The retail-replicable slice of this is NEWS REACTION — hence this project's
   [[Learning-Loop|news sentiment pipeline]] collecting per-stock news history
   for future validation.
4. **Market making & premium selling** — earning the spread/theta with hedges
   retail cannot maintain.

There is no institutional "chart pattern" secret; their edge is structural.

## Implications adopted in the bot

- Fewer, filtered trades (pre-10:00 + wide-OR) — NOT more trades. SEBI's 80%
  figure for frequent traders settles this.
- Hard loss control: stop-loss, trailing, stale exit, daily cap, consecutive-loss
  breaker.
- "Stable daily profit with minimum risk" **does not exist in intraday retail
  trading** — 7 in 10 lose, and the stable-looking ones are selling courses.
  The honest goal: capital preservation while validating a real edge (news
  reaction per stock) on collected data.

Sources: SEBI press releases PR July 2024 (intraday study) and Sept 2024 (F&O
study); Business Standard, Moneylife coverage; strategy content from Indian
trading-education sites (ICFM, Sahi, Flattrade, Choice India) 2025–2026.

Related: [[2026-07-04-live-readiness-analysis]], [[Opening-Range-Breakout]], [[Learning-Loop]]
