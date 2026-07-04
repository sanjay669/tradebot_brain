---
date: 2026-07-04
type: research
status: recorded
---

# News sentiment vs subsequent price moves — first validation

456 dated headlines (5 stocks, backfilled from Google News RSS, lexicon-scored)
matched to their affected trading session in the 24-month candle cache. Forward
return = publication (or session open) -> session close.

## Results

| Sentiment bucket | n | Up-close rate | Mean fwd return |
|---|---:|---:|---:|
| positive (>0) | 113 | 46.9% | −0.089% |
| neutral (0) | 277 | 46.9% | −0.051% |
| negative (<0) | 66 | 47.0% | −0.054% |
| strong negative (≤−2) | 18 | 44.4% | −0.295% |
| **gate level (≤−3)** | **13** | **30.8%** | **−0.561%** |

Spearman rank correlation overall: +0.08 (≈ none).

## Honest reading

1. **Mild sentiment carries NO signal.** Positive, neutral, and mildly negative
   headlines all show the same ~47% up rate. Notably, positive news does NOT
   predict up moves (its mean is slightly WORSE than neutral) — the
   never-auto-buy-on-good-news design decision is now evidence-backed.
2. **The negative TAIL is real and monotonic**: ≤−2 → −0.30%, ≤−3 → −0.56% with
   only a 31% up rate. The live entry gate (block at ≤−3) sits exactly where the
   signal lives: sessions it blocks averaged −0.56%. The gate earns its place
   as a risk filter.
3. **Limits**: n=13 at the gate level is far too small to declare an edge;
   many "crash" headlines are REACTIVE (reporting a move already underway), so
   part of the measured drift is continuation after bad-news opens - acceptable
   for a defensive gate (that drift is exactly what a long should avoid), not
   proof of short alpha. Google-News backfill has selection bias. Keep
   collecting `news_history.csv` daily; revisit at n≥50 in the gate bucket.

DECISION: keep `news.block_below: -3` unchanged; keep positive news inert;
continue daily collection. Next milestone: re-run this study when the gate
bucket reaches ~50 samples from LIVE-collected (non-backfilled) headlines.

Validation script: `news_validation.py` (session scratchpad, reusable).

Related: [[Learning-Loop]], [[Research-Traders-and-Institutions]],
[[2026-07-04-live-readiness-analysis]]
