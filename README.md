# Agent Query Layer Skill Pack

An installable skill package that gives AI agents built-in knowledge of the current Agent Query Layer market-read surface.

Instead of re-explaining the product contract every time, this skill teaches the agent:

- which MCP tools exist right now
- which requests belong on MCP vs gateway JSON-RPC
- how to stay inside the current DEX-first phase-1 boundary
- how to validate against the active test deployment

## Installation

### Via skills.sh

```bash
npx skills add chenhuayi83/agent-query-layer-skill-pack -g -y
```

### Via GitHub

```bash
git clone https://github.com/chenhuayi83/agent-query-layer-skill-pack.git
mv agent-query-layer-skill-pack ~/.agents/skills/agent-query-layer-skill-pack
```

The exact install path may vary by agent host.

## What's Inside

```text
SKILL.md
agents/
  openai.yaml
references/
  capabilities.md
  partner-kit.md
  quickstart.md
```

- `SKILL.md`: main entry point for routing, boundary rules, and live-data behavior
- `references/capabilities.md`: current tool catalog and request mapping
- `references/partner-kit.md`: hosted access endpoints and integration rules
- `references/quickstart.md`: copy-ready smoke checks for the current test deployment

## Try It Out

Once installed, try prompts like:

- "What is the latest price for WBNB on BSC?"
- "Give me 1 minute candles for a Solana token."
- "Show the liquidity for this BSC token address."
- "Resolve the canonical market object for this pool."
- "Use the test deployment and verify the MCP tools available right now."

## Current Boundary

- Phase 1 is still DEX-first
- Supported DEX networks are `bsc` and `solana`
- Supported service families are price, OHLCV, token profile, liquidity, pool metadata, and canonical market-read lookups
- Do not promise trending, gainers, new listings, balances, portfolio, or execution features

## Test Environment

This test deployment is currently available for validation:

- Test gateway base URL: `http://43.135.176.179:8080`
- Test hosted MCP URL: `http://43.135.176.179:8090`
- Published hosted MCP URL: `https://mcp.chainrpc.io`
- Published BSC JSON-RPC URL: `https://gateway.chainrpc.io/rpc/bsc`
- Published Solana JSON-RPC URL: `https://gateway.chainrpc.io/rpc/solana`

Use the test URLs when validating the latest test-node deployment.
Use the published hosted URLs above when validating the default hosted install flow.

## Validation

Start with:

```bash
curl http://43.135.176.179:8080/
curl http://43.135.176.179:8090/
```

Expected behavior:

- gateway root returns `404 Not Found`
- MCP root returns `405 Method Not Allowed`

For full MCP and RPC smoke checks, see [references/quickstart.md](references/quickstart.md).

## Feedback

If the skill routes the wrong tool, misses a supported workflow, or makes the boundary unclear, open an issue in this repository.
