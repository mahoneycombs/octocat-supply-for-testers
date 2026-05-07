---
title: "Author Module 4 — Agent Skills (page-object generator + API contract validator)"
labels: ["workshop", "core", "documentation", "module:4", "customization:skills", "copilot-coding-agent"]
---

## User story

**As a** QA tech lead who wants every page object on the team to follow one template,
**I want** a packaged skill that ships the template plus a generator script Copilot can invoke,
**so that** new page objects look the same regardless of who scaffolded them.

## Description

Author `demo/walkthroughs/qa-testing-workshop/04-agent-skills.md` and create two skill packages:

### Skill 1: `qa-playwright-page-object`

- `.github/skills/qa-playwright-page-object/SKILL.md` — frontmatter `name` and a specific `description` so Copilot's skill discovery picks it up. Body: generation flow, naming (`<PageName>Page.ts`), location (`tests/e2e/pages/`), locator vs action distinction, nested components.
- `.github/skills/qa-playwright-page-object/templates/page-object.template.ts` — minimal Playwright page object with `{{PAGE_NAME}}`, `{{LOCATORS}}`, `{{ACTIONS}}` placeholders.
- `.github/skills/qa-playwright-page-object/scripts/generate-page-object.mjs` — ~30 lines, takes `--name` and `--url`, writes a populated page object to stdout. No network access required.

### Skill 2: `qa-api-contract-validator`

- `.github/skills/qa-api-contract-validator/SKILL.md` — frontmatter as in [`implementation.md`](../implementation.md). Body must reference the actual OpenAPI spec path (grep first; do not guess).
- `.github/skills/qa-api-contract-validator/scripts/validate-against-openapi.mjs` — Node script, takes a path to a JSON file and a path to an OpenAPI spec, reports schema violations. Use `@apidevtools/swagger-parser` + `ajv`. Pin versions in a header comment.

### Walkthrough steps

- Concept framing: skills load their description first, body when relevant, assets on demand. Different from instructions (always-on) and prompts (one-shot).
- Build skill 1 (SKILL.md → template → script).
- Invoke skill in Copilot Chat: "use the qa-playwright-page-object skill to create a page object for the products page" → save as `tests/e2e/pages/ProductsPage.ts`.
- Repeat for `CartPage.ts` and `CheckoutPage.ts`.
- Refactor `products.spec.ts`, `cart.spec.ts`, `checkout.spec.ts` to use page objects (Copilot does this).
- Build skill 2; show invocation against captured response or generated test; do **not** refactor `tests/api/orders.spec.ts`.
- Try-it: extend the page-object skill with a second template variant.

## Acceptance criteria

- **Given** all three E2E specs after refactor, **when** run against `make dev`, **then** they pass and use the page objects.
- **Given** the page-object generator script, **when** invoked with `--name Products --url /products`, **then** it writes a valid TypeScript class to stdout that compiles.
- **Given** the contract-validator script, **when** run against a known-good response, **then** it reports zero violations; against a known-bad response, **then** it reports them precisely.
- **Given** both `SKILL.md` files, **when** the description is read in isolation, **then** a Copilot agent can determine when to invoke the skill without reading the body.
- **Given** the validator script header, **when** read, **then** library versions are pinned and noted.

## Source

[`implementation.md`](../implementation.md) — `04-agent-skills.md` section; the two skill specs and their assets; Implementation note #2.

## Dependencies

- Issues #01, #02, #05, #06, #07.
