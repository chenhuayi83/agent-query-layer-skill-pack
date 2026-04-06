---
name: agent-query-layer-skill-pack
description: Use Agent-Query-Layer through the hosted MCP and gateway JSON-RPC surfaces.
---

# Agent Query Layer Skill Pack

Use this skill when you need Agent-Query-Layer market-read or session workflows through the hosted product surfaces.

This skill is a host-side wrapper and onboarding layer. It is not the capability contract.
It should still teach the host enough supported capability vocabulary to route common requests accurately.

For MCP protocol operations, tool semantics, and runtime behavior, use the MCP server docs as the source of truth.
For the currently backed service catalog and request routing guidance, use `references/capabilities.md`.

## Host Setup

- Create an API key in portal `Dashboard Keys` before using hosted MCP or gateway access.
- Expose that key through the host environment variable referenced by your skill config, for example `AQL_API_KEY`.
- Configure the hosted MCP endpoint at `https://mcp.chainrpc.io`.
- Authenticate hosted traffic with `Authorization: Bearer <api_key>`.
- Keep `mcp_server_url` distinct from `gateway_base_url`.
- Treat `workspace_id` as a diagnostic label, not as hosted auth.

## Wrapper Boundary

- Use this skill to steer the host toward the product MCP surface for session workflows and toward gateway-owned JSON-RPC routes for raw chain compatibility.
- Do not treat this skill as the place that defines MCP operations or event semantics.
- Do not connect directly to upstream providers from this skill.

## Supported Market Services

- `market.get_token_price`: direct token price lookup for one network and token address.
- `market.get_token_ohlcv`: direct token OHLCV lookup for one network and token address.
- `market.get_token_profile`: direct token metadata lookup for one network and token address.
- `market.get_token_liquidity`: direct token liquidity lookup for one network and token address.
- `market.get_pool`: direct pool metadata lookup for one network and pool address.
- `market.read_price`: friendlier canonical price snapshot lookup when the caller asks for the latest price.
- `market.read_ohlcv`: friendlier canonical OHLCV lookup when the caller asks for chart candles or a short-range price window.
- `market.resolve_asset` and `market.resolve_market`: canonical object lookup tools when the caller needs structured contract objects instead of service-shaped results.

Current boundary:

- Phase 1 is DEX-first for the current backed market services.
- Supported DEX networks are `bsc` and `solana`.
- CEX requests may still return explicit unsupported errors until backing lands.
- Discovery-heavy requests such as trending, gainers, or new listings are not part of the current skill-pack promise.

## Request Routing Guide

- If the user asks for `the latest token price`, prefer `market.get_token_price` or `market.read_price`.
- If the user asks for `candles`, `ohlcv`, `chart`, or `price window`, prefer `market.get_token_ohlcv` or `market.read_ohlcv`.
- If the user asks for `token metadata`, `symbol`, `name`, or `profile`, prefer `market.get_token_profile`.
- If the user asks for `liquidity`, prefer `market.get_token_liquidity` for token-level liquidity context.
- If the user asks for `pool details`, `pair metadata`, or `pool composition`, prefer `market.get_pool`.
- If the user asks for a contract-shaped object shared with REST or gateway semantics, prefer the canonical `market.resolve_*` tools.

## Code And Query Expectations

- Prefer hosted MCP tools first for supported market-read requests.
- Use bearer auth and issued product endpoints in all generated examples.
- When writing sample code, show concrete `network` plus `address` inputs for DEX requests.
- Do not invent support for trending, discovery, portfolio, or execution features that are not present in the current service catalog.

## References

- [Capabilities](references/capabilities.md)
- [Partner Kit](references/partner-kit.md)
- Repository MCP server docs: `apps/mcp-server/README.md`
