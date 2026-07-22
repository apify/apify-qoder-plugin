# Changelog

All notable changes to this project will be documented in this file.

## [0.1.0](https://github.com/daveomri/apify-qoder-plugin/releases/tag/v0.1.0) (2026-07-22)

### 🚀 Features

- Implement first version of the plugin ([aa6c005](https://github.com/daveomri/apify-qoder-plugin/commit/aa6c005b86e80b47a307f79e2c89a1b2c828b1bc)) by [@daveomri](https://github.com/daveomri)
- Add initial project structure with .gitignore, CONTRIBUTING, RELEASE, and workflow files ([04eb1df](https://github.com/daveomri/apify-qoder-plugin/commit/04eb1df0272658167849cb206cadc721d7c85101)) by [@daveomri](https://github.com/daveomri)

### 🐛 Bug Fixes

- Fix plugin.json ([b3dd7cd](https://github.com/daveomri/apify-qoder-plugin/commit/b3dd7cd86dff48553ac42f2c55fb076a97e7874a)) by [@daveomri](https://github.com/daveomri)


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
