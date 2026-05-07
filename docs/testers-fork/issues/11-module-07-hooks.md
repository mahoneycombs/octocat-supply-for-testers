---
title: "Author Module 7 — Hooks (block-test-skip + lint-after-test-edit)"
labels: ["workshop", "core", "documentation", "module:7", "customization:hooks", "copilot-coding-agent"]
---

## User story

**As a** QA engineer whose CI was broken last week by a stray `.only`,
**I want** Copilot to block writes that introduce `.only` or `.skip` and run lint after every test edit,
**so that** I get the signal in the editor instead of from a failing pipeline an hour later.

## Description

Author `demo/walkthroughs/qa-testing-workshop/07-hooks.md` and create:

1. `.github/hooks/qa-hooks.json` — `preToolUse` hook on file writes to `tests/**/*.spec.ts`, plus `postToolUse` hook on writes to `tests/**`. Verify the exact schema against current GitHub docs before committing; the example in [`implementation.md`](../implementation.md) is intent, not literal spec.
2. `.github/hooks/scripts/block-test-skip.sh` — bash, reads stdin (proposed file content), greps for `\.only\b` and `\.skip\b` outside comments, exits 1 with a message if found.
3. `.github/hooks/scripts/lint-after-test-edit.sh` — bash, runs `npm run lint -- <file>` against the path passed in, prints output, exits 0 either way (warn, don't block).

Walkthrough steps:

- Concept: hooks are deterministic shell commands at lifecycle points. Eight hook types exist; this module shows two.
- Step 1: build `qa-hooks.json` `preToolUse` block.
- Step 2: build `block-test-skip.sh`, make it executable.
- Step 3: ask Copilot to add `.only` to a test, watch the hook reject it.
- Step 4: add the `postToolUse` block.
- Step 5: build `lint-after-test-edit.sh`, make it executable.
- Step 6: edit a test file, watch the lint output appear.
- Try-it: add a `sessionStart` hook that prints a reminder if `tests/` has uncommitted changes.

## Acceptance criteria

- **Given** the `preToolUse` hook installed, **when** Copilot attempts to write a test containing `.only`, **then** the write is blocked and the participant sees the block message.
- **Given** the `postToolUse` hook installed, **when** Copilot edits a file under `tests/`, **then** lint output appears and the edit is **not** blocked even if lint fails.
- **Given** both shell scripts, **when** invoked manually, **then** they behave the same outside Copilot.
- **Given** `qa-hooks.json`, **when** validated against current GitHub docs, **then** the schema is current; if the doc-example schema differs from this issue, the current schema wins and a note in the PR description records the divergence.

## Source

[`implementation.md`](../implementation.md) — `07-hooks.md` section; `qa-hooks.json` and the two scripts; Implementation note #1.

## Dependencies

- Issues #01, #02, plus at least one prior module that produces test files (#05 onward) so the hook has something to act on.
