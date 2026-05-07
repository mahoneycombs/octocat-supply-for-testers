---
title: "[Nice-to-have] Facilitator guide for instructor-led delivery"
labels: ["workshop", "nice-to-have", "documentation", "copilot-coding-agent"]
---

> **Nice-to-have.** This issue is **not** part of [`implementation.md`](../implementation.md). The core workshop is self-paced; this guide enables an instructor-led variant. Treat it as additive; ship core issues first.

## User story

**As a** Copilot champion delivering the QA workshop live to my org,
**I want** a facilitator guide with timing, common-question answers, and per-module talking points,
**so that** I can run a half-day session confidently without re-reading every walkthrough beforehand.

## Description

Create `demo/walkthroughs/qa-testing-workshop/reference/facilitator-guide.md`. Include:

- Suggested half-day and full-day agendas with timing buffers.
- For each module: the one demo to run live (vs. let participants do), the most common stumbling block observed, and one talking point that lands well.
- A prep checklist for the day before (org-level `gh-aw`, GitHub PAT, Enterprise license verification, Docker pre-warmed).
- A "demo gone wrong" section: most likely failure modes during a live run and how to recover gracefully.
- A short script for the opening five minutes that frames why customization matters for testers specifically.

This is **not** a rewrite of the participant-facing modules. Keep it complementary.

## Acceptance criteria

- **Given** a first-time facilitator who has not run the workshop, **when** they read the guide once, **then** they can run a half-day session without rehearsal.
- **Given** the timing tables, **when** a real session is run against them, **then** modules complete within the buffered range for ≥80% of cohorts.
- **Given** the "demo gone wrong" section, **when** a known failure occurs (e.g. MCP PAT expired), **then** the recovery steps work without reaching for external docs.
- **Given** the file, **when** linked from the workshop README, **then** it's clearly marked as facilitator-only (not required reading for participants).

## Out of scope

- Translating modules into different formats (slides, video). See nice-to-have N06 for capstone module work.

## Dependencies

- All core module issues (#04–#12) merged so the facilitator guide can reference accurate timing and content.
