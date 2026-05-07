---
title: "Author Module 8 — Plugins (qa-toolkit plugin.json)"
labels: ["workshop", "core", "documentation", "module:8", "customization:plugins", "copilot-coding-agent"]
---

## User story

**As a** QA tech lead with multiple teams duplicating the same Copilot customizations,
**I want** to package the workshop's agents, skills, hooks, and MCP configs into one installable plugin,
**so that** new teams get the full toolkit with one install command instead of cloning files.

## Description

Author `demo/walkthroughs/qa-testing-workshop/08-plugins.md` and create `plugins/qa-toolkit/.github/plugin.json` referencing existing artifacts in `.github/` (don't duplicate them).

Verify path resolution works with the current plugin format. If relative-up paths don't resolve, fall back to moving the customizations under `plugins/qa-toolkit/` and symlinking or duplicating, and document the choice in the walkthrough.

Walkthrough steps:

- Concept: a plugin packages any combination of agents, skills, hooks, MCP server configs, and slash commands into one installable unit declared in `plugin.json`. Distributed via marketplaces (Git repos with a `marketplace.json`).
- Step 1: build `plugins/qa-toolkit/.github/plugin.json`.
- Step 2: walk through the marketplace concept; show what `marketplace.json` looks like; link to where to publish; do not have participants stand up a marketplace inside workshop time.
- Step 3: demonstrate the install command (`copilot plugin marketplace add <repo>` then `copilot plugin install qa-toolkit@<marketplace>`) as instructor-led — phrase it as "here's what install looks like once published."
- Try-it: participants modify `plugin.json` to add a custom agent of their own.
- Closing section: short "what now" with three concrete next steps for taking this back to their team.

## Acceptance criteria

- **Given** `plugin.json`, **when** parsed against the current plugin manifest schema, **then** it validates and the referenced source paths resolve.
- **Given** an attempt to install the plugin from a local path or fork, **when** executed by an experienced reviewer (not the participant), **then** the install succeeds and exposes the agents, skills, hooks, and MCP configs.
- **Given** the closing "what now," **when** read, **then** every next step is concrete (one named action, not a generic exhortation).
- **Given** the implementing author's PR description, **when** reviewed, **then** it states whether relative-up paths worked or whether the duplicate-under-plugin fallback was used.

## Source

[`implementation.md`](../implementation.md) — `08-plugins.md` section; `plugin.json` spec; Implementation note #1.

## Dependencies

- Issues #07 (agents), #08 (skills), #09 (mcp), #11 (hooks) all merged. This is the last core issue.
