# Agent Query Layer Skill Pack

Install globally with one command:

```bash
npx skills add chenhuayi83/agent-query-layer-skill-pack -g -y
```

This repository publishes the installable skill bundle for Agent Query Layer.

Published files:

- `SKILL.md`
- `agents/openai.yaml`
- `references/capabilities.md`
- `references/hosted-access.md`
- `references/quickstart.md`

Current phase-1 scope stays inside DEX market-read workflows on `bsc` and `solana`, plus the live minimal symbol-first CEX query slice.
Hosted auth uses `Authorization: Bearer <workspace_api_key>` and should load the issued key from an environment variable such as `AQL_WORKSPACE_API_KEY`.
The installed skill should steer the agent toward the current hosted product surfaces: gateway `REST`, gateway `WebSocket`, and hosted `JSON-RPC`.
A private MCP companion may still exist as separately provisioned companion wiring, but it remains optional alongside the standard hosted surfaces.
For supported service names and routing guidance, read `references/capabilities.md` after install.
For copy-ready smoke checks against the active hosted surfaces, read `references/quickstart.md`.

## Test Environment

This test deployment is currently available for validation:

- Test gateway base URL: `http://43.135.176.179:8080`
- Test gateway WebSocket URL: `ws://43.135.176.179:8080/v1/market/ws`
- Published gateway base URL: `https://gateway.chainrpc.io`
- Published gateway WebSocket URL: `wss://gateway.chainrpc.io/v1/market/ws`
- Published BSC JSON-RPC URL: `https://gateway.chainrpc.io/rpc/bsc`
- Published Solana JSON-RPC URL: `https://gateway.chainrpc.io/rpc/solana`
- Optional private MCP URL when separately provisioned for your workspace: `https://mcp.chainrpc.io`

Use the test URLs when validating the latest test-node deployment.
Use the published hosted URLs above when validating the default hosted install flow.
