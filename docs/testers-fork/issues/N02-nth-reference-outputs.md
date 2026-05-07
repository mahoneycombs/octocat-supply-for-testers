---
title: "[Nice-to-have] Reference example outputs for unsticking participants"
labels: ["workshop", "nice-to-have", "documentation", "copilot-coding-agent"]
---

> **Nice-to-have.** This issue is **not** part of [`implementation.md`](../implementation.md). [`implementation.md`](../implementation.md) Implementation note #7 explicitly permits reference test files **only** under `reference/example-outputs/` and clearly labelled. This issue follows that note.

## User story

**As a** participant who got blocked generating a spec at Module 4 because my Copilot output diverged badly,
**I want** to compare against a reference output that's clearly labelled as one possible answer,
**so that** I can keep moving without abandoning the workshop or copying a "canonical" answer.

## Description

Create `demo/walkthroughs/qa-testing-workshop/reference/example-outputs/` and populate one reference output per module that produces a test artifact:

- `module-01-products.spec.ts`
- `module-02-cart.spec.ts`
- `module-02-orders.spec.ts`
- `module-03-checkout.spec.ts`
- `module-04-pages-ProductsPage.ts` (and the matching refactored spec)
- `module-05-visual.spec.ts`

Each file's header comment must explicitly state:

> This is **one possible Copilot output** following the module's instructions, captured at <date>. Your output will differ. Use this only if you're stuck. Do not copy unmodified.

Add a `README.md` in `example-outputs/` summarizing the same warning and pointing back to the modules.

## Acceptance criteria

- **Given** every reference output, **when** opened, **then** the header comment with the warning is present and dated.
- **Given** the `example-outputs/README.md`, **when** linked from each module's "Try it yourself" section as a fallback, **then** the link is the only reference to it from the participant flow (don't link from the main steps).
- **Given** every reference test, **when** run against `make dev`, **then** it passes.
- **Given** the directory, **when** a reviewer scans the workshop, **then** it's obvious these files are reference, not starter code.

## Dependencies

- Core module issues #05, #06, #07, #08, #09 merged.
