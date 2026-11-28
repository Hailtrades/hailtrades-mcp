<p align="center"><a href="https://hailtrades.com"><img src="assets/logo-256.png" alt="Hailtrades" width="128" height="128"></a></p>

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
- Chen Media MCP directory: [![Listed on chen.media/mcp](https://chen.media/mcp/badge/com-hailtrades-mcp.svg)](https://chen.media/mcp/com-hailtrades-mcp)
- Find us on the [Chen Media MCP Directory](https://chen.media/mcp)
- Claude Code Agents directory: https://www.claudecodeagents.com/mcp/hailtrades

## Tools

| Tool | Type | What it does |
|---|---|---|
| `find_help` | read-only | **Start here.** One call from a plain request (`need`: "my toilet is overflowing", `location`: "Mimico" / "Yonge and Eglinton" / "M6P 2T1", optional `when`: "tonight", "Sunday") to the 3 best providers: call-now phone, open/closed in Toronto time with closing time, a one-line reason, only the fees that are published, licence and third-party rating with source/date, `ref` + `suggested_message`, plus a ready-to-say `summary`. Understands synonyms, French, typos, intersections, lat/lng and postal codes in any spacing. Unsupported trades (HVAC, roofing) and areas outside the GTA return an honest `status` with guidance. |
| `search_providers` | read-only | Filterable search: `trade` (plumber/electrician), `location` or `postal_code`, `emergency`, `open_now`, `when`, `service`, `max_callout_fee`, `sort` (cheapest by known fee), `limit`. Same decision fields on every result plus the full record. |
| `get_provider` | read-only | Full details (weekly hours, all channels, sources) for one provider id from a search. |
| `report_contact` | write (optional) | Record the outcome of a user's contact attempt so availability stays accurate. Only when the user actually contacted the provider. |

Null/unknown fields mean *unknown*, not "no". Results carry readable text in `content` and the same data in `structuredContent` (with `outputSchema`).

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

## Works with Meta Muse
Meta Muse has connected to this server through its Custom Connector flow and called `search_providers` successfully (`find_help` has since been added). A typical question needs one `find_help` call. To try it, ask Muse: "Build a custom connector for the public MCP server at https://hailtrades.com/mcp (streamable HTTP, no auth)". Hailtrades is an independent project and is not affiliated with or endorsed by Meta.

## Example calls

MCP (JSON-RPC over HTTP):
```bash
curl -s https://hailtrades.com/mcp \
  -H 'content-type: application/json' -H 'accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"find_help","arguments":{"need":"my toilet is overflowing","location":"Mimico","when":"tonight"}}}'
```

REST:
```bash
curl 'https://hailtrades.com/api/v1/help?need=no%20power%20in%20half%20the%20house&location=Scarborough'
curl 'https://hailtrades.com/api/v1/providers/search?trade=electrician&postal_code=M5V&emergency=true&limit=3'
```

Example prompts:
- "My basement pipe burst. Find an emergency plumber in North York open now."
- "Which 24/7 electricians serve Etobicoke? Show licence numbers."
- "Plumbers in Scarborough with a call-out fee under $150."

## Data and attribution
Data under CC BY 4.0. Cite as "Hailtrades (https://hailtrades.com)". Provider data comes from public sources with per-field provenance; see https://hailtrades.com/methodology.

## Support
support@hailtrades.com

## Operator
Buzz Research Inc. · founder Malcolm Wylie · https://hailtrades.com
