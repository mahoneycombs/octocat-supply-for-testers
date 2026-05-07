---
title: "Scaffold the QA Copilot Customization Workshop directory structure"
labels: ["workshop", "core", "scaffolding", "copilot-coding-agent"]
---

## User story

**As a** workshop author preparing the QA Copilot Customization Workshop,
**I want** the full directory skeleton for the workshop in place (empty or stub files only),
**so that** every subsequent module issue has a predictable home for its artifacts and reviewers can see the shape of the workshop before any content lands.

## Description

Create the directory and empty-file skeleton described under "Repository changes at a glance" in [`implementation.md`](../implementation.md). This issue is **structure only** — no walkthrough prose, no Playwright tests, no script bodies. Every file should exist as either an empty file or a one-line placeholder so later issues can fill them in.

Add net-new content under:

- `.github/instructions/`, `.github/prompts/`, `.github/agents/`, `.github/skills/`, `.github/workflows/`, `.github/hooks/` (scoped with the `qa-` prefix where the design doc specifies)
- `plugins/qa-toolkit/.github/`
- `tests/` (top-level, with `e2e/`, `e2e/pages/`, and `api/` subfolders)
- `demo/walkthroughs/qa-testing-workshop/` (with `reference/` subfolder)

Do **not** delete or restructure anything in the existing repo. Do **not** modify `.github/copilot-instructions.md` or `.vscode/mcp.json` in this issue — those land in the modules that depend on them.

## Acceptance criteria (Given/When/Then)

- **Given** the repository is checked out fresh, **when** I run `tree demo/walkthroughs/qa-testing-workshop/ .github/{instructions,prompts,agents,skills,workflows,hooks} plugins/qa-toolkit tests`, **then** the output matches the structure in [`implementation.md`](../implementation.md) under "Repository changes at a glance".
- **Given** the scaffolding PR is merged, **when** I diff against `main`, **then** no existing file outside the new directories is modified.
- **Given** an empty placeholder file, **when** opened, **then** it contains either nothing or a single HTML/Markdown comment noting which issue will fill it (e.g. `<!-- Filled by issue #05 -->`).
- **Given** the new directories, **when** I run `git ls-files`, **then** every directory has at least one tracked file (use `.gitkeep` if needed).

## Source

[`implementation.md`](../implementation.md) — "Repository changes at a glance".

## Dependencies

None. This issue blocks every module issue.
