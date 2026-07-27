# Releasing & Publishing

The plugin ships through two channels, both sourced from the marketplace:

- **Qoder CLI:** installed from the marketplace by manually adding the repo link. No build needed.
- **QoderWork & Qoder IDE:** installed from the marketplace after the plugin is submitted via ZIP and the publish form. Manual, review-gated.

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

That ZIP is the artifact for the QoderWork & Qoder IDE marketplace.

### How to run it

1. Go to **Actions → Create a release → Run workflow** (branch: `main`).
2. Pick a **Release type**:
   - `auto` — next version inferred from commit history (needs at least one prior tag).
   - `patch` / `minor` / `major` — force a specific bump.
   - `custom` — set **custom_version** explicitly (e.g. `0.1.0`). **Use this for the very first release**, since `auto` has no prior tag to diff against.
3. Run it, then check the `chore(release): <version>` commit diff and the published release.

## 2. Publish to QoderWork & Qoder IDE (public marketplace)

Publishing is a **manual, review-gated** step in the web app. There is no git/API auto-publish. Both QoderWork and Qoder IDE install the plugin from this marketplace listing.

1. Go to **qoder.com > My Publications > Publish > Plugin**.
2. Fill the **Publish Plugin** form:
   - **Icon:** `assets/icon.png` (PNG/JPG/WebP, 1:1, ≤500 KB). *SVG is not accepted.*
   - **Display name:** `Apify`
   - **Description:** a marketplace-friendly summary.
   - **Category:** pick from the dropdown (e.g. Productivity / Developer Tools).
   - **Contact:** `support@apify.com` or the GitHub repo URL.
   - **Plugin file:** the `apify-qoder-plugin-<tag>.zip` from the GitHub release (must contain `plugin.json`; ≤500 files, ≤100 MB).
3. **Submit for Review** (Plugin review is a structural pre-check + automated review).
4. Manage the listing under **My Publications** (installs, edit info, new versions, delist/relist). A new version re-triggers full review; existing users get an opt-in update notification.

## 3. Distribute via Qoder CLI (marketplace)

No build or ZIP. The CLI reads the repo's root `.qoder-plugin/marketplace.json`:

```bash
qodercli plugins marketplace add apify/apify-qoder-plugin   # owner/repo or the git URL
qodercli plugins install apify@apify                        # plugin-name @ marketplace-name
qodercli plugins marketplace update                         # pull later updates
```

## Notes & open items

- `.qoder-plugin/marketplace.json` is **Qoder-CLI-only**. QoderWork and Qoder IDE ignore it and take listing metadata from the publish form.
- **MCP auth:** the bundled `.mcp.json` uses the bare `https://mcp.apify.com` URL, which works via OAuth in the Qoder CLI. For the **QoderWork/Qoder IDE marketplace**, the "resolvable dependencies" rule may require the `{{USER_CONFIG}}` + `_setup` declaration form, or an officially listed Apify **Connector**. Verify on first submission.
- **Naming:** QoderWork favors a real-world role/job-title name; "Apify" is a brand and may draw review feedback.
- **Skill content placeholders:** skills document credentials with placeholders (`<APIFY_TOKEN>`, `export APIFY_TOKEN=your_token_here`). This is standard and satisfies the "no real credentials in examples" rule, but a strict automated review *could* flag them. Watch for this on the first marketplace submission.
