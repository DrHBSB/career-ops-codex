# Career-Ops for Codex

Career-Ops is a local job-search workspace that Codex should operate directly.
This is the primary operating guide for Codex in this repo.

Codex should reuse the existing checked-in modes, scripts, templates, and
tracker flow. Do not create a parallel automation layer when the existing
Career-Ops files already define the behavior.

## Default Runtime

- Default local runtime: Codex signed in with ChatGPT.
- No API key is required for the normal local Codex workflow in this fork.
- API-key-based integrations such as `gemini-eval.mjs` remain optional and
  separate from the default Codex path.
- Keep `CLAUDE.md`, `.claude/`, `.gemini/`, and `.opencode/` as legacy and
  reference surfaces. Do not remove them unless a later task explicitly asks.

## Core Rules

- Reuse `modes/*.md`, `*.mjs`, `templates/*`, `batch/*`, and `dashboard/*`.
- Treat raw JD text or a job URL as the full auto-pipeline path unless the user
  explicitly asks for evaluation only.
- Keep user-specific customization in `config/profile.yml`,
  `modes/_profile.md`, `article-digest.md`, or `portals.yml`.
- Never put user-specific customization in `modes/_shared.md`.
- Never submit an application on the user's behalf.
- Never add new tracker rows directly to `data/applications.md`; use the TSV
  addition flow and `merge-tracker.mjs`.
- Preserve the existing tracker schema, scoring logic, PDF templates, and core
  mode behavior unless the user explicitly asks to change them.

## Data Contract

User Layer files are never auto-updated and are where personalization lives:

- `cv.md`
- `config/profile.yml`
- `modes/_profile.md`
- `article-digest.md`
- `portals.yml`
- `data/*`
- `reports/*`
- `output/*`
- `jds/*`
- `interview-prep/*`

System Layer files contain shared logic and can be updated safely:

- `AGENTS.md`, `CLAUDE.md`, `docs/*`
- `modes/_shared.md`, the mode files in `modes/`, and language variants
- `*.mjs`
- `templates/*`
- `batch/*`
- `dashboard/*`
- `.claude/skills/*`
- `.agents/skills/*`

For the full contract, read `DATA_CONTRACT.md`.

## Codex Routing

Use the existing Career-Ops modes instead of inventing new prompt logic.

| User intent | Files Codex should load |
| --- | --- |
| Raw JD text or job URL | `modes/_shared.md` + `modes/auto-pipeline.md` |
| Single evaluation only | `modes/_shared.md` + `modes/oferta.md` |
| Compare offers | `modes/_shared.md` + `modes/ofertas.md` |
| Portal scan | `modes/_shared.md` + `modes/scan.md` |
| PDF generation | `modes/_shared.md` + `modes/pdf.md` |
| Live application help | `modes/_shared.md` + `modes/apply.md` |
| Pipeline inbox processing | `modes/_shared.md` + `modes/pipeline.md` |
| Tracker status | `modes/tracker.md` |
| Deep company research | `modes/deep.md` |
| Training or certification review | `modes/training.md` |
| Project evaluation | `modes/project.md` |
| Rejection pattern analysis | `modes/patterns.md` |
| Follow-up cadence | `modes/followup.md` |
| Batch processing | `modes/_shared.md` + `modes/batch.md` for reference only |

Batch is not yet Codex-native in Phase 1. The checked-in batch runner still
invokes `claude -p`, so Codex should not present local batch workers as
implemented.

## Onboarding And Session Checks

Before normal workflows, confirm the local setup is ready:

1. `cv.md` exists
2. `config/profile.yml` exists
3. `modes/_profile.md` exists
4. `portals.yml` exists

If any are missing, guide the user through onboarding and keep personalization
in the user-layer files listed above.

When practical, silently run:

```bash
node update-system.mjs check
```

If an update is available, explain that it only targets system-layer files and
does not touch the user's CV, tracker, reports, or other personal data.

## Codex-Specific Operating Notes

- Use public-page reading, repo inspection, and repo-native scripts as the main
  Codex workflow.
- For logged-in or browser-only pages that Codex cannot directly inspect, ask
  the user to paste the text or share a screenshot instead of pretending the
  page is accessible.
- Use `generate-pdf.mjs` for PDF rendering. Node Playwright in this repo is a
  script dependency, not a guarantee of Claude-style browser control.
- Keep the existing legacy command surfaces intact for Claude, Gemini CLI, and
  OpenCode. This fork makes Codex first-class without removing those references.

## Start Here

- User-facing setup: `docs/CODEX.md`
- General repo setup: `docs/SETUP.md`
- Shared system behavior and legacy reference: `CLAUDE.md`
