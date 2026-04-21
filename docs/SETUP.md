# Setup Guide

## Prerequisites

- A local Codex client signed in with ChatGPT
- Node.js 18+ (for PDF generation and utility scripts)
- (Optional) Go 1.21+ (for the dashboard TUI)

No API key is required for the normal local Codex workflow in this fork.

## Quick Start (6 steps)

### 1. Clone and install

```bash
git clone <your-fork-url> career-ops-codex
cd career-ops-codex
npm install
npx playwright install chromium   # Required for PDF generation
```

### 2. Sign in to Codex

Open your local Codex client for this directory and sign in with ChatGPT.

If your environment exposes the CLI directly, you can start it from the repo
root:

```bash
codex
```

### 3. Configure your profile

```bash
cp config/profile.example.yml config/profile.yml
```

Edit `config/profile.yml` with your personal details: name, email, target
roles, narrative, and proof points.

### 4. Add your CV

Create `cv.md` in the project root with your full CV in markdown format. This
is the source of truth for all evaluations and PDFs.

(Optional) Create `article-digest.md` with proof points from your portfolio
projects or articles.

### 5. Configure portals

```bash
cp templates/portals.example.yml portals.yml
```

Edit `portals.yml`:

- Update `title_filter.positive` with keywords matching your target roles
- Add companies you want to track in `tracked_companies`
- Customize `search_queries` for your preferred job boards

### 6. Start using Career-Ops in Codex

Use plain-language prompts in Codex, for example:

- `Evaluate this job URL with Career-Ops and run the full pipeline.`
- `Scan my configured portals for new roles that match my profile.`
- `Generate the tailored ATS PDF for this role.`
- `Show me the tracker status and next actions.`

## Codex-Oriented Usage

Codex is the intended local runtime in this Codex-first fork. The normal local
workflow is:

1. Open Codex in this repo
2. Stay signed in with ChatGPT
3. Use natural-language prompts that route into the existing mode files

Legacy slash-command surfaces for Claude, Gemini CLI, and OpenCode are kept in
the repo for reference and compatibility, but they are not the default path in
this fork.

## Available Actions

| Action | Codex-oriented prompt |
| --- | --- |
| Evaluate an offer | `Evaluate this job URL with Career-Ops.` |
| Search for offers | `Scan my configured portals for matching roles.` |
| Process pending URLs | `Process the pending URLs in my pipeline.` |
| Generate a PDF | `Generate the ATS PDF for this role.` |
| Check tracker status | `Show me the tracker status.` |
| Fill an application form | `Help me answer this application form.` |

## Verify Setup

```bash
node cv-sync-check.mjs      # Check configuration
node verify-pipeline.mjs    # Check pipeline integrity
```

## Build Dashboard (Optional)

```bash
cd dashboard
go build -o career-dashboard .
./career-dashboard --path ..
```

## Phase 1 Limitation

Batch processing is not yet ported to native Codex workers in Phase 1.
`batch/batch-runner.sh` still invokes `claude -p`, so keep batch as a legacy
Claude path for now. Codex batch is a Phase 2 item.
