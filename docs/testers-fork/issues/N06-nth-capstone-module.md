---
title: "[Nice-to-have] Capstone module — combine all customizations on one realistic ticket"
labels: ["workshop", "nice-to-have", "documentation", "copilot-coding-agent"]
---

> **Nice-to-have.** This issue is **not** part of [`implementation.md`](../implementation.md). The core workshop ends at Module 8 (Plugins). This adds an optional Module 9 for participants who want to feel the cumulative effect.

## User story

**As a** participant who completed Modules 0–8 individually,
**I want** one capstone exercise that uses every customization together against a single realistic ticket,
**so that** I see the cumulative effect (not just each piece in isolation) before I take this back to my team.

## Description

Author `demo/walkthroughs/qa-testing-workshop/09-capstone.md` plus a single seeded ticket under `demo/walkthroughs/qa-testing-workshop/reference/capstone-ticket.md`.

The capstone walks participants through a realistic ticket — for example, "add E2E coverage for guest checkout" — and shows every customization activating in turn:

- Path-scoped instructions shape the spec on first draft.
- A prompt file scaffolds the initial structure.
- The `qa-engineer` agent owns the multi-turn refinement.
- The page-object skill produces a `GuestCheckoutPage`.
- Playwright MCP verifies the live UI; GitHub MCP confirms CI is green at the end.
- The pre-commit hook intercepts a pasted `.only`.
- The agentic workflow triages a deliberately seeded failure on PR.
- The plugin install reminds participants this whole stack is portable.

Add a closing reflection prompt (three questions) and a "what would you build next?" exercise.

Keep this clearly **optional**. State at the top that it requires Modules 0–8 complete and adds 60–90 minutes.

## Acceptance criteria

- **Given** Modules 0–8 complete, **when** the capstone is followed end to end, **then** every customization fires at least once and the ticket's acceptance criteria are met by the produced tests.
- **Given** the capstone walkthrough, **when** scanned, **then** each customization activation is annotated with the module that introduced it.
- **Given** the seeded ticket, **when** read, **then** it reflects a realistic QA backlog item (no toy bugs).
- **Given** the closing reflection, **when** answered honestly, **then** the answers surface at least one customization the participant intends to ship to their team.

## Dependencies

- All core module issues (#04–#12) and ideally N02 (reference outputs) merged.
