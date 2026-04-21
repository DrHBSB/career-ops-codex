# Codex Setup

Career-Ops runs locally in Codex by following `AGENTS.md` and reusing the
existing `modes/*.md`, `*.mjs`, templates, and tracker flow already checked
into the repo.

This fork is Codex-first by project direction, with Codex as the intended local
runtime.

Phase 1 is documentation and routing enablement plus repo-native scripts. It is
not full Claude feature parity.

## Default Auth Story

- Sign in to your local Codex client with ChatGPT.
- No API key is required for the normal local Codex workflow in this fork.
- API keys remain optional for separate integrations such as `gemini-eval.mjs`
  and future provider-specific scripts.

If your environment exposes a `codex` command, you can launch it from the repo
root. If not, use the Codex app or the local Codex surface available on your
machine.

## Prerequisites

- A local Codex client signed in with ChatGPT
- Node.js 18+
- Playwright Chromium installed for PDF generation and repo-native checks
- Go 1.21+ if you want the TUI dashboard

## Install

```bash
npm install
npx playwright install chromium
```

## Start Using Career-Ops In Codex

1. Open Codex in this repo.
2. Make sure your Codex client is signed in with ChatGPT.
3. Start with a plain-language prompt such as:

- `Evaluate this job URL with Career-Ops and run the full pipeline.`
- `Scan my configured portals for matching roles.`
- `Generate the tailored ATS PDF for this role using Career-Ops.`
- `Show me the tracker status and what needs attention.`

## Routing Map

| User intent | Files Codex should read |
| --- | --- |
| Raw JD text or job URL | `modes/_shared.md` + `modes/auto-pipeline.md` |
| Single evaluation only | `modes/_shared.md` + `modes/oferta.md` |
| Multiple offers | `modes/_shared.md` + `modes/ofertas.md` |
| Portal scan | `modes/_shared.md` + `modes/scan.md` |
| PDF generation | `modes/_shared.md` + `modes/pdf.md` |
| Live application help | `modes/_shared.md` + `modes/apply.md` |
| Pipeline inbox processing | `modes/_shared.md` + `modes/pipeline.md` |
| Tracker status | `modes/tracker.md` |
| Deep company research | `modes/deep.md` |
| Training or certification review | `modes/training.md` |
| Project evaluation | `modes/project.md` |
| Pattern analysis | `modes/patterns.md` |
| Follow-up cadence | `modes/followup.md` |

The key point: Codex should route into the existing Career-Ops logic instead of
introducing a new automation layer.

## Phase 1 Status

Codex-ready in Phase 1:

- Single-offer evaluation and auto-pipeline work
- Tracker, merge, verification, and normalization scripts
- PDF generation via `generate-pdf.mjs`
- Portal scanning via `scan.mjs`
- Repo-guided onboarding and customization through the checked-in mode files

Not yet ported in Phase 1:

- Native Codex batch workers
- A Codex replacement for `batch/batch-runner.sh`
- Full parity for any workflow that assumes Claude-specific slash commands or
  hook integration

Batch remains legacy-only for now because `batch/batch-runner.sh` still invokes
`claude -p`. Codex batch is a Phase 2 item.

## Behavioral Rules

- Treat raw JD text or a job URL as the full auto-pipeline path unless the user
  explicitly asks for evaluation only.
- Keep personalization in `config/profile.yml`, `modes/_profile.md`,
  `article-digest.md`, or `portals.yml`.
- Never submit an application for the user.
- Never add new tracker rows directly to `data/applications.md`; use the TSV
  addition flow and `merge-tracker.mjs`.
- For logged-in or browser-only flows that Codex cannot inspect directly, ask
  for pasted text or screenshots instead of assuming access.

## Verification

```bash
npm run verify

# optional dashboard build
cd dashboard && go build ./...
```
