# Releasing & Publishing

The plugin ships as a single **Qoder marketplace** listing, published from a GitHub release ZIP:

- **Qoder CLI, IDE, Desktop app, and QoderWork** install that plugin from the marketplace listing. The install method differs per surface — a CLI command, a one-click install in the IDE, or downloading the ZIP and importing it (Desktop and QoderWork). See the end-user docs.
- **QoderWake and the JetBrains plugin** don't use the plugin. They connect to the Apify MCP server (`https://mcp.apify.com`) directly, so they need no release or publish step.

## Versioning

The release version lives in **three** manifest fields, all kept in lockstep (semver):

- `apify/.qoder-plugin/plugin.json` → `version`
- `.qoder-plugin/marketplace.json` → `metadata.version`
- `.qoder-plugin/marketplace.json` → `plugins[0].version`

Skills/agents carry no version. **You do not bump these by hand** — the release workflow computes the next version and writes all three (plus `CHANGELOG.md`) for you.

## 1. Cut a GitHub release (push-button)

Releases are triggered from the **Actions** tab — no manual tagging. On dispatch, [`.github/workflows/release.yml`](./.github/workflows/release.yml):

1. **`release_metadata`** — [`apify/actions/git-cliff-release`](https://github.com/apify/actions) derives the next version and release notes from conventional commits since the last tag.
2. **`bump_and_changelog`** — writes the version into the three manifest fields + `CHANGELOG.md`, commits as `apify-plugins-bot`, and pushes to `main`.
3. **`create_github_release`** — tags that commit and publishes the GitHub release with the generated notes.
4. **`package_zip`** — zips the **contents of `apify/`** (so `.qoder-plugin/` is at the ZIP root) and attaches `apify-qoder-plugin-<tag>.zip` to the release.

That ZIP is the artifact you publish to the Qoder marketplace.

### How to run it

1. Go to **Actions → Create a release → Run workflow** (branch: `main`).
2. Pick a **Release type**:
   - `auto` — next version inferred from commit history (needs at least one prior tag).
   - `patch` / `minor` / `major` — force a specific bump.
   - `custom` — set **custom_version** explicitly (e.g. `0.1.0`). **Use this for the very first release**, since `auto` has no prior tag to diff against.
3. Run it, then check the `chore(release): <version>` commit diff and the published release.

## 2. Publish to the Qoder marketplace

Publishing is a **manual, review-gated** step in the web app. There is no git/API auto-publish. The Qoder CLI, IDE, Desktop app, and QoderWork all install the plugin from this marketplace listing.

1. Go to **qoder.com > My Publications > Publish > Plugin**.
2. Fill the **Publish Plugin** form:
   - **Icon:** upload `assets/icon.png` — 1:1, ≤500 KB, PNG/JPG/WebP (SVG is not accepted).
   - **Display name:** `Apify`
   - **Description:** use the `description` field from `apify/.qoder-plugin/plugin.json`.
   - **Category:** `Data Analysis` (from the fixed dropdown: Administration · HR · Finance · Legal · Marketing · Sales · Product & R&D · Operations · Data Analysis · Supply Chain · Design · Government & Public Affairs · Healthcare · Education · Consulting · Creation · Investment · Other).
   - **Developer signature:** `Apify` (shown publicly as "by Apify"; defaults to the submitting account's name, so set it explicitly).
   - **Contact:** `support@apify.com`
   - **Plugin file:** `apify-qoder-plugin-<tag>.zip` from the GitHub release (must contain `plugin.json`; ≤500 files, ≤100 MB).
3. **Submit for Review** (Plugin review is a structural pre-check + automated review).
4. Manage the listing under **My Publications** (installs, edit info, new versions, delist/relist). A new version re-triggers full review; existing users get an opt-in update notification.

## 3. Installing the plugin (end users)

End-user installation is documented in the Apify docs:

- **[Qoder integration](https://docs.apify.com/integrations/qoder-plugin)** — the Qoder CLI, IDE, Desktop app, and QoderWork, plus QoderWake and the JetBrains plugin via the Apify MCP server.
- **[Qwen Code integration](https://docs.apify.com/integrations/qwen-code)** — Qwen Code reuses the same plugin.

## Notes & open items

- `.qoder-plugin/marketplace.json` is **Qoder-CLI-only**. The GUI surfaces (IDE, Desktop app, QoderWork) ignore it and take listing metadata from the publish form.
- **MCP auth:** the bundled `.mcp.json` uses the bare `https://mcp.apify.com` URL; Qoder authorizes via OAuth on first use (no API token to paste). Verified in the Qoder CLI.
- **Naming:** the marketplace favors a real-world role/job-title name; "Apify" is a brand and may draw review feedback.
- **Skill content placeholders:** skills document credentials with placeholders (`<APIFY_TOKEN>`, `export APIFY_TOKEN=your_token_here`). This is standard and satisfies the "no real credentials in examples" rule, but a strict automated review *could* flag them. Watch for this on the first marketplace submission.
