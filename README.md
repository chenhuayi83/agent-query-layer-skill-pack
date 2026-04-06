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
- `references/partner-kit.md`

Current phase-1 scope stays inside DEX-first market-read workflows on `bsc` and `solana`.
For supported service names and routing guidance, read `references/capabilities.md` after install.

## Test Environment

This test deployment is currently available for validation:

- Test gateway base URL: `http://43.135.176.179:8080`
- Test hosted MCP URL: `http://43.135.176.179:8090`
- Published hosted MCP URL: `https://mcp.chainrpc.io`
- Published BSC JSON-RPC URL: `https://gateway.chainrpc.io/rpc/bsc`
- Published Solana JSON-RPC URL: `https://gateway.chainrpc.io/rpc/solana`

Use the test URLs when validating the latest test-node deployment.
Use the published hosted URLs above when validating the default hosted install flow.
