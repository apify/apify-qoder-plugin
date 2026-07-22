# Apify plugin for Qoder

Extract data from any website with thousands of trusted scrapers, crawlers, and
automations from the [Apify Store](https://apify.com/store). Run ready-made
**Actors** for social media, video, e-commerce, search engines, maps, and travel
sites — or build, debug, and publish your own — directly from Qoder.

## What's inside

| Component | Purpose |
|---|---|
| `apify` subagent (`agents/`) | Routes any Apify request to the right skill or MCP tool |
| `apify-ultimate-scraper` skill | Run existing Actors across 15+ platforms for data extraction |
| `apify-actor-development` skill | Build, debug, and deploy new Actors |
| `apify-actorization` skill | Convert an existing project into an Actor |
| `apify-generate-output-schema` skill | Generate an Actor's output schemas |
| `apify-sdk-integration` skill | Call Actors from an existing app via `apify-client` |
| `apify` MCP server (`.mcp.json`) | Actor search, run, and dataset tools over `https://mcp.apify.com` |

## Installation

Install from the QoderWork Expert Kits marketplace, or add the bundle manually:

```bash
qodercli plugins install ./apify-qoder-plugin
```

The same bundle installs in Qoder IDE/CLI and QoderWork.

## Authentication

The `apify` MCP server uses **OAuth**. On the first Actor call, Qoder opens a
browser window to authorize against your Apify account — no API token to paste.
Don't have an account? Sign up free at
[console.apify.com/sign-up](https://console.apify.com/sign-up).

## Usage

Ask naturally, for example:

- "Scrape the latest 100 posts from an Instagram profile."
- "Find an Apify Actor that scrapes Google Maps reviews and run it for this place."
- "Help me build an Actor that crawls this documentation site."

The `apify` subagent picks the right route and skill automatically.

## Links

- [Apify](https://apify.com) · [Apify Store](https://apify.com/store) · [Docs](https://docs.apify.com)
- [Apify MCP server](https://mcp.apify.com)

## Maintainers

Skills and the subagent in this repo are **generated** from the
`apify-plugins-internal` source of truth (the `qoder` platform). Do not hand-edit
`skills/` or `agents/` here — change them upstream and rebuild. The manifest
(`.qoder-plugin/plugin.json`), `.mcp.json`, and `.qoder/rules/` are maintained
directly in this repo.

## License

[Apache-2.0](./LICENSE)
