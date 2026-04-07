# Supported Capabilities

This reference tells the agent how to use the current gateway-owned phase-1 surfaces exposed through the installed skill.

## Current Networks And Boundary

- Current backed DEX networks: `bsc`, `solana`
- Primary external surfaces: gateway `REST` market reads, gateway `WebSocket` subscriptions, and hosted `BSC` plus `Solana` `JSON-RPC` compatibility
- Current backed service families: canonical market-read object lookups, live price or short-range OHLCV subscriptions, and raw chain compatibility
- Current backed CEX slice: symbol-first `asset_profile`, `market_profile`, `price_snapshot`, and `ohlcv_window` through the canonical market-read contract
- A private MCP companion may still be provisioned separately, but it is not the default external path.
- Do not promise trending, gainers, new listings, orderbooks, execution, balances, or portfolio workflows from this skill.

## Gateway REST Routes

| Route | Use when | What the agent should do |
|---|---|---|
| `GET /v1/market/asset-profile` | the user asks what an asset is | send DEX or CEX identity inputs and report canonical `asset_profile`, `source_status`, and freshness |
| `GET /v1/market/market-profile` | the user asks for market or pair metadata | send DEX or CEX identity inputs and report canonical `market_profile`, `source_status`, and freshness |
| `GET /v1/market/price-snapshot` | the user asks for the latest price | send DEX or CEX identity inputs and report canonical `price_snapshot`, `source_status`, and freshness |
| `GET /v1/market/ohlcv-window` | the user asks for candles or a short price window | send DEX or CEX identity inputs plus `interval`, `aggregate`, and `limit` when needed |

## Gateway WebSocket Route

| Route | Use when | What the agent should do |
|---|---|---|
| `/v1/market/ws` | the user asks for live price or short-range OHLCV updates | open a bearer-auth websocket, send a `subscribe` frame with one or more targets, then consume `SESSION_READY` plus subsequent live events |

Current realtime note:

- Gateway websocket delivery should still be treated as DEX-first for target support unless later contract docs say otherwise.

## Gateway JSON-RPC Routes

| Route | Use when | What the agent should do |
|---|---|---|
| `/rpc/bsc` | the user needs raw BSC chain compatibility | send valid `JSON-RPC 2.0` requests with bearer auth at the transport layer |
| `/rpc/solana` | the user needs raw Solana chain compatibility | send valid `JSON-RPC 2.0` requests with bearer auth at the transport layer |

## Examples

- Latest token price on BSC: call gateway `GET /v1/market/price-snapshot?market_kind=dex&chain=bsc&address=...`.
- Solana token candles: call gateway `GET /v1/market/ohlcv-window?market_kind=dex&chain=solana&address=...&interval=1m&limit=...`.
- Canonical CEX market profile read: call gateway `GET /v1/market/market-profile?market_kind=cex&symbol=BTCUSDT`.
- Live DEX price stream: open gateway websocket `/v1/market/ws` and send a `subscribe` frame for one or more DEX targets.
- Raw BSC chain compatibility: `POST` valid `JSON-RPC 2.0` to `/rpc/bsc`.

## Response Guidance

- Prefer concise summaries plus the most relevant fields instead of dumping raw objects.
- Keep `source_id`, freshness, and explicit market-support boundaries visible when they matter to the answer.
- When code is requested, generate examples that call issued gateway surfaces rather than inventing direct upstream calls.
