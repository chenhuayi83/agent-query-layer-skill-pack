# Quickstart

Use this file when you want to validate the skill against the active test deployment without reverse-engineering the MCP flow first.

## Current Endpoints

### Test Deployment

- Gateway base URL: `http://43.135.176.179:8080`
- Hosted MCP URL: `http://43.135.176.179:8090`

### Published Hosted Endpoints

- Hosted MCP URL: `https://mcp.chainrpc.io`
- BSC JSON-RPC URL: `https://gateway.chainrpc.io/rpc/bsc`
- Solana JSON-RPC URL: `https://gateway.chainrpc.io/rpc/solana`

## Prerequisite

Export the issued key before running any validation:

```bash
export AQL_WORKSPACE_API_KEY="<workspace_api_key>"
```

## Fast Health Check

```bash
curl http://43.135.176.179:8080/
curl http://43.135.176.179:8090/
```

Expected:

- gateway root returns `404 Not Found`
- MCP root returns `405 Method Not Allowed`

## MCP Initialize

```bash
cat >/tmp/aql-mcp-init.json <<'JSON'
{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-11-25","capabilities":{},"clientInfo":{"name":"skill-quickstart","version":"1.0.0"}}}
JSON

curl -sS -D /tmp/aql-mcp-init.headers \
  -o /tmp/aql-mcp-init.body \
  -X POST http://43.135.176.179:8090 \
  -H "Authorization: Bearer $AQL_WORKSPACE_API_KEY" \
  -H "Content-Type: application/json" \
  --data-binary @/tmp/aql-mcp-init.json

cat /tmp/aql-mcp-init.body
```

## MCP Tools List

```bash
SESSION_ID=$(perl -ne 'print "$1\n" if /^Mcp-Session-Id: (.*)\r$/' /tmp/aql-mcp-init.headers | head -n 1)

cat >/tmp/aql-mcp-tools.json <<'JSON'
{"jsonrpc":"2.0","id":2,"method":"tools/list","params":{}}
JSON

curl -sS \
  -X POST http://43.135.176.179:8090 \
  -H "Authorization: Bearer $AQL_WORKSPACE_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Mcp-Session-Id: $SESSION_ID" \
  --data-binary @/tmp/aql-mcp-tools.json
```

You should see friendly market tools such as:

- `market.get_token_price`
- `market.get_token_ohlcv`
- `market.get_token_profile`
- `market.get_token_liquidity`
- `market.get_pool`
- `market.get_asset_profile`
- `market.get_market_profile`
- `market.get_price_snapshot`
- `market.get_ohlcv_window`

## MCP Price Check

```bash
cat >/tmp/aql-mcp-price.json <<'JSON'
{"jsonrpc":"2.0","id":3,"method":"tools/call","params":{"name":"market.get_token_price","arguments":{"network":"bsc","address":"0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c"}}}
JSON

curl -sS \
  -X POST http://43.135.176.179:8090 \
  -H "Authorization: Bearer $AQL_WORKSPACE_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Mcp-Session-Id: $SESSION_ID" \
  --data-binary @/tmp/aql-mcp-price.json
```

## Gateway JSON-RPC Check

```bash
cat >/tmp/aql-rpc-bsc.json <<'JSON'
{"jsonrpc":"2.0","id":11,"method":"eth_blockNumber","params":[]}
JSON

curl -sS \
  -X POST http://43.135.176.179:8080/rpc/bsc \
  -H "Authorization: Bearer $AQL_WORKSPACE_API_KEY" \
  -H "Content-Type: application/json" \
  --data-binary @/tmp/aql-rpc-bsc.json
```

## How To Use These Checks

- Use the test deployment when validating the latest node rollout.
- Use the published hosted endpoints when validating the default hosted install path.
- If a live call fails, report the actual transport or auth failure instead of answering from memory.
