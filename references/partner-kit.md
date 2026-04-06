# Partner Kit

This artifact is packaged for one pilot account and preserves the hosted phase-1 contract.

## Portal Onboarding

1. Complete portal registration or pilot activation for the target account.
2. Open portal `Dashboard Keys` and create an API key.
3. Store the issued key in the host environment variable used by your skill config, for example `AQL_API_KEY`.

## Hosted Access

- MCP endpoint: `https://mcp.chainrpc.io`
- BSC JSON-RPC endpoint: `https://gateway.chainrpc.io/rpc/bsc`
- Solana JSON-RPC endpoint: `https://gateway.chainrpc.io/rpc/solana`
- Auth header: `Authorization: Bearer <api_key>`
- MCP capability docs: repository `apps/mcp-server/README.md`
- Capability summary for the installed skill: `references/capabilities.md`

## Test Deployment Endpoints

- Test gateway URL: `http://43.135.176.179:8080`
- Test hosted MCP URL: `http://43.135.176.179:8090`
- Current test-node repository head validated in operations docs: `ab80755`
- The test node is intended for smoke checks of install flow, tool catalog, and bearer-auth behavior.

## Integration Rules

- Keep `mcp_server_url` separate from `gateway_base_url` unless the issued partner kit explicitly collapses them.
- Do not treat `workspace_id` as the hosted source of truth for identity.
- Do not add host-local retries that mask gateway continuity or source-state events.
- Route all MCP and JSON-RPC traffic through product-owned surfaces only.
