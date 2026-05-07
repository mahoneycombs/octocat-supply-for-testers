---
title: "Author Module 5 — MCP Servers (GitHub MCP, optional docs MCP, visual.spec.ts)"
labels: ["workshop", "core", "documentation", "module:5", "customization:mcp", "copilot-coding-agent"]
---

## User story

**As a** QA engineer authoring tests for an app I can't fully see from inside the editor,
**I want** Copilot to drive a real browser via Playwright MCP and check CI state via GitHub MCP,
**so that** I can author and verify tests in one chat session without alt-tabbing to the browser or the GitHub UI.

## Description

Author `demo/walkthroughs/qa-testing-workshop/05-mcp-servers.md` and modify `.vscode/mcp.json`. Pull the current GitHub MCP server URL and config shape from official GitHub docs at implementation time — do **not** copy from this design doc.

Walkthrough steps:

- Concept framing: MCP servers are tools available to Copilot at runtime, configured per-workspace in `.vscode/mcp.json`.
- Step 1: open `.vscode/mcp.json`, note Playwright MCP is already there.
- Step 2: add the GitHub MCP server. Walk PAT setup. Flag that Copilot Enterprise admins may have org-level MCP configs that override workspace ones.
- Step 3: optional third MCP — only if the implementing author can identify a currently maintained docs MCP server. **Skip the step entirely rather than recommend a stale server.**
- Step 4: in Copilot Chat, ask the agent to open the products page and describe what it sees; confirm the Playwright MCP tool call appears in the chat trace.
- Step 5: build `tests/e2e/visual.spec.ts` with the agent using Playwright MCP for live inspection. Snapshot one page. (This is also where `qa-visual-regression.prompt.md` from Module 2 finally gets exercised.)
- Step 6: ask the agent to check whether the most recent push has green CI; confirm GitHub MCP tool call.
- Try-it: configure one additional MCP server of the participant's choosing.

## Acceptance criteria

- **Given** the modified `.vscode/mcp.json`, **when** parsed, **then** it is valid JSON, retains the Playwright entry, and adds a GitHub entry whose shape matches current official docs.
- **Given** the workshop run with a valid PAT, **when** the participant asks the agent to check CI, **then** the GitHub MCP tool call returns a real result.
- **Given** Step 3, **when** no maintained docs MCP exists at implementation time, **then** the step is omitted with a one-line note (no broken recommendation).
- **Given** `tests/e2e/visual.spec.ts`, **when** run against `make dev`, **then** it captures at least one snapshot successfully.
- **Given** the prerequisites callout, **when** read, **then** it points participants to the GitHub PAT setup and notes the org-policy caveat.

## Source

[`implementation.md`](../implementation.md) — `05-mcp-servers.md` section; `.vscode/mcp.json` modify spec; Implementation note #1.

## Dependencies

- Issues #01, #02, #06 (visual regression prompt).
