---
title: "[Nice-to-have] Sample bug report library for the qa-bug-reproducer agent"
labels: ["workshop", "nice-to-have", "documentation", "module:3", "copilot-coding-agent"]
---

> **Nice-to-have.** This issue is **not** part of [`implementation.md`](../implementation.md). Module 3 only has participants build the `qa-bug-reproducer` agent's frontmatter; they don't actually use it. This issue creates an optional exercise that does.

## User story

**As a** participant who built the `qa-bug-reproducer` agent in Module 3,
**I want** three realistic bug reports against OctoCAT Supply I can hand the agent,
**so that** I can experience the full reproduce-before-fix flow that the agent's persona promises.

## Description

Create `demo/walkthroughs/qa-testing-workshop/reference/bug-reports/` containing three Markdown files, each a complete bug report with:

- Title.
- Environment.
- Steps to reproduce.
- Expected vs. actual.
- Severity.

The bugs should target real behaviour in the OctoCAT Supply app (verify before authoring — don't invent bugs). Mix:

1. A UI-level issue that produces a deterministic failing E2E test.
2. An API contract issue that produces a deterministic failing API test.
3. A flake/race that requires careful selectors to reproduce reliably.

Add a short `bug-reports/README.md` framing this as an **optional Module 3 extension**: load the `qa-bug-reproducer` agent, paste a report, watch the agent produce a failing test under `tests/e2e/regressions/` or `tests/api/regressions/`, then stop (no fix attempt — that's the agent's contract).

## Acceptance criteria

- **Given** each bug report, **when** verified manually, **then** the underlying behaviour is reproducible in the running app today (or the bug is intentionally seeded — note which).
- **Given** any of the three reports handed to the `qa-bug-reproducer` agent, **when** the agent produces a test, **then** the test fails on `main` and the failure mode matches the bug.
- **Given** the README, **when** read, **then** participants understand this is not a required exercise and where the resulting tests should live.
- **Given** the bug-report files, **when** any production-code edit is implied, **then** the README reminds participants the agent's contract is reproduce-only.

## Dependencies

- Issue #07 (Module 3 — agents) merged.
