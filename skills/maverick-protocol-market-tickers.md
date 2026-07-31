---
name: maverick-market-tickers
description: >-
  Retrieve 24-hour market pricing and volume statistics for every Maverick V2
  pool/pair on a given EVM chain, using the public Maverick V2 Data API.
api: Maverick V2 Data API
server: https://v2-api.mav.xyz
operations:
- getTickers
generated: '2026-07-20'
method: generated
source: openapi/maverick-protocol-openapi.yaml
---

# Maverick V2 Market Tickers

Fetch the latest 24-hour market statistics (last price, base/target volume, USD
liquidity) for all Maverick V2 markets on a chain. The API is public and needs
no authentication.

## Steps

1. Choose the target chain and its EVM `chainId` in decimal — for example
   `1` for Ethereum mainnet or `8453` for Base.
2. Call `getTickers` — `GET https://v2-api.mav.xyz/api/latest/tickers?chainId={chainId}`.
   `chainId` is mandatory; omitting it returns no useful data.
3. Parse the JSON array. Each element is a `Ticker` with:
   `ticker_id`, `base_currency`, `target_currency`, `last_price`,
   `base_volume`, `target_volume`, `pool_id`, and `liquidity_in_usd`.
4. `base_currency` and `target_currency` are ERC-20 contract addresses; resolve
   them to symbols/decimals off-chain if you need human-readable pairs. Numeric
   fields are returned as strings — parse with a decimal-safe library, not a
   float, to avoid precision loss.
5. Filter or rank markets by `liquidity_in_usd` or `base_volume` as needed.

## Notes

- Read-only market data; no order placement or on-chain actions here. Trading
  and liquidity actions happen through the Maverick V2 smart contracts (see the
  contract references in the docs), not this API.
- Data reflects a trailing 24-hour window and reflects only the requested chain;
  call once per chain you care about.
