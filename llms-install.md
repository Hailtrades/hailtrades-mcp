# Installing Hailtrades (for AI agents such as Cline)

Hailtrades is a hosted remote MCP server. Nothing to clone, build or install; no API key.

Add to your MCP settings (e.g. `cline_mcp_settings.json`):

```json
{
  "mcpServers": {
    "hailtrades": {
      "type": "streamableHttp",
      "url": "https://hailtrades.com/mcp",
      "disabled": false
    }
  }
}
```

Tools: `find_help` (need, location, when: start here), `search_providers` (trade, location|postal_code, emergency, open_now, service, max_callout_fee, limit), `get_provider` (id), `report_contact` (optional outcome report). Try: find_help {"need":"burst pipe in the basement","location":"North York"}.
