---
name: agent-query-layer-skill-pack
description: Use when the user needs Agent Query Layer market-read data, hosted MCP workflows, gateway JSON-RPC compatibility, or help routing token price, OHLCV, liquidity, token profile, and pool requests across the current `bsc` and `solana` DEX-first surface.
---

# Agent Query Layer Skill Pack

Use this skill when you need Agent Query Layer market-read or session workflows through the hosted product surfaces.

This skill is a host-side wrapper and onboarding layer. It is not the canonical product contract.
It should still teach the host enough supported capability vocabulary to route common requests accurately.

For MCP protocol operations and runtime behavior, use the MCP server docs as the source of truth.
For the current service catalog and request routing guidance, use `references/capabilities.md`.

## When To Use

Trigger this skill when the request involves any of the following:

- latest token price on `bsc` or `solana`
- token OHLCV, candles, or chart windows
- token profile or token metadata lookup
- token liquidity or pool metadata
- canonical price, asset, or market reads
- hosted MCP setup or validation
- gateway `BSC` or `Solana` JSON-RPC validation
- "what can this skill do?" or "which tool should I use?"

## Live Data Rule

Do not answer current market-read questions from memory.

If the request depends on live price, liquidity, OHLCV, or current pool state:

1. use MCP or gateway calls
2. report the actual returned result
3. surface unsupported or degraded-source states clearly

Do not invent trending, gainers, new listings, or unsupported CEX answers.

## Host Setup

- Create an API key in portal `Dashboard Keys` before using hosted MCP or gateway access.
- Expose that key through the host environment variable referenced by your skill config, for example `AQL_API_KEY`.
- Configure the published hosted MCP endpoint at `https://mcp.chainrpc.io`.
- Authenticate hosted traffic with `Authorization: Bearer <api_key>`.
- Keep `mcp_server_url` distinct from `gateway_base_url`.
- Treat `workspace_id` as a diagnostic label, not as hosted auth.

## Test Deployment

- Current test gateway URL: `http://43.135.176.179:8080`
- Current test hosted MCP URL: `http://43.135.176.179:8090`
- Use the same bearer-auth contract when validating the test node.
- Treat the test-node URLs as validation endpoints, not as the default published hosted URLs.

## Quick Route

- Latest token price: prefer `market.get_token_price`, or `market.read_price` when the caller wants canonical contract output
- OHLCV or candles: prefer `market.get_token_ohlcv`, or `market.read_ohlcv` for canonical output
- Token metadata: prefer `market.get_token_profile`
- Token liquidity: prefer `market.get_token_liquidity`
- Pool or pair metadata: prefer `market.get_pool`
- Canonical object lookup: prefer `market.resolve_asset` or `market.resolve_market`

## Wrapper Boundary

- Use this skill to steer the host toward the product MCP surface for session workflows and toward gateway-owned JSON-RPC routes for raw chain compatibility.
- Do not treat this skill as the place that defines MCP operations or event semantics.
- Do not connect directly to upstream providers from this skill.

## Supported Market Services

- `market.get_token_price`
- `market.get_token_ohlcv`
- `market.get_token_profile`
- `market.get_token_liquidity`
- `market.get_pool`
- `market.read_price`
- `market.read_ohlcv`
- `market.resolve_asset`
- `market.resolve_market`

Current boundary:

- Phase 1 is DEX-first for the current backed market services.
- Supported DEX networks are `bsc` and `solana`.
- CEX requests may still return explicit unsupported errors until backing lands.
- Discovery-heavy requests such as trending, gainers, or new listings are not part of the current skill-pack promise.

## Code And Query Expectations

- Prefer hosted MCP tools first for supported market-read requests.
- Use bearer auth and issued product endpoints in all generated examples.
- When writing sample code, show concrete `network` plus `address` inputs for DEX requests.
- Do not invent support for trending, discovery, portfolio, or execution features that are not present in the current service catalog.

## References

- [Capabilities](references/capabilities.md)
- [Partner Kit](references/partner-kit.md)
- [Quickstart](references/quickstart.md)
- Repository MCP server docs: `apps/mcp-server/README.md`
