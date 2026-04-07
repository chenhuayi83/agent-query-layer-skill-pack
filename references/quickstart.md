# Quickstart

Use this file when you want to validate the skill against the active test deployment without reverse-engineering the gateway flow first.

## Current Endpoints

### Test Deployment

- Gateway base URL: `http://43.135.176.179:8080`
- Gateway WebSocket URL: `ws://43.135.176.179:8080/v1/market/ws`

### Published Hosted Endpoints

- Gateway base URL: `https://gateway.chainrpc.io`
- Gateway WebSocket URL: `wss://gateway.chainrpc.io/v1/market/ws`
- BSC JSON-RPC URL: `https://gateway.chainrpc.io/rpc/bsc`
- Solana JSON-RPC URL: `https://gateway.chainrpc.io/rpc/solana`
- Optional private MCP URL when your workspace setup explicitly includes one: `https://mcp.chainrpc.io`

## Prerequisite

Export the issued key before running any validation:

```bash
export AQL_WORKSPACE_API_KEY="<workspace_api_key>"
```

## Fast Health Check

```bash
curl http://43.135.176.179:8080/
```

Expected:

- gateway root returns `404 Not Found`

## Gateway REST Price Snapshot

```bash
curl -sS -G "http://43.135.176.179:8080/v1/market/price-snapshot" \
  -H "Authorization: Bearer $AQL_WORKSPACE_API_KEY" \
  --data-urlencode "market_kind=dex" \
  --data-urlencode "chain=bsc" \
  --data-urlencode "address=0xbnb"
```

## Gateway REST CEX Market Profile

```bash
curl -sS -G "http://43.135.176.179:8080/v1/market/market-profile" \
  -H "Authorization: Bearer $AQL_WORKSPACE_API_KEY" \
  --data-urlencode "market_kind=cex" \
  --data-urlencode "symbol=BTCUSDT"
```

## Gateway WebSocket Subscribe

Use a websocket client such as `wscat` from the same machine where the skill is installed:

```bash
npx -y wscat -c "ws://43.135.176.179:8080/v1/market/ws" \
  -H "Authorization: Bearer $AQL_WORKSPACE_API_KEY"
```

Then send this subscribe frame:

```json
{"action":"subscribe","profile":"standard","targets":[{"market_kind":"dex","target_id":"0xbnb","chain":"bsc","event_family":"price","base_asset":"BNB","quote_asset":"USDT"}]}
```

Expected websocket behavior:

- first event is `SESSION_READY`
- later events include `MARKET_PRICE` for the subscribed DEX target

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

## Optional Private MCP Companion

- Only use a private MCP endpoint when your workspace setup explicitly includes one.
- A private MCP companion is not required for the standard hosted validation checks above.

## How To Use These Checks

- Use the test deployment when validating the latest node rollout.
- Use the published hosted endpoints when validating the default hosted install path.
- If a live call fails, report the actual transport or auth failure instead of answering from memory.
