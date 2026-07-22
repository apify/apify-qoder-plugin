# Contributing

This repo is the distributable **Apify plugin for Qoder** (Qoder CLI/IDE + QoderWork). Read this before changing anything. Some files here are **generated** and must not be hand-edited.

## Repository layout

```
apify-qoder-plugin/
├─ .qoder-plugin/marketplace.json   # Qoder CLI marketplace registry → ./apify  (MANUAL; CLI-only)
├─ README.md · LICENSE · CHANGELOG.md
├─ assets/
│  ├─ logo.svg                      # source logo
│  └─ icon.png                      # 512×512 raster icon for the QoderWork publish form
├─ .github/workflows/release.yml    # builds the QoderWork ZIP on GitHub release
└─ apify/                           # THE PLUGIN (a flat Qoder plugin)
   ├─ .qoder-plugin/plugin.json     # manifest                         (MANUAL)
   ├─ .mcp.json                     # apify MCP server declaration     (MANUAL)
   ├─ qoder.md                      # role/project instructions        (MANUAL)
   ├─ CONNECTORS.md                 # MCP/connector dependency doc      (MANUAL)
   ├─ skills/                       # 5 Apify skills                   (GENERATED, do not edit here)
   └─ agents/apify.md               # apify subagent                   (GENERATED, do not edit here)
```

## Generated vs. manually maintained

| Files | Source of truth | Edit where |
|---|---|---|
| `apify/skills/**`, `apify/agents/**` | **`apify-plugins-internal`** (`content/skills`, `content/subagents`) | Upstream, never here |
| Everything else (manifest, `.mcp.json`, `marketplace.json`, `qoder.md`, `CONNECTORS.md`, README, LICENSE, CHANGELOG, assets, workflows) | **this repo** | Here |

> ⚠️ **Do not hand-edit `apify/skills/` or `apify/agents/`.** They are overwritten every time the content is regenerated/synced from `apify-plugins-internal`. Fix skills and the subagent upstream instead.

## Relationship to `apify-plugins-internal`

`apify-plugins-internal` is the single source of truth for the plugin's **skills and subagent**, shared across all Apify AI-coding plugins (Codex, Cursor, Copilot, … and Qoder). Its `qoder` platform (`platforms/qoder.ts` + `src/generators/qoder.ts`) emits Qoder-shaped `skills/` + `agents/`.

To change skill or subagent content:

1. Edit `content/skills/**` or `content/subagents/apify.md` in `apify-plugins-internal`.
2. `npm run build:qoder` there → produces `output/qoder/{skills,agents}`.
3. Sync into this repo's `apify/`:
   - **Automated** (once `platforms/qoder.ts` sets `repository: "apify/apify-qoder-plugin"`): a CI PR from `apify-plugins-internal` updates `apify/skills` + `apify/agents`.
   - **Manual** (until then): copy `output/qoder/{skills,agents}` into `apify/`.

## Local development & testing (Qoder CLI)

```bash
qodercli plugins validate ./apify     # structural check + discovered components
qodercli plugins install ./apify      # local install
# then FULLY RESTART qodercli (MCP servers register at startup, not on /plugins reload)
```

Verify: `qodercli plugins list` shows the 5 skills + `apify` agent; `/mcp` → **Plugin** tab shows the `apify` server as connected.

## Manifest gotchas (learned the hard way)

`apify/.qoder-plugin/plugin.json` is **pure metadata + one file pointer**:

- **Do not** add dir-valued path pointers. `"agents": "./agents/"` fails to parse, and `"skills": "./skills/"` silently loads **zero** skills. Skills and agents load via **convention discovery** (`skills/*/SKILL.md`, `agents/*.md`), no pointer needed.
- `"mcpServers": "./.mcp.json"` (a single-file pointer) is fine and is kept.
- Skills carry **no `version`** (optional in Qoder; the plugin's release version lives in `plugin.json`).

See [RELEASE.md](./RELEASE.md) for how to release and publish.
