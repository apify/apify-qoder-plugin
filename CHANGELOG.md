# Changelog

All notable changes to the **Apify for Qoder** plugin are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - Initial Qoder release

### Added
- `apify/.qoder-plugin/plugin.json` manifest with Qoder plugin metadata.
- `.qoder-plugin/marketplace.json` registry (repo root) cataloging the `./apify` plugin.
- `apify` MCP server entry in `.mcp.json` pointing to `https://mcp.apify.com` (OAuth on first use).
- Five bundled skills: `apify-ultimate-scraper`, `apify-actor-development`, `apify-actorization`, `apify-generate-output-schema`, `apify-sdk-integration`.
- `apify` subagent that routes requests to the right skill or MCP tool.
- Role instruction doc at `.qoder/rules/apify.md`.
- Apache-2.0 license.
