# Hailtrades MCP server

Find **emergency residential plumbers and electricians in Toronto and the GTA** from any AI agent or MCP client. About 7,000 providers with hours, 24/7 / emergency / weekend flags, call-out and free-quote fees, licence numbers, service areas, preferred contact method and a last-verified date.

- Remote MCP endpoint (Streamable HTTP, stateless, **no auth, no API key**): `https://hailtrades.com/mcp`
- Website: https://hailtrades.com
- Agent docs: https://hailtrades.com/for-agents
- JSON API + OpenAPI 3.1: https://hailtrades.com/openapi.json
- `llms.txt`: https://hailtrades.com/llms.txt
- Open data (CSV/JSON): https://hailtrades.com/data, licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) (attribution + link to https://hailtrades.com required)

This repository holds connection snippets and `server.json` for registries. The server itself is hosted at hailtrades.com; there is nothing to install.

## Listings
- Official MCP Registry: `com.hailtrades/mcp` ([entry](https://registry.modelcontextprotocol.io/v0/servers?search=com.hailtrades))
- Glama: [![Hailtrades MCP connector](https://glama.ai/mcp/connectors/com.hailtrades/mcp/badges/score.svg)](https://glama.ai/mcp/connectors/com.hailtrades/mcp)

## Tools

| Tool | Type | What it does |
|---|---|---|
| `search_providers` | read-only | Search by `trade` (plumber/electrician), `location` or `postal_code`, `emergency`, `open_now`, `service`, `max_callout_fee`, `limit`. |
| `get_provider` | read-only | Full details for one provider id from search. |
| `report_contact` | write (optional) | Record the outcome of a user's contact attempt so availability stays accurate. Only when the user actually contacted the provider. |

Null/unknown fields mean *unknown*, not "no".

## Connect

### Claude Desktop / claude.ai (custom connector)
Settings → Connectors → Add custom connector → URL `https://hailtrades.com/mcp` (no authentication).

### Claude Code
```bash
claude mcp add --transport http hailtrades https://hailtrades.com/mcp
```

### Cursor (`~/.cursor/mcp.json` or `.cursor/mcp.json`)
```json
{ "mcpServers": { "hailtrades": { "url": "https://hailtrades.com/mcp" } } }
```

### VS Code (`.vscode/mcp.json`)
```json
{ "servers": { "hailtrades": { "type": "http", "url": "https://hailtrades.com/mcp" } } }
```

### ChatGPT (developer mode)
Settings → Connectors → Advanced → enable Developer mode → Create → MCP server URL `https://hailtrades.com/mcp`, authentication: none.

### Muse Code (`~/.config/muse/settings.json`)
```json
{
  "schema_version": 1,
  "mcpServers": {
    "hailtrades": { "type": "streamable-http", "url": "https://hailtrades.com/mcp" }
  }
}
```
Consumer Muse: ask it to "Build a custom connector for the public MCP server at https://hailtrades.com/mcp (streamable HTTP, no auth)".

### Any client via `mcp-remote` (stdio bridge)
```json
{ "mcpServers": { "hailtrades": { "command": "npx", "args": ["-y", "mcp-remote", "https://hailtrades.com/mcp"] } } }
```

## Example calls

MCP (JSON-RPC over HTTP):
```bash
curl -s https://hailtrades.com/mcp \
  -H 'content-type: application/json' -H 'accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"search_providers","arguments":{"trade":"plumber","location":"North York","emergency":true,"limit":3}}}'
```

REST:
```bash
curl 'https://hailtrades.com/api/v1/providers/search?trade=electrician&postal_code=M5V&emergency=true&limit=3'
```

Example prompts:
- "My basement pipe burst. Find an emergency plumber in North York open now."
- "Which 24/7 electricians serve Etobicoke? Show licence numbers."
- "Plumbers in Scarborough with a call-out fee under $150."

## Data and attribution
Data under CC BY 4.0. Cite as "Hailtrades (https://hailtrades.com)". Provider data comes from public sources with per-field provenance; see https://hailtrades.com/methodology.

## Operator
Buzz Research Inc. · founder Malcolm Wylie · https://hailtrades.com
