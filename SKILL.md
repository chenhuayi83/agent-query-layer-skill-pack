---
name: agent-query-layer-skill-pack
description: Use Agent-Query-Layer through gateway REST, WebSocket, and hosted JSON-RPC surfaces.
---

# Agent Query Layer Skill Pack

Use this skill when you need Agent-Query-Layer market-read or realtime workflows through gateway-owned hosted product surfaces.

This skill is a host-side wrapper and onboarding layer. It is not the capability contract.
It should still teach the host enough supported capability vocabulary to route common requests accurately.

For the currently backed service catalog and request routing guidance, use `references/capabilities.md`.

## Host Setup

- Create a workspace API key in portal `Dashboard Keys` before using hosted gateway access.
- Expose that key through the host environment variable referenced by your skill config, for example `AQL_WORKSPACE_API_KEY`.
- Configure the hosted gateway base URL at `https://gateway.chainrpc.io`.
- Configure gateway WebSocket subscriptions at `wss://gateway.chainrpc.io/v1/market/ws`.
- Authenticate hosted traffic with `Authorization: Bearer <workspace_api_key>`.
- Use gateway `REST` for request-response market reads, gateway `WebSocket` for live subscriptions, and gateway `JSON-RPC` for raw chain compatibility.
- If your workspace setup explicitly includes a private MCP companion at `https://mcp.chainrpc.io`, keep it separate and treat it as optional.
- Treat `workspace_id` as a diagnostic label, not as hosted auth.

## Test Deployment

- Current test gateway URL: `http://43.135.176.179:8080`.
- Current test gateway WebSocket URL: `ws://43.135.176.179:8080/v1/market/ws`.
- Use the same bearer-auth contract when validating the test node.
- Treat the test-node URLs as validation endpoints, not as the default published hosted URLs.

## Wrapper Boundary

- Use this skill to steer the host toward gateway-owned `REST` routes for canonical market reads.
- Use this skill to steer the host toward gateway-owned `WebSocket` subscriptions for live market events.
- Use this skill to steer the host toward gateway-owned `JSON-RPC` routes for raw chain compatibility.
- Do not treat a private MCP companion as the default product path after install.
- Do not connect directly to upstream providers from this skill.

## Supported Market Services

- Gateway `REST` asset identity reads through `https://gateway.chainrpc.io/v1/market/asset-profile`.
- Gateway `REST` market identity reads through `https://gateway.chainrpc.io/v1/market/market-profile`.
- Gateway `REST` latest price reads through `https://gateway.chainrpc.io/v1/market/price-snapshot`.
- Gateway `REST` short-range OHLCV reads through `https://gateway.chainrpc.io/v1/market/ohlcv-window`.
- Gateway `WebSocket` realtime delivery through `wss://gateway.chainrpc.io/v1/market/ws` for supported live event targets.
- Gateway raw `BSC` and `Solana` compatibility through `https://gateway.chainrpc.io/rpc/bsc` and `https://gateway.chainrpc.io/rpc/solana`.

Current boundary:

- Phase 1 keeps DEX token and pool reads as the richer current path.
- Supported DEX networks are `bsc` and `solana`.
- Gateway `REST` now also exposes live minimal symbol-first CEX market-read support through canonical `asset_profile`, `market_profile`, `price_snapshot`, and `ohlcv_window` queries.
- Gateway `WebSocket` is for live market delivery, but current realtime target support should still be treated as DEX-first unless later contract docs say otherwise.
- Discovery-heavy requests such as trending, gainers, or new listings are not part of the current skill-pack promise.

## Request Routing Guide

- If the user asks for `the latest token price`, prefer gateway `GET /v1/market/price-snapshot`.
- If the user asks for `candles`, `ohlcv`, `chart`, or `price window`, prefer gateway `GET /v1/market/ohlcv-window`.
- If the user asks for `token metadata`, `symbol`, `name`, or `profile`, prefer gateway `GET /v1/market/asset-profile` or `GET /v1/market/market-profile`.
- If the user asks for live price or short-range OHLCV updates, prefer gateway `WebSocket` subscribe flow on `/v1/market/ws`.
- If the user asks for raw chain calls, use gateway `POST /rpc/bsc` or `POST /rpc/solana`.
- If a separately provisioned private MCP companion is explicitly available, use it only for that companion workflow rather than as the standard hosted path.

## Code And Query Expectations

- Prefer gateway `REST` for request-response market reads.
- Prefer gateway `WebSocket` for live market subscriptions.
- Use gateway `JSON-RPC` only when the caller needs raw chain compatibility semantics.
- Use bearer auth and issued product endpoints in all generated examples.
- When writing sample code, show concrete `network` plus `address` inputs for DEX requests.
- For CEX examples, use concrete `symbol` plus `market` inputs such as `BTCUSDT` and `spot`.
- Do not invent support for trending, discovery, portfolio, or execution features that are not present in the current service catalog.

## References

- [Capabilities](references/capabilities.md)
- [Hosted Access](references/hosted-access.md)
- [Quickstart](references/quickstart.md)
- Repository gateway API inventory: `docs/standards/agent-api-inventory.md`
