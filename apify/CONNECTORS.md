# Connectors

This plugin depends on one MCP connector.

## `apify`: Apify MCP Server

- **Endpoint:** `https://mcp.apify.com` (Streamable HTTP)
- **Declared in:** [`.mcp.json`](./.mcp.json)
- **Auth:** OAuth. On the first tool call that needs authorization, Qoder opens a browser to sign in to your Apify account, no API token to paste.
- **Provides:** search the Apify Store, fetch Actor details, run Actors, and read Actor run datasets / key-value stores.
- **Account:** free sign-up at <https://console.apify.com/sign-up>.

> **QoderWork note:** for the public QoderWork marketplace, an MCP server referenced by a Plugin may need to resolve to an officially listed Apify **Connector**, or be declared with the `{{USER_CONFIG}}` + `_setup` form. The bare-URL OAuth declaration here is verified for the Qoder CLI/IDE; confirm the QoderWork listing path before publishing there.
