---
title: "Author Module 3 — Custom Agents (qa-engineer, qa-bug-reproducer)"
labels: ["workshop", "core", "documentation", "module:3", "customization:agents", "copilot-coding-agent"]
---

## User story

**As a** QA engineer who once watched Copilot refactor production code while I was drafting a test,
**I want** a session-scoped QA persona with a tool allow-list that keeps edits inside `tests/`,
**so that** my test-writing session stays inside its lane and produces predictable changes.

## Description

Author `demo/walkthroughs/qa-testing-workshop/03-custom-agents.md` and create two agent files:

1. `.github/agents/qa-engineer.agent.md` — frontmatter with `name`, `description`, `tools` limited to read-only filesystem and test-running tools, `model` preference. Body: persona text — modifies only `tests/`, prefers Playwright, asks before adding dependencies, ends every test with a passing run before declaring done. (Schema: see [`implementation.md`](../implementation.md) example; verify against current GitHub agent format before locking in.)
2. `.github/agents/qa-bug-reproducer.agent.md` — frontmatter only filled in; participants don't use this agent in the exercise. Persona: takes a bug report, produces a failing test that reproduces the bug, no fix attempts, no production code edits, ends with the failing test output as evidence.

Walkthrough must:

- Establish the difference between agents (session-scoped persona), prompt files (one-shot), and instructions (always-on).
- Step 1: build `qa-engineer.agent.md`.
- Step 2: open the Agents dropdown in VS Code Copilot Chat, select `qa-engineer`, confirm visible.
- Step 3: inside that agent session, build `tests/e2e/checkout.spec.ts` across multiple turns (plan → draft → refine).
- Step 4: build `qa-bug-reproducer.agent.md` (frontmatter shown, body short).
- Verification: `tests/e2e/checkout.spec.ts` runs and passes.
- Try-it: participant modifies `qa-engineer.agent.md` to add a handoff to a hypothetical reviewer agent; brief discussion of handoffs.

## Acceptance criteria

- **Given** the `qa-engineer` agent loaded in VS Code, **when** Copilot Chat is asked to edit a production source file, **then** the tool allow-list prevents the edit (or the agent declines and explains).
- **Given** Module 3 followed end to end inside the `qa-engineer` session, **when** `tests/e2e/checkout.spec.ts` is run against `make dev`, **then** it passes.
- **Given** both agent files, **when** validated against the current GitHub agent schema, **then** frontmatter parses without errors.
- **Given** the walkthrough, **when** read, **then** the distinction between agents, prompts, and instructions is stated in two or three sentences.

## Source

[`implementation.md`](../implementation.md) — `03-custom-agents.md` section; `qa-engineer.agent.md` and `qa-bug-reproducer.agent.md` specs; Implementation note #1 (verify schemas).

## Dependencies

- Issues #01, #02, #05, #06.
