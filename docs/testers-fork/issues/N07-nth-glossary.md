---
title: "[Nice-to-have] Glossary of Copilot customization terminology"
labels: ["workshop", "nice-to-have", "documentation", "copilot-coding-agent"]
---

> **Nice-to-have.** This issue is **not** part of [`implementation.md`](../implementation.md). The cheatsheet covers concepts; this glossary covers vocabulary.

## User story

**As a** QA engineer who keeps tripping over overlapping terms (instruction vs. prompt vs. skill vs. agent),
**I want** a one-screen glossary with disambiguating sentences,
**so that** I can read team chat or GitHub docs without re-deriving definitions.

## Description

Create `demo/walkthroughs/qa-testing-workshop/reference/glossary.md`. Alphabetical, each entry one or two sentences. Cover at minimum:

- Agent (custom agent), Agent skill, Agentic workflow, `applyTo`, Hook (pre/post tool use, lifecycle), Instructions (root + path-scoped), Marketplace, MCP server, Plugin, Prompt file, `safe-outputs`, SKILL.md, Slash command, Tool allow-list, `workflow_run`.

Each entry must:

- Distinguish from the most easily confused neighbour (e.g. agent vs. prompt file).
- Point to the workshop module that demonstrates it (e.g. "see [Module 4](../04-agent-skills.md)").

Add a short opening note: "If a term is missing, open a PR — terms drift faster than this glossary."

## Acceptance criteria

- **Given** any pair of confusable terms (instruction/prompt, agent/prompt, skill/instruction), **when** their entries are read together, **then** the distinguishing sentence resolves the confusion.
- **Given** every entry, **when** it names a customization type covered by the workshop, **then** it links to the corresponding module.
- **Given** the glossary, **when** linked from the cheatsheet and troubleshooting, **then** navigation is consistent.
- **Given** the entries, **when** spot-checked against current GitHub docs, **then** the definitions match.

## Dependencies

- Best authored after all core module issues are merged.
