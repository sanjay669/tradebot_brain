---
tags: [reference, api, upstox]
---
# Upstox API

Verified against **`upstox-python-sdk==2.28.0`** (pinned in `requirements.txt`). These
are the exact methods/fields the code relies on — re-verify if you ever bump the SDK.

## Auth
- OAuth dialog → auth code → token exchange (`LoginApi.token`). Handled by
  `refresh_token.py`; tokens expire daily. See [[Token-Refresh]].
- Static IP whitelist + 2FA required — [[SEBI-Algo-Trading-Rules]].

## Market data (`MarketQuoteApi`)
- `ltp(symbol, api_version)` — `symbol` is comma-separated instrument keys;
  `api_version="2.0"`.
- Response: `resp.data` is a dict keyed by `"EXCHANGE:SYMBOL"` (e.g. `NSE_EQ:RELIANCE`).
  Each value is a **`MarketQuoteSymbolLtp`** with fields **`last_price`** and
  **`instrument_token`**.
- `market_data.py` re-keys by `instrument_token` so callers look up by the key they
  passed in. ⚠️ **Runtime check:** confirm `instrument_token` comes back in the same
  `NSE_EQ|ISIN` format you sent. If not, fix `_ltp_map` (one line). Watch for
  `No LTP for ...` warnings ([[Incident-Response]]).
- India VIX key: `NSE_INDEX|India VIX`.

## Historical candles (`HistoryApi`) — used by `backtest.py`
- `get_historical_candle_data1(instrument_key, interval, to_date, from_date, api_version)`.
- `resp.data.candles` = list of `[timestamp, open, high, low, close, volume, oi]`.
- Intraday minute history is limited; also `get_intra_day_candle_data(...)` for today.

## Orders (`OrderApiV3`) — used by `upstox_client_wrapper.py`
- `place_order(body)` where `body` is a **`PlaceOrderV3Request`** with:
  `quantity, product, validity, price, tag, slice, instrument_token, order_type,
  transaction_type, disclosed_quantity, trigger_price, is_amo` (+ optional
  `market_protection`).
- `product="I"` intraday, `validity="DAY"`.
- `order_type`: `MARKET`, `LIMIT`, `SL`, `SL-M`. The protective stop uses **`SL-M`**
  with `trigger_price` ([[Risk-Rules]]).

## Not yet wired (stubs, but SDK support exists)
- `PortfolioApi` → positions (`get_positions` stub).
- `UserApi` → funds/margin (`get_funds_and_margin` stub).

## Related
[[Components]] · [[Config-Reference]] · [[Data-Flow]]
