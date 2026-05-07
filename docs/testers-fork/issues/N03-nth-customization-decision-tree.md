---
title: "[Nice-to-have] Visual decision tree: which Copilot customization should I use?"
labels: ["workshop", "nice-to-have", "documentation", "copilot-coding-agent"]
---

> **Nice-to-have.** This issue is **not** part of [`implementation.md`](../implementation.md). The cheatsheet (Issue #03) covers the same ground in prose; this is a visual companion.

## User story

**As a** QA engineer back at work three weeks after the workshop,
**I want** a one-glance decision tree that maps "I want to do X" to the right customization type,
**so that** I pick instructions vs. prompt vs. agent vs. skill correctly without re-reading the cheatsheet.

## Description

Create `demo/walkthroughs/qa-testing-workshop/reference/customization-decision-tree.md`. Use a Mermaid `flowchart` so it renders inline on GitHub. Include:

- A root question ("What are you trying to do?") branching into common QA tasks: enforce conventions, scaffold a test, run a multi-turn session, generate a templated artifact, give Copilot a runtime tool, automate triage, gate test edits, share a toolkit.
- Each leaf points to one customization type and the file path where the example lives.
- A short prose section under the diagram explaining the two or three most common mis-picks (e.g. "people reach for prompt files when they want instructions").

Keep the diagram small enough to fit one screen on a 13" laptop.

## Acceptance criteria

- **Given** the Mermaid block, **when** rendered on GitHub, **then** it displays without errors.
- **Given** any of the example QA tasks named in the workshop, **when** traced through the tree, **then** the leaf points to the customization type the workshop actually demonstrates for that task.
- **Given** the prose mis-picks section, **when** reviewed by an experienced Copilot user, **then** the mis-picks reflect real patterns (not invented ones).
- **Given** the file, **when** linked from the cheatsheet, **then** participants can navigate cheatsheet ↔ tree easily.

## Dependencies

- Issues #03 (cheatsheet) and all module issues merged so the leaves reference real paths.
