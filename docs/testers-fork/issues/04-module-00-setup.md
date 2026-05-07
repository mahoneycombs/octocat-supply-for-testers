---
title: "Author Module 0 — Setup and orientation walkthrough"
labels: ["workshop", "core", "documentation", "module:0", "copilot-coding-agent"]
---

## User story

**As a** QA engineer who just cloned the workshop fork,
**I want** a setup walkthrough that gets every prerequisite verified and surfaces failures early,
**so that** I don't hit a missing dependency three modules in.

## Description

Author `demo/walkthroughs/qa-testing-workshop/00-setup.md` following the per-module structural skeleton in [`implementation.md`](../implementation.md) → "Style and voice conventions" (Title, What you'll do, Why, Where files live, Steps, Try it yourself, What you built, Next).

Required step content:

- Clone the fork, run `make install`, run `make dev`, verify both ports respond.
- Verify Copilot Chat works in VS Code; confirm Enterprise tier in account settings.
- Walk through `npm init playwright@latest` accepting TypeScript defaults; point the test directory at `tests/` (link Issue #02's bootstrap as the expected end state).
- Verify the existing Playwright MCP server is configured by opening `.vscode/mcp.json`.
- Tour where each customization type lives (condensed directory map).
- Final checkpoint: a one-line "you're ready when..." confirmation.

Word budget: 600–1200 words of prose.

## Acceptance criteria

- **Given** a new clone of the fork, **when** a participant follows the steps end to end, **then** they reach the "you're ready when..." line without consulting external docs.
- **Given** the file, **when** scanned, **then** every step has a goal sentence, an action, and a verification.
- **Given** the prose, **when** reviewed against the style guide, **then** voice is direct, second-person, present tense, and no banned words appear.
- **Given** more than four callouts, **when** counted, **then** the count drops to four or fewer (callouts must earn their place).

## Source

[`implementation.md`](../implementation.md) — `00-setup.md` section; structural conventions.

## Dependencies

- Issues #01 (scaffolding), #02 (test bootstrap).
