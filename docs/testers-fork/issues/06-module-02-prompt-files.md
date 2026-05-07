---
title: "Author Module 2 — Prompt Files (E2E scaffold, API contract, visual regression)"
labels: ["workshop", "core", "documentation", "module:2", "customization:prompts", "copilot-coding-agent"]
---

## User story

**As a** QA engineer who retypes the same Playwright scaffolding requests all day,
**I want** reusable slash-command prompt files for the most common test types,
**so that** I produce a consistent test in seconds and onboard new team members to one shared shape.

## Description

Author `demo/walkthroughs/qa-testing-workshop/02-prompt-files.md` and create three prompt files:

1. `.github/prompts/qa-scaffold-e2e.prompt.md` — frontmatter `description`, `mode: agent`. Takes a flow name, references `qa-playwright.instructions.md` via Markdown link, produces a Playwright spec with happy path + one validation error case + one network failure case. Output target: `tests/e2e/<flow>.spec.ts`.
2. `.github/prompts/qa-api-contract-test.prompt.md` — same frontmatter shape. Takes a resource name; locates the OpenAPI spec (the implementing author **must grep for `openapi:` in the repo first** to confirm the path before writing this); produces a Playwright API test exercising every operation on the resource and validating response shape against the schema. Output: `tests/api/<resource>.spec.ts`.
3. `.github/prompts/qa-visual-regression.prompt.md` — same shape. Takes a page name; produces a Playwright spec capturing snapshots at mobile, tablet, desktop viewports. Output: `tests/e2e/visual/<page>.spec.ts`. **Do not run this prompt yet** — Module 5 covers running it with MCP.

Walkthrough must:

- Step 1: build `qa-scaffold-e2e.prompt.md`, walk frontmatter and body.
- Step 2: invoke `/qa-scaffold-e2e add-to-cart`, save as `tests/e2e/cart.spec.ts`, run it.
- Step 3: build `qa-api-contract-test.prompt.md`.
- Step 4: invoke `/qa-api-contract-test orders`, save as `tests/api/orders.spec.ts`, run it.
- Step 5: build `qa-visual-regression.prompt.md` to reinforce the pattern (no run).
- Try-it: participants design and write a fourth prompt of their own.

## Acceptance criteria

- **Given** the OpenAPI spec lookup, **when** the contract prompt is authored, **then** the prompt references the verified actual path in the repo (not a guessed path).
- **Given** Module 2 followed end to end, **when** the participant invokes both `/qa-scaffold-e2e add-to-cart` and `/qa-api-contract-test orders`, **then** the resulting `tests/e2e/cart.spec.ts` and `tests/api/orders.spec.ts` pass against `make dev`.
- **Given** all three prompt files, **when** opened in VS Code Copilot Chat, **then** they appear in the slash command menu.
- **Given** `qa-scaffold-e2e.prompt.md`, **when** rendered, **then** it contains a working Markdown link to `qa-playwright.instructions.md`.

## Source

[`implementation.md`](../implementation.md) — `02-prompt-files.md` section; the three prompt-file specs; Implementation note #2 (verify OpenAPI location).

## Dependencies

- Issues #01, #02, #05 (instructions module must be merged so the prompt can link to the path-scoped file).
