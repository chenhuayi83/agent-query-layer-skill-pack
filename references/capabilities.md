# Supported Capabilities

This reference tells the agent what the current skill-pack can do without forcing it to reverse-engineer the MCP docs first.

## Current Networks And Boundary

- Current backed DEX networks: `bsc`, `solana`
- Current backed service families: token price, token OHLCV, token profile, token liquidity, pool metadata, canonical market-read object lookups, and session workflows
- Current backed CEX slice: symbol-first `asset_profile`, `market_profile`, `price_snapshot`, and `ohlcv_window` through the canonical market-read contract
- Do not promise trending, gainers, new listings, orderbooks, execution, balances, or portfolio workflows from this skill.

## Tool Catalog

| Tool | Use when | What the agent should do |
|---|---|---|
| `market.get_token_price` | the user asks for the latest token price | send `network` and `address`; report `price_usd`, `source_id`, and freshness when available |
| `market.get_token_ohlcv` | the user asks for OHLCV, candles, or a chart window | send `network`, `address`, and optional `timeframe` or `interval`, `aggregate`, `limit` |
| `market.get_token_profile` | the user asks what a token is | send `network` and `address`; report symbol, name, and useful metadata fields |
| `market.get_token_liquidity` | the user asks for current liquidity | send `network` and `address`; report liquidity plus source context |
| `market.get_pool` | the user asks for pool or pair metadata | send `network` and `address` or `pool_address`; report token legs, protocol, liquidity, and timestamps |
| `market.read_price` | the user wants the canonical price snapshot contract | prefer when the result should match the shared market-read contract shape |
| `market.read_ohlcv` | the user wants canonical OHLCV contract output | prefer when the result should match the shared market-read contract shape |
| `market.resolve_asset` | the user wants canonical asset identity | use for asset-level contract objects |
| `market.resolve_market` | the user wants canonical market identity | use for market-level contract objects such as pair or pool resolution |

## Examples

- Latest token price on BSC: call `market.get_token_price` with `network=bsc` and the token `address`.
- Solana token candles: call `market.get_token_ohlcv` with `network=solana`, `address`, and `timeframe=minute` or `interval=1m`.
- Pool metadata for one pair: call `market.get_pool` with `network` and `pool_address`.
- Canonical structured price read: call `market.read_price` with `chain` plus `address`.
- Canonical CEX market profile read: call `market.resolve_market` or `market.get_market_profile` with `symbol=BTCUSDT` and `market=spot`.

## Response Guidance

- Prefer concise summaries plus the most relevant fields instead of dumping raw objects.
- Keep `source_id`, freshness, and explicit market-support boundaries visible when they matter to the answer.
- When code is requested, generate examples that call the hosted MCP surface or use the installed skill host flow rather than inventing direct upstream REST calls.
