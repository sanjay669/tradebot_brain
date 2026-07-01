---
tags: [architecture, flow]
---
# Data Flow

One tick of `orchestrator.run_once()`, per agent. Modules in [[Components]].

```
                   ┌─────────────────────────────────────────────┐
 MarketDataFeed ──▶│ market_data_by_instrument                    │
 (ltp, VIX, OR)    │  {token: {ltp, opening_range_high/low, vix}} │
                   └───────────────────┬─────────────────────────┘
                                       ▼
                          agent.generate_signal(md)
                                       │  None? → skip (the common case)
                                       ▼
                         notional = price × quantity
                                       ▼
                     risk_mgr.check_trade(...)  ◀── [[Risk-Rules]] + [[Trading-Rules]]
                                       │
              ┌────────────────────────┼───────────────────────────┐
              ▼                        ▼                            ▼
        not allowed              requires_approval               allowed
     journal: rejected       journal: pending                journal: pending
              │              queue → pending_approvals.json         │
              │              (review via approve_trades.py)         ▼
              ▼                        ▼                    client.place_order(entry)
            done                      done                          ▼
                                                        client.place_order(SL-M stop)
                                                          fail? → flatten entry
```

## Key points
1. **Every** proposal is journaled — executed, rejected, or queued — with its reasoning.
   This is the raw material for [[Journal/README|the review loop]].
2. **Risk check is the gate.** Order: halted? → trade-count caps → leverage → regime
   (confidence, VIX) → approval threshold. First failure wins.
3. **Approval split:** notional > `approval_threshold_inr` → queue; else auto-execute.
   See [[Trading-Rules]].
4. **Entry + stop are atomic-ish:** the SL-M stop is placed immediately after the
   entry; if it fails, the entry is flattened so no naked position survives
   ([[Incident-Response]]).
5. `record_fill()` updates daily P&L on close → can trip the [[Risk-Rules|daily loss cap]].

## Related
[[System-Overview]] · [[Components]] · [[Config-Reference]]
