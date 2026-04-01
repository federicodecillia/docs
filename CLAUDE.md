# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

Mintlify documentation site for the **GPT Chatbot API** (AI Agent Builder). Content is authored in MDX and deployed automatically via the Mintlify GitHub App when changes are pushed to `main`.

## Development Commands

```bash
# Install Mintlify CLI globally (one-time)
npm i -g mintlify

# Preview docs locally (run from repo root, where mint.json lives)
mintlify dev

# If dev server fails to start
mintlify install  # re-installs Mintlify dependencies, then retry mintlify dev
```

No build step, no test suite, no lint commands — Mintlify handles everything.

## Deployment

Pushing to `main` triggers automatic deployment via the Mintlify GitHub App. No manual deploy step needed.

## Upstream Sync

This repo is a fork of `ks-collab/gpt-trainer-docs` with all `gpt-trainer` references rebranded to `GPT Chatbot`. A GitHub Actions workflow (`.github/workflows/sync-upstream.yml`) keeps it in sync.

**Trigger manually** (preferred — runs on demand):
```bash
gh workflow run sync-upstream.yml --repo federicodecillia/docs
```
Or go to **Actions → Sync Upstream & Rebrand → Run workflow** in the GitHub UI.

**Scheduled run:** currently set to the 1st of every month at 08:00 UTC. To change the cadence, edit the `cron` field on line 6 of the workflow file.

The workflow opens a PR (`sync/upstream-YYYY-MM-DD`) with a review checklist. After merging, scan for any missed branding with:
```bash
grep -r "gpt-trainer" --include="*.mdx" --include="*.json" .
```

## Repository Structure

```
mint.json              ← Mintlify config: nav, colors, API base URL, anchors
*.mdx                  ← Root-level user guides (getting started, feature walkthroughs)
api-reference/         ← API docs organized by resource (chatbots, agents, sessions, messages, data-sources, source-tags)
integrations/          ← Integration guides (currently: Slack)
tools/                 ← Tools documentation
logo/                  ← SVG logos and favicon
images/                ← Screenshots and illustrations referenced in MDX files
```

## Content Architecture

### Navigation is defined in `mint.json`
Adding a new page requires two things: (1) create the `.mdx` file and (2) add its path to the `navigation` array in `mint.json`. Pages not in `mint.json` won't appear in the sidebar.

### API reference structure
Each API resource (e.g., `api-reference/chatbots/`) follows a consistent pattern:
- `create.mdx`, `fetch.mdx`, `update.mdx`, `delete.mdx` — individual endpoint docs
- `properties-reference.mdx` — full field reference for that resource

### API configuration
- Main API base URL: `https://app.gptchatbot.it/api` (set in `mint.json` under `"api"`)
- Tools API base URL: `https://tools.gptchatbot.it/` (set in the Tools anchor)
- Auth method: Bearer token for both

### MDX conventions
- Use kebab-case filenames
- Images live in `/images/` and are referenced as `/images/filename.png`
- Mintlify components (`<Card>`, `<CardGroup>`, `<Tabs>`, `<Note>`, etc.) are available without imports
