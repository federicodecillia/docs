# Plan: gptchatbot_api — Python Client Library & Scripts

## Context

This repo ([federicodecillia/docs](https://github.com/federicodecillia/docs)) is a fork of [ks-collab/gpt-trainer-docs](https://github.com/ks-collab/gpt-trainer-docs) with all "gpt-trainer" references rebranded to "GPT Chatbot". It must be periodically synced with upstream to incorporate new API endpoints, parameters, or behaviour changes — see the **Docs Sync Workflow** section below.


## Docs Sync Workflow

Upstream updates may introduce new API endpoints, changed parameters, or deprecated features
that directly impact this client library.

### Remote setup

```bash
cd ../docs
git remote -v
# origin    https://github.com/federicodecillia/docs.git
# upstream  https://github.com/ks-collab/gpt-trainer-docs.git
```

### Running the sync

The sync is fully automated via `.github/workflows/sync-upstream.yml`.

**Trigger manually** (run on demand from your terminal):
```bash
gh workflow run sync-upstream.yml --repo federicodecillia/docs
```
Or go to **Actions → Sync Upstream & Rebrand → Run workflow** in the GitHub UI.

**Scheduled run:** runs automatically on the 1st of every month at 08:00 UTC.
To change the cadence, edit the `cron` field on line 6 of the workflow file.

The workflow will:
1. Fetch `ks-collab/gpt-trainer-docs` and skip if already up to date
2. Create a `sync/upstream-YYYY-MM-DD` branch and merge upstream
3. Re-apply branding (URL replacements first, then text replacements)
4. Open a PR against `main` with a review checklist

**After the PR is opened:**
1. Check for merge conflicts (`<<<<<<` markers)
2. Verify no stray references remain: `grep -r "gpt-trainer" --include="*.mdx" --include="*.json" .`
3. Preview with `mintlify dev`
4. Review API diff for new/changed/removed endpoints (see checklist below)
5. Merge into `main`

### Checklist after each docs sync

- [ ] Review API reference diff for new/changed/removed endpoints
- [ ] Update this `PLAN.md` API tables if endpoints changed
- [ ] Update `gptchatbot/models/` if request/response shapes changed
- [ ] Update `gptchatbot/api/` for new or modified endpoints
- [ ] Add/update tests for any changed API behaviour
- [ ] Run `uv run pytest` to verify nothing broke
- [ ] Update `CLAUDE.md` if stack or design choices changed