---
title: "[Nice-to-have] Pre-seeded broken test fixtures for hook + agentic-workflow demos"
labels: ["workshop", "nice-to-have", "module:6", "module:7", "copilot-coding-agent"]
---

> **Nice-to-have.** This issue is **not** part of [`implementation.md`](../implementation.md). Modules 6 and 7 ask participants to "deliberately break a test" with no scaffolding; this issue gives them a curated set instead.

## User story

**As a** participant at Module 6 or Module 7 who doesn't want to invent a broken test on the spot,
**I want** a small set of pre-seeded broken test patches I can apply,
**so that** I trigger the hook block and the triage workflow with a controlled, classifiable failure mode each time.

## Description

Create `demo/walkthroughs/qa-testing-workshop/reference/broken-test-fixtures/` containing one Markdown file per fixture, each describing:

- Which module it serves (7 hooks, 6 workflows, or both).
- The exact patch to apply (as a fenced diff block) and where.
- The expected hook or workflow behaviour (block message, classification category).
- How to revert.

Cover at minimum:

1. A `.only` introduction (hook demo).
2. A `page.waitForTimeout` introduction (covers the path-scoped instruction's "no waitForTimeout" rule visibly).
3. A selector-drift failure (workflow triage classifies as "selector drift").
4. A network-flake-style failure (workflow triage classifies as "network flake").
5. A real assertion-vs-actual mismatch (workflow triage classifies as "real regression").

Update Module 6 and Module 7 walkthroughs (in their "Try it yourself" sections only — don't change the main steps) to point here as an optional alternative to inventing failures.

## Acceptance criteria

- **Given** any fixture's diff, **when** applied to a clean working tree, **then** it produces the failure described.
- **Given** the workflow-triage fixtures, **when** the workflow runs against each, **then** the posted comment's classification matches the fixture's expected category.
- **Given** the hook fixtures, **when** Copilot is asked to apply them, **then** the hook blocks the write and prints the expected message.
- **Given** the README in the directory, **when** read, **then** participants can pick a fixture, apply it, and revert without consulting other docs.

## Dependencies

- Issues #10 (Module 6) and #11 (Module 7) merged.
