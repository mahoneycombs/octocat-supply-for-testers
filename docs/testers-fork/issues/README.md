# QA Copilot Customization Workshop — Issue Backlog

This directory contains GitHub issue drafts derived from [implementation.md](../implementation.md). Each `.md` file is intended to be created manually as a GitHub issue and then assigned to GitHub Copilot's coding agent.

Issues are written as BDD user stories. Each file's frontmatter contains the issue title and labels; copy the body into the issue description.

## Issue index

### Core implementation (from `implementation.md`)

| #   | File                                                                       | Module / area                            |
| --- | -------------------------------------------------------------------------- | ---------------------------------------- |
| 01  | [01-workshop-scaffolding.md](./01-workshop-scaffolding.md)                 | Directory scaffolding                    |
| 02  | [02-test-suite-bootstrap.md](./02-test-suite-bootstrap.md)                 | Playwright config + `tests/` bootstrap   |
| 03  | [03-workshop-readme-and-references.md](./03-workshop-readme-and-references.md) | Workshop README + reference files    |
| 04  | [04-module-00-setup.md](./04-module-00-setup.md)                           | Module 0 — Setup                         |
| 05  | [05-module-01-instructions.md](./05-module-01-instructions.md)             | Module 1 — Custom Instructions           |
| 06  | [06-module-02-prompt-files.md](./06-module-02-prompt-files.md)             | Module 2 — Prompt Files                  |
| 07  | [07-module-03-custom-agents.md](./07-module-03-custom-agents.md)           | Module 3 — Custom Agents                 |
| 08  | [08-module-04-agent-skills.md](./08-module-04-agent-skills.md)             | Module 4 — Agent Skills                  |
| 09  | [09-module-05-mcp-servers.md](./09-module-05-mcp-servers.md)               | Module 5 — MCP Servers                   |
| 10  | [10-module-06-agentic-workflows.md](./10-module-06-agentic-workflows.md)   | Module 6 — Agentic Workflows             |
| 11  | [11-module-07-hooks.md](./11-module-07-hooks.md)                           | Module 7 — Hooks                         |
| 12  | [12-module-08-plugins.md](./12-module-08-plugins.md)                       | Module 8 — Plugins                       |

### Nice-to-have (extensions beyond `implementation.md`)

These are clearly marked as additive. Skip any that don't fit your event.

| #   | File                                                                       | Theme                                    |
| --- | -------------------------------------------------------------------------- | ---------------------------------------- |
| N01 | [N01-nth-facilitator-guide.md](./N01-nth-facilitator-guide.md)             | Facilitator guide for instructor-led runs |
| N02 | [N02-nth-reference-outputs.md](./N02-nth-reference-outputs.md)             | Reference example outputs for unsticking |
| N03 | [N03-nth-customization-decision-tree.md](./N03-nth-customization-decision-tree.md) | Visual decision tree                |
| N04 | [N04-nth-bug-report-library.md](./N04-nth-bug-report-library.md)           | Sample bug reports for `qa-bug-reproducer` |
| N05 | [N05-nth-broken-test-fixtures.md](./N05-nth-broken-test-fixtures.md)       | Pre-seeded broken tests for hook + workflow demos |
| N06 | [N06-nth-capstone-module.md](./N06-nth-capstone-module.md)                 | Capstone: combine all customizations     |
| N07 | [N07-nth-glossary.md](./N07-nth-glossary.md)                               | Glossary of Copilot customization terms  |
| N08 | [N08-nth-self-assessments.md](./N08-nth-self-assessments.md)               | Per-module self-assessment checkpoints   |

## Label legend

- `workshop` — applies to every issue in this backlog.
- `core` — required by `implementation.md`.
- `nice-to-have` — additive; not required.
- `documentation` — primarily prose authoring.
- `scaffolding` — directory or config bootstrap, no module content.
- `module:<n>` — pertains to that walkthrough module.
- `customization:<type>` — pertains to that customization surface (instructions, prompts, agents, skills, mcp, workflows, hooks, plugins).
- `copilot-coding-agent` — intended to be assigned to GitHub Copilot.

## Suggested execution order

1. Issues 01 → 02 → 03 first (scaffolding before content).
2. Then modules 04 → 12 in numeric order (each module's "stand alone" promise still holds, but the linking between them is easier when authored sequentially).
3. Nice-to-have issues last, picked à la carte.
