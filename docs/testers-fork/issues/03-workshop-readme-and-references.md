---
title: "Author workshop README plus customization cheatsheet and troubleshooting reference"
labels: ["workshop", "core", "documentation", "copilot-coding-agent"]
---

## User story

**As a** QA engineer landing on the workshop for the first time,
**I want** a single README that orients me to scope, prerequisites, and module order, plus quick-reference docs I can return to later,
**so that** I can decide whether to invest the time and have a stable reference once I've finished.

## Description

Author three files:

1. `demo/walkthroughs/qa-testing-workshop/README.md` — workshop entry point.
2. `demo/walkthroughs/qa-testing-workshop/reference/customization-cheatsheet.md` — one-page reference, prose paragraphs grouped by customization type.
3. `demo/walkthroughs/qa-testing-workshop/reference/troubleshooting.md` — symptom-organized troubleshooting.

Follow the style and voice conventions in [`implementation.md`](../implementation.md) (direct, second-person, banned-words list, deliberate callouts).

README must include:

- One-paragraph overview.
- Audience: QA engineers, Copilot Enterprise, VS Code.
- Prerequisites checklist (VS Code, Copilot/Copilot Chat extensions, Enterprise license, Node 18+, npm, Make, Docker, repo fork, GitHub PAT for MCP module).
- Module index with one-sentence descriptions.
- Estimated time per module as ranges (e.g. "20–30 minutes").
- Repository structure overview pointing to where each customization type lives.
- Link to cheatsheet and troubleshooting.

Cheatsheet must include, per customization type: what it is (one line), when to reach for it, where it lives, one-line activation example.

Troubleshooting must be organized by symptom, not feature. Likely entries listed in [`implementation.md`](../implementation.md). Each entry: symptom → likely causes (priority order) → fix.

## Acceptance criteria

- **Given** a fresh QA engineer with prerequisites, **when** they read the README front to back, **then** they know whether they qualify, what they will build, and where to start, with no follow-up questions.
- **Given** a participant who finished the workshop, **when** they open the cheatsheet six months later, **then** they can answer "which customization type should I use for X?" without re-reading the modules.
- **Given** a stuck participant with one of the listed symptoms, **when** they open troubleshooting, **then** the first listed cause matches the most common root in practice.
- **Given** the prose, **when** scanned, **then** none of the banned words from the style guide appear.

## Source

[`implementation.md`](../implementation.md) — `README.md`, `customization-cheatsheet.md`, and `troubleshooting.md` sections; "Style and voice conventions".

## Dependencies

- Issue #01 (scaffolding).
- Best authored after modules are at least drafted, so links and time estimates are accurate. Can be opened in parallel and revised at the end.
