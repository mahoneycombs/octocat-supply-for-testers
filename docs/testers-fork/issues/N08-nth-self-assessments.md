---
title: "[Nice-to-have] Per-module self-assessment checkpoints"
labels: ["workshop", "nice-to-have", "documentation", "copilot-coding-agent"]
---

> **Nice-to-have.** This issue is **not** part of [`implementation.md`](../implementation.md). Each core module already has a "What you built" recap and a "Try it yourself" exercise; this adds a brief self-assessment between them.

## User story

**As a** self-paced participant who tends to skim,
**I want** a short self-assessment at the end of each module,
**so that** I notice when I missed a concept before it compounds in the next module.

## Description

Add a self-assessment block to **the end of** each module file (`01-instructions.md` through `08-plugins.md`). Three questions per module:

- One concept question (no Googling).
- One file-location question (where does X live?).
- One application question ("if you wanted to do Y, which customization type and why?").

Provide answers in collapsed `<details>` blocks below each question. Keep the section under 200 words.

This issue is a **single follow-up edit** across all eight module files — not new files. It must not change anything in the existing module steps.

## Acceptance criteria

- **Given** each module file, **when** rendered, **then** a "Check yourself" section appears between "What you built" and "Next" with exactly three questions.
- **Given** every answer, **when** verified, **then** it agrees with the module's body text and the cheatsheet/glossary.
- **Given** the diff against `main`, **when** reviewed, **then** no existing prose in the modules is modified outside the new section.
- **Given** the answers, **when** rendered on GitHub, **then** the `<details>` collapses correctly and the questions are visible by default.

## Dependencies

- All core module issues (#05–#12) merged.
