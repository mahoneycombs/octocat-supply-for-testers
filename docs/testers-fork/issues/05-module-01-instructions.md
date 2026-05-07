---
title: "Author Module 1 — Custom Instructions (root + path-scoped)"
labels: ["workshop", "core", "documentation", "module:1", "customization:instructions", "copilot-coding-agent"]
---

## User story

**As a** QA engineer tired of Copilot generating Playwright tests with brittle selectors and missing waits,
**I want** to write project-wide and path-scoped instructions that visibly change generated test code,
**so that** every test Copilot drafts in this repo follows the team's selector strategy and assertion patterns by default.

## Description

Author `demo/walkthroughs/qa-testing-workshop/01-instructions.md` and produce its two artifacts:

1. **Modify** `.github/copilot-instructions.md` — append a clearly delimited "QA conventions" section. Do not rewrite or restructure existing content. Cover: which test types live where, the Playwright-first stance, the no-`waitForTimeout` rule, the test ID convention, and a one-line pointer to the path-scoped instructions file.
2. **Create** `.github/instructions/qa-playwright.instructions.md` — frontmatter `applyTo: "tests/**/*.spec.ts"`, `description: ...`. Body covers locator priority (role > test-id > text > CSS), fixtures over hooks, network mocking with `route` rather than env overrides, expectation patterns, file naming, parallel safety, and what to do when a test is flaky (don't add waits — investigate).

Walkthrough steps must:

- Step 1: open root instructions, read what's there, add the QA section, show the diff.
- Step 2: create the path-scoped file, walking through the YAML frontmatter and rules.
- Step 3: ask Copilot to write `tests/e2e/products.spec.ts` **without** instructions context (capture the output as a comment or screenshot).
- Step 4: ask the same question **with** instructions in place; compare; participant saves the second response as the actual file.
- Verification: `npx playwright test tests/e2e/products.spec.ts` passes against `make dev`.
- Try-it: participant adds one more rule of their choice and observes the next generated test.

Follow the structural skeleton and style guide in [`implementation.md`](../implementation.md). Word budget 600–1200.

## Acceptance criteria

- **Given** Module 1 followed end to end, **when** the resulting `tests/e2e/products.spec.ts` is run, **then** it passes against `make dev`.
- **Given** the diff against `.github/copilot-instructions.md`, **when** reviewed, **then** existing content is untouched and the QA section is clearly delimited.
- **Given** the path-scoped file, **when** Copilot generates a test under `tests/`, **then** the resulting code reflects the documented selector priority and avoids `waitForTimeout`.
- **Given** the implementing author follows their own steps fresh, **when** they finish, **then** Step 5 of [`implementation.md`](../implementation.md) "Implementation notes" (run Module 1 end-to-end) is satisfied and recorded in the PR description.

## Source

[`implementation.md`](../implementation.md) — `01-instructions.md` section, `.github/copilot-instructions.md` modify spec, `.github/instructions/qa-playwright.instructions.md` new spec, Implementation note #5.

## Dependencies

- Issues #01, #02, #04.
