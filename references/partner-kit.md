# Partner Kit

This artifact is packaged for one pilot account and preserves the hosted phase-1 contract.

## Portal Onboarding

1. Complete portal registration or pilot activation for the target account.
2. Open portal `Dashboard Keys` and create a `workspace_api_key`.
3. Store the issued key in the host environment variable used by your skill config, for example `AQL_WORKSPACE_API_KEY`.

## Hosted Access

- Gateway base URL: `https://gateway.chainrpc.io`
- Gateway WebSocket endpoint: `wss://gateway.chainrpc.io/v1/market/ws`
- Gateway REST asset profile route: `https://gateway.chainrpc.io/v1/market/asset-profile`
- Gateway REST market profile route: `https://gateway.chainrpc.io/v1/market/market-profile`
- Gateway REST price snapshot route: `https://gateway.chainrpc.io/v1/market/price-snapshot`
- Gateway REST OHLCV window route: `https://gateway.chainrpc.io/v1/market/ohlcv-window`
- BSC JSON-RPC endpoint: `https://gateway.chainrpc.io/rpc/bsc`
- Solana JSON-RPC endpoint: `https://gateway.chainrpc.io/rpc/solana`
- Optional private MCP endpoint for separately issued partner connectors: `https://mcp.chainrpc.io`
- Auth header: `Authorization: Bearer <workspace_api_key>`
- Gateway API inventory docs: repository `docs/standards/agent-api-inventory.md`
- Capability summary for the installed skill: `references/capabilities.md`
- Quick validation guide: `references/quickstart.md`

## Test Deployment Endpoints

- Test gateway URL: `http://43.135.176.179:8080`
- Test gateway WebSocket URL: `ws://43.135.176.179:8080/v1/market/ws`
- Optional private test MCP URL when a partner connector needs it: `http://43.135.176.179:8090`
- Current test-node repository head validated in operations docs: `ab80755`
- The test node is intended for smoke checks of gateway access, install flow, and bearer-auth behavior.

## Integration Rules

- Route request-response market reads through gateway `REST`.
- Route live market subscriptions through gateway `WebSocket`.
- Route raw chain compatibility through gateway `JSON-RPC`.
- Keep websocket target promises inside the current DEX-first realtime scope unless later contract docs widen that boundary.
- If a partner-specific private MCP companion is issued, treat it as optional and non-default.
- Do not treat `workspace_id` as the hosted source of truth for identity.
- Do not add host-local retries that mask gateway continuity or source-state events.
- Route all hosted traffic through product-owned surfaces only.
