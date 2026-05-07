---
title: "Bootstrap the Playwright test suite (config + tests/README)"
labels: ["workshop", "core", "scaffolding", "module:0", "copilot-coding-agent"]
---

## User story

**As a** workshop participant arriving at Module 0,
**I want** a working `tests/` folder with `playwright.config.ts` already wired to the repo's existing dev servers,
**so that** I can run a smoke test before the workshop begins and trust the toolchain is healthy before adding generated specs.

## Description

Create `tests/playwright.config.ts` and `tests/README.md` only. Do **not** create any `.spec.ts` files — those are produced by participants in later modules.

Configuration requirements (from [`implementation.md`](../implementation.md) → "Test suite plan"):

- API tests target the existing Express server on port 3000.
- E2E tests target the frontend on port 5173.
- TypeScript defaults from `npm init playwright@latest`.
- Test directory is `tests/`.
- Configure `webServer` (or document the requirement to run `make dev` first) so participants get green output.

`tests/README.md` should be a short orientation: what lives where, how to run E2E vs API, where to find the walkthrough, and a "the tests are generated, not pre-written" note pointing to `demo/walkthroughs/qa-testing-workshop/`.

## Acceptance criteria

- **Given** `make dev` is running, **when** I run `npx playwright test --list` from the repo root, **then** Playwright resolves the config without errors and lists zero tests (because none exist yet).
- **Given** the new `tests/README.md`, **when** read by a fresh participant, **then** they can locate the workshop walkthrough and the dev-server prerequisite within one minute.
- **Given** the bootstrap PR, **when** diffed, **then** no `.spec.ts` files are added.
- **Given** the existing repo, **when** the PR merges, **then** no other Playwright config or test infra is broken.

## Source

[`implementation.md`](../implementation.md) — "Test suite plan", note about `playwright.config.ts` ports.

## Dependencies

- Issue #01 (scaffolding) must be merged first.
