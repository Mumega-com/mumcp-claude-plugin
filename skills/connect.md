# Connect to WordPress

Set up a mumcp connection to a WordPress site. $ARGUMENTS = site URL and API key, or "setup" for guided flow.

## Guided Setup

If no arguments provided, walk the user through:

1. **Install mumcp on WordPress:**
   ```bash
   wp plugin install https://mumega.com/mcp-updates/mumega-mcp-latest.zip --activate
   ```
   Or download from https://mucp.mumega.com and install via WP Admin > Plugins > Add New > Upload.

2. **Generate API key:**
   In WP Admin > mumcp > Settings, click "Generate API Key". Copy the key (starts with `spai_`).

3. **Add MCP config:**
   Add this to your Claude Code settings (or project `.mcp.json`):
   ```json
   {
     "mcpServers": {
       "mumcp": {
         "url": "https://YOUR-SITE.com/wp-json/site-pilot-ai/v1/mcp",
         "headers": {"X-API-Key": "spai_YOUR_KEY"}
       }
     }
   }
   ```

4. **Test connection:**
   Call `wp_introspect()` — should return site info, tools, and capabilities.

## Quick Connect (with arguments)

If user provides URL and key:
```
/mumcp:connect https://example.com spai_abc123...
```

Generate the MCP config JSON and tell them where to save it:
- Global: `~/.claude/settings.json` under `mcpServers`
- Project: `.mcp.json` in project root
- Claude Desktop: `~/Library/Application Support/Claude/claude_desktop_config.json` (Mac) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows)

## For Cursor / Windsurf / Other MCP Clients

Same MCP endpoint URL, just configure it in each client's MCP settings:
- **Cursor:** Settings > MCP Servers > Add
- **Windsurf:** Settings > MCP > Add Server

## Troubleshooting

- **401 Unauthorized:** Check API key is correct and starts with `spai_`
- **404 Not Found:** Plugin might not be activated. Check `https://YOUR-SITE.com/wp-json/site-pilot-ai/v1/site-info`
- **500 Error:** Check WP debug log. Common cause: PHP version < 7.4
- **CORS error:** Add your domain to mumcp > Settings > Allowed Origins
