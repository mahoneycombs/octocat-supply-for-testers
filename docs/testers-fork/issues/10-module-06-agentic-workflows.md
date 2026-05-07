---
title: "Author Module 6 — Agentic Workflows (qa-test-triage)"
labels: ["workshop", "core", "documentation", "module:6", "customization:workflows", "copilot-coding-agent"]
---

## User story

**As a** QA on-call rotating through CI failures,
**I want** an agent to read failed test logs, classify the failure (selector drift, network flake, real regression), and post a triage comment on the PR,
**so that** I see a starting hypothesis before I open the logs myself.

## Description

Author `demo/walkthroughs/qa-testing-workshop/06-agentic-workflows.md` and create `.github/workflows/qa-test-triage.md` (note: `.md`, not `.yml`).

Workflow file requirements (from [`implementation.md`](../implementation.md)):

- Trigger: `workflow_run` on the CI workflow's failure.
- Permissions: read on `contents` and `pull-requests`, write on `issues`.
- `safe-outputs`: constrain the agent to a single PR comment, no other side effects.
- Body in natural language: read failure logs → classify → post comment with classification and suggested next step.

Walkthrough steps:

- Concept framing: agentic workflows are Markdown in `.github/workflows/` that describe what an agent does when triggered. They run in GitHub Actions infrastructure under specific permissions.
- Prerequisites callout at the top: requires org-level `gh-aw` extension and Actions permissions; link participants to the official setup guide rather than duplicating it.
- Step 1: build the workflow file.
- Step 2: commit and push; trigger by pushing a deliberately broken test.
- Step 3: watch the workflow run; inspect the comment it posts.
- Step 4: inspect logs with `gh aw logs`.
- Try-it: extend the workflow to also label the PR.

## Acceptance criteria

- **Given** a participant with `gh-aw` and the right org permissions, **when** they follow the steps, **then** the workflow runs against a deliberately broken test and posts a single PR comment with a classification.
- **Given** the workflow frontmatter, **when** parsed by `gh-aw`, **then** triggers, permissions, and `safe-outputs` validate.
- **Given** the prerequisites callout, **when** read, **then** participants without org access self-identify and stop before pushing untriggerable changes.
- **Given** the comment posted by the workflow, **when** read, **then** the classification matches one of the three categories named in the body (selector drift, network flake, real regression).

## Source

[`implementation.md`](../implementation.md) — `06-agentic-workflows.md` section; `.github/workflows/qa-test-triage.md` spec; the IMPORTANT callout about org-level setup.

## Dependencies

- Issues #01, #02. Most easily authored after Module 5 since it builds on prior CI assumptions.
