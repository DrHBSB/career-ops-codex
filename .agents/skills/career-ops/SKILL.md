---
name: career-ops
description: Use when evaluating job descriptions, scanning configured job portals, generating tailored ATS PDFs, processing pipeline/tracker workflows, or routing Career-Ops job-search tasks. Do not use for native batch orchestration; Phase 1 batch remains legacy Claude-only because batch/batch-runner.sh invokes claude -p.
---

# Career-Ops

Use this skill to run Career-Ops inside Codex without creating a parallel
workflow.

## First Principles

- Reuse the existing checked-in `modes/*.md`, `*.mjs`, templates, and tracker
  flow.
- Keep personalization in `config/profile.yml`, `modes/_profile.md`,
  `article-digest.md`, or `portals.yml`.
- Never submit an application for the user.
- Never add new tracker rows directly to `data/applications.md`; use the TSV
  addition flow and `merge-tracker.mjs`.
- Preserve scoring logic, report format, PDF templates, and tracker schemas.

## Routing

Load the same mode files already used by the legacy Career-Ops surfaces.

| Intent | Files to load |
| --- | --- |
| Raw JD text or a job URL | `modes/_shared.md` + `modes/auto-pipeline.md` |
| Evaluation only | `modes/_shared.md` + `modes/oferta.md` |
| Compare offers | `modes/_shared.md` + `modes/ofertas.md` |
| Portal scan | `modes/_shared.md` + `modes/scan.md` |
| PDF generation | `modes/_shared.md` + `modes/pdf.md` |
| Apply help | `modes/_shared.md` + `modes/apply.md` |
| Pipeline processing | `modes/_shared.md` + `modes/pipeline.md` |
| Tracker status | `modes/tracker.md` |
| Deep research | `modes/deep.md` |
| Training review | `modes/training.md` |
| Project evaluation | `modes/project.md` |
| Pattern analysis | `modes/patterns.md` |
| Follow-up cadence | `modes/followup.md` |

If the user pastes a JD or public job URL with no explicit sub-mode, default to
the full auto-pipeline path.

## Batch Status

Batch is not yet ported to native Codex workers in Phase 1.

- You may read `modes/_shared.md` + `modes/batch.md` to explain the existing
  design.
- Do not claim that local Codex batch orchestration is implemented.
- `batch/batch-runner.sh` still invokes `claude -p`, so the standalone batch
  runner remains a legacy Claude path for now.

## Codex Runtime Notes

- For public pages, use the normal Codex browsing and repo workflow.
- For logged-in or browser-only flows that are not directly inspectable, ask
  the user for pasted text or screenshots.
- Prefer repo-native scripts such as `scan.mjs`, `generate-pdf.mjs`,
  `merge-tracker.mjs`, and `verify-pipeline.mjs` when they already implement
  the required behavior.
