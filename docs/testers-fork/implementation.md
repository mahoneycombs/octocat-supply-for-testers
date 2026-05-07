# OctoCAT Supply — QA Copilot Customization Workshop: Design and Build Plan

## What this document is

A specification for building a self-paced GitHub Copilot customization workshop on top of the existing `Azure-Samples/octocat-supply` fork. The workshop targets QA testers with Copilot Enterprise, working in VS Code, and demonstrates every Copilot customization type through a fresh test suite covering E2E, UI, API, and contract testing.

This is a build plan, not the workshop content itself. An implementing LLM should use this document to scaffold the directory structure, create each file outlined below, write the prose for each module's walkthrough, and wire the example artifacts into the repository. Where the document says "outline," produce real content following that structure. Where it says "stub," produce a working file that participants can run.

## Workshop goals

Participants finish the workshop able to:

1. Recognize when each Copilot customization type is the right tool, and know where each one lives in the repository.
2. Write Copilot instructions that meaningfully change generated test code.
3. Build a working set of testing-focused customizations they can copy into their own repos.
4. Package customizations into a plugin so a QA org can share a single installable toolkit.

Participants are QA engineers. They write E2E, UI, API, and contract tests; they generally do not write unit tests. Examples and scenarios should reflect this. Avoid framing exercises around code-coverage of internal functions or unit-test patterns like AAA.

## Prerequisites participants need

- VS Code with the GitHub Copilot and GitHub Copilot Chat extensions.
- A Copilot Enterprise license assigned through their org.
- Node.js 18+, npm, Make, Docker.
- A fork of the workshop repository.
- For the MCP module: a GitHub Personal Access Token (fine-grained, read-only on repo metadata is enough).

State this explicitly in the workshop entry README so participants can verify before starting.

## Repository changes at a glance

Everything net-new for this workshop sits in two places: a new walkthrough folder under `demo/walkthroughs/qa-testing-workshop/`, and additions to `.github/` prefixed with `qa-` to avoid collisions with existing demo content. A fresh `tests/` folder at the repo root holds the test suite participants build module by module.

```
octocat-supply/
├── .github/
│   ├── copilot-instructions.md            # ADD: section appended for QA conventions
│   ├── instructions/
│   │   └── qa-playwright.instructions.md  # NEW: path-scoped to tests/**
│   ├── prompts/
│   │   ├── qa-scaffold-e2e.prompt.md      # NEW
│   │   ├── qa-api-contract-test.prompt.md # NEW
│   │   └── qa-visual-regression.prompt.md # NEW
│   ├── agents/
│   │   ├── qa-engineer.agent.md           # NEW
│   │   └── qa-bug-reproducer.agent.md     # NEW
│   ├── skills/
│   │   ├── qa-playwright-page-object/
│   │   │   ├── SKILL.md                   # NEW
│   │   │   ├── templates/
│   │   │   │   └── page-object.template.ts
│   │   │   └── scripts/
│   │   │       └── generate-page-object.mjs
│   │   └── qa-api-contract-validator/
│   │       ├── SKILL.md                   # NEW
│   │       └── scripts/
│   │           └── validate-against-openapi.mjs
│   ├── workflows/
│   │   └── qa-test-triage.md              # NEW: agentic workflow (.md, not .yml)
│   └── hooks/
│       ├── qa-hooks.json                  # NEW
│       └── scripts/
│           ├── block-test-skip.sh
│           └── lint-after-test-edit.sh
├── .vscode/
│   └── mcp.json                           # MODIFY: add github + (optional) docs MCP server
├── plugins/
│   └── qa-toolkit/
│       └── .github/
│           └── plugin.json                # NEW: bundles the above for distribution
├── tests/
│   ├── README.md                          # NEW: test suite orientation
│   ├── playwright.config.ts               # NEW
│   ├── e2e/
│   │   ├── products.spec.ts               # built in Module 1
│   │   ├── cart.spec.ts                   # built in Module 2
│   │   ├── checkout.spec.ts               # built in Module 3
│   │   ├── visual.spec.ts                 # built in Module 5
│   │   └── pages/                         # built in Module 4
│   │       ├── ProductsPage.ts
│   │       ├── CartPage.ts
│   │       └── CheckoutPage.ts
│   └── api/
│       └── orders.spec.ts                 # built in Module 2
└── demo/
    └── walkthroughs/
        └── qa-testing-workshop/
            ├── README.md                  # entry point and module index
            ├── 00-setup.md
            ├── 01-instructions.md
            ├── 02-prompt-files.md
            ├── 03-custom-agents.md
            ├── 04-agent-skills.md
            ├── 05-mcp-servers.md
            ├── 06-agentic-workflows.md
            ├── 07-hooks.md
            ├── 08-plugins.md
            └── reference/
                ├── customization-cheatsheet.md
                └── troubleshooting.md
```

The implementing LLM should not delete or restructure any existing repository content. The base repo's existing `demo/walkthroughs/` content stays intact; this workshop sits beside it.

## Style and voice conventions

These conventions apply to every walkthrough file. Apply them consistently; the user will polish, but the starting point should already feel like a senior practitioner wrote it.

**Voice.** Direct, second-person, present tense. "Open the file," "run the command," "look at the output." Avoid hedging ("you might want to consider"). Avoid filler ("it's worth noting that," "as we mentioned"). Don't explain what Copilot is — participants have Enterprise licenses. Do explain what each customization type does, but in two or three sentences, not a paragraph.

**Words to avoid.** Delve, leverage, robust, comprehensive, seamless, holistic, journey, unleash, empower, supercharge. Em-dashes are a tell — use them sparingly. Avoid the "not just X but Y" construction. Avoid opening sentences with "Imagine" or "Picture this."

**Callouts.** Use GitHub-flavored Markdown alerts. Five types are available; use them deliberately:

```markdown
> [!NOTE]
> Information that adds context but isn't required to complete the step.

> [!TIP]
> A shortcut, recommended pattern, or thing experienced users do.

> [!IMPORTANT]
> A step that's easy to miss but breaks things if skipped.

> [!WARNING]
> A non-destructive thing that's likely to confuse or produce wrong output.

> [!CAUTION]
> A destructive or irreversible action.
```

Don't stack callouts. Don't put a callout in every section out of habit. If a module has more than four callouts, most of them aren't earning their place.

**Code blocks.** Always include the language hint. Annotate non-obvious lines with end-of-line comments rather than separate explanation paragraphs. When showing Copilot Chat input, use ```text or ```markdown and prefix with the agent or slash command being used:

```text
@workspace /qa-scaffold-e2e add-to-cart flow with happy path and out-of-stock case
```

**Structural conventions per module.** Every module file uses this skeleton:

1. **Title** — H1, the customization type and a short noun phrase, e.g. "Module 3 — Custom Agents: A Persistent QA Persona"
2. **What you'll do** — three to five bullets, outcome-focused
3. **Why this customization type exists** — two short paragraphs grounding the QA pain point
4. **Where the files live** — the path(s), one sentence on each
5. **Steps** — numbered, each step has a goal sentence, the action, and a verification
6. **Try it yourself** — one open-ended exercise with no provided solution
7. **What you built** — bullet list of files added/modified in this module
8. **Next** — link to the next module

Keep modules to 600–1200 words of prose plus code. If a module would run longer, the customization type is being over-explained.

## Test suite plan

The test suite is built across modules so each module produces tangible, runnable tests. Participants can verify their work each step. The arc:

- **Module 1 (Instructions)** produces `tests/e2e/products.spec.ts` — a basic product-listing E2E test. This is the test where instructions visibly shape selector strategy and assertion patterns.
- **Module 2 (Prompt files)** produces `tests/e2e/cart.spec.ts` (via `/qa-scaffold-e2e`) and `tests/api/orders.spec.ts` (via `/qa-api-contract-test`). Two prompt files, two tests.
- **Module 3 (Custom agents)** produces `tests/e2e/checkout.spec.ts` working entirely inside the `qa-engineer` agent session. Demonstrates how the persona keeps Copilot scoped to test files only.
- **Module 4 (Skills)** produces `tests/e2e/pages/*.ts` and refactors the previous specs to use them. The `qa-playwright-page-object` skill is used to generate each page object; `qa-api-contract-validator` is invoked against the OpenAPI spec.
- **Module 5 (MCP)** doesn't necessarily produce a new test on its own. Instead, participants use the Playwright MCP server live during authoring to inspect a real browser, and the GitHub MCP server to check whether their last test PR is green. A small `tests/e2e/visual.spec.ts` is added here to give MCP something concrete to interact with.
- **Module 6 (Agentic workflows)** produces `.github/workflows/qa-test-triage.md`. This module is hands-on: participants commit the workflow, force a test failure, and watch the workflow open a triage comment.
- **Module 7 (Hooks)** produces `.github/hooks/qa-hooks.json` and its helper scripts. Participants test the hooks by attempting to commit a `.only` and watching the hook block it.
- **Module 8 (Plugins)** doesn't produce new tests. It produces `plugins/qa-toolkit/.github/plugin.json` and walks through publishing it to a marketplace fork.

The test suite uses Playwright for both E2E and API tests. This keeps the toolchain narrow and matches the existing repo's investment in Playwright. The implementing LLM should configure `playwright.config.ts` to run the API tests against the existing Express server on port 3000 and the E2E tests against the frontend on port 5173.

> [!NOTE]
> The implementing LLM should not write the *contents* of the test files in advance. Each module's walkthrough has participants generate the tests using Copilot. The implementing LLM should write each module's walkthrough such that following its steps produces the test files described. After the implementing LLM finishes the walkthroughs, they should run through Module 1 themselves to confirm a fresh participant can produce `tests/e2e/products.spec.ts` from the steps as written. The same end-to-end check is unnecessary for later modules.

## Module specifications

Each section below is the brief for one module file in `demo/walkthroughs/qa-testing-workshop/`. Treat the bullets as required content. The implementing LLM writes the prose, callouts, and code samples to fit.

### `README.md` — Workshop entry point

- One-paragraph overview of what the workshop covers.
- Audience statement: QA engineers, Copilot Enterprise, VS Code.
- Prerequisites checklist.
- Module index with one-sentence descriptions.
- Estimated time per module (the user prefers not to over-specify pacing; use ranges like "20–30 minutes").
- Repository structure overview pointing to where each customization type lives.
- Link to `reference/customization-cheatsheet.md` and `reference/troubleshooting.md`.

### `00-setup.md` — Setup and orientation

- Clone the fork, run `make install`, run `make dev`, verify both ports respond.
- Verify Copilot Chat works in VS Code; confirm Enterprise tier in account settings.
- Install Playwright: walk through `npm init playwright@latest` in a `tests/` folder, accepting TypeScript defaults but pointing the test directory at `tests/`.
- Verify the existing Playwright MCP server is configured by opening `.vscode/mcp.json`.
- Tour of where each customization type lives (the directory structure from this design doc, condensed).
- Final checkpoint: a one-line "you're ready when..." confirmation.

### `01-instructions.md` — Custom Instructions

- Pain point: Copilot generates Playwright tests with brittle selectors, missing waits, and inconsistent assertion styles.
- Two scopes covered: root `.github/copilot-instructions.md` (project-wide, includes one or two QA-relevant rules added to the existing file) and `.github/instructions/qa-playwright.instructions.md` (path-scoped via `applyTo` frontmatter to `tests/**/*.spec.ts`).
- Step 1: Open `.github/copilot-instructions.md`, read what's already there, add a small QA section. Show the diff.
- Step 2: Create `.github/instructions/qa-playwright.instructions.md`. Walk through the YAML frontmatter (`applyTo`, `description`). Spell out the Playwright conventions: selector priority order (role > test-id > text > CSS), required `await expect().toHaveURL` after navigation, no `page.waitForTimeout`, fixtures over `beforeAll`.
- Step 3: Without instructions context, ask Copilot to write `tests/e2e/products.spec.ts` for browsing the products page. Save the output as a comment in the walkthrough or screenshot it.
- Step 4: With instructions in place, ask the same question. Compare. The participant produces the actual file from the second response.
- Verification: run the test against `make dev`. It should pass.
- Try-it: have participants add one more rule to the path-scoped file (their choice) and observe the next generated test.

### `02-prompt-files.md` — Prompt Files

- Pain point: testers retype variations of the same scaffolding request all day.
- Step 1: Create `.github/prompts/qa-scaffold-e2e.prompt.md`. Walk through the frontmatter (`description`, `mode`, optional `tools`). Body: a parameterized scaffold for a Playwright E2E test that takes a flow name and produces happy path, validation error, and network failure cases. Reference the path-scoped instructions file from Module 1 with a Markdown link.
- Step 2: Invoke `/qa-scaffold-e2e add-to-cart` in chat. Save output as `tests/e2e/cart.spec.ts`. Run it.
- Step 3: Create `.github/prompts/qa-api-contract-test.prompt.md`. Body should produce a Playwright API test that validates response shape against the OpenAPI spec at `api/openapi.yaml` (or wherever it lives in the repo — the implementing LLM should verify the actual path).
- Step 4: Invoke `/qa-api-contract-test orders` in chat. Save as `tests/api/orders.spec.ts`. Run it.
- Step 5: Create `.github/prompts/qa-visual-regression.prompt.md` as a smaller third example to reinforce the pattern. Don't have participants run it yet — that's Module 5.
- Try-it: have participants write a fourth prompt of their own design.

### `03-custom-agents.md` — Custom Agents

- Pain point: in a normal Copilot session, Copilot will happily refactor production code while you're trying to write tests.
- Concept: agents are session-scoped personas with their own tool allow-lists, model preferences, and instruction context. Different from prompt files (one-shot) and instructions (always-on).
- Step 1: Create `.github/agents/qa-engineer.agent.md`. Frontmatter: `name`, `description`, `tools` limited to read-only filesystem and test-running tools, `model` preference (mention that Enterprise gives them choice but recommend a specific one for the exercise). Body: persona text describing how the agent should think — only modifies files under `tests/`, prefers Playwright over other tools, asks before adding new dependencies.
- Step 2: Open the Agents dropdown in VS Code Copilot Chat, select `qa-engineer`. Confirm it appears.
- Step 3: Inside that agent session, build `tests/e2e/checkout.spec.ts`. Walk through prompting the agent across multiple turns to plan the test, draft it, then refine.
- Step 4: Create `.github/agents/qa-bug-reproducer.agent.md` as the second example. Persona: takes a bug report and produces a failing test before any fix is attempted. Show the frontmatter only; the participant doesn't need to use this agent in the exercise.
- Verification: the new spec runs and passes.
- Try-it: have participants modify `qa-engineer.agent.md` to add a handoff to a (hypothetical) reviewer agent. Discuss what handoffs do.

### `04-agent-skills.md` — Agent Skills

- Pain point: page objects should follow a precise template and naming convention. Every team's template is slightly different. Instructions can describe the template; skills can ship the template plus a generator.
- Concept: skills are folders containing `SKILL.md` plus optional bundled scripts and assets. Copilot loads the description first, then the body when relevant, then the assets on demand. Different from instructions because they're scoped to specific tasks rather than always-on.
- Step 1: Create `.github/skills/qa-playwright-page-object/SKILL.md`. Frontmatter: `name`, `description` (this is what Copilot uses to discover the skill — be specific about when it applies). Body: instructions for generating a page object from a URL or running app, referencing `templates/page-object.template.ts` and `scripts/generate-page-object.mjs` via Markdown links.
- Step 2: Create the template file. A minimal Playwright page object class with placeholders for class name, locators, and actions.
- Step 3: Create the generator script. A simple Node script that takes a name and outputs a page object scaffold to stdout. Doesn't need to be sophisticated; needs to be invokable.
- Step 4: In Copilot Chat, ask "use the qa-playwright-page-object skill to create a page object for the products page." Save output as `tests/e2e/pages/ProductsPage.ts`.
- Step 5: Repeat for `CartPage.ts` and `CheckoutPage.ts`.
- Step 6: Refactor the three existing E2E specs (products, cart, checkout) to use the new page objects. Have Copilot do this; verify tests still pass.
- Step 7: Create the second skill, `.github/skills/qa-api-contract-validator/`. SKILL.md plus a `validate-against-openapi.mjs` script. Show the participant invoking it; don't refactor `tests/api/orders.spec.ts` to use it (the prompt file already covers contract testing).
- Try-it: have participants extend the page-object skill with a second template variant.

### `05-mcp-servers.md` — MCP Servers

- Pain point: Copilot can't see what your app actually looks like in a browser, can't tell whether your last PR is passing CI, and doesn't always have current docs for your test framework.
- Concept: MCP servers are tools available to Copilot at runtime. Configured per-workspace in `.vscode/mcp.json`.
- Step 1: Open `.vscode/mcp.json`. Note Playwright MCP is already there from the base repo.
- Step 2: Add the GitHub MCP server. Walk through configuration. Cover the PAT setup; flag that Copilot Enterprise admins may have org-level MCP configs that override workspace ones.
- Step 3: Optional third MCP server (the implementing LLM should pick a docs MCP that's currently maintained — verify before locking it in; if no clear choice exists, skip this step rather than recommend a stale server).
- Step 4: Use Playwright MCP live: in Copilot Chat, ask the agent to open the products page and describe what it sees. Confirm the agent uses the MCP tool.
- Step 5: Build `tests/e2e/visual.spec.ts` with the agent using Playwright MCP for live inspection. Snapshot one page.
- Step 6: Use GitHub MCP: ask the agent to check whether the most recent push has green CI. Confirm tool call.
- Try-it: have participants configure one additional MCP server of their choosing.

### `06-agentic-workflows.md` — Agentic Workflows

- Pain point: when E2E tests fail in CI, somebody has to read the logs, figure out which selector broke, and either fix or open an issue. This work is repetitive.
- Concept: agentic workflows are Markdown files in `.github/workflows/` (note: `.md`, not `.yml`) that describe what an AI agent should do when triggered. They run in GitHub Actions infrastructure under specific permissions.
- Step 1: Create `.github/workflows/qa-test-triage.md`. Frontmatter covers `on:` triggers (e.g., `workflow_run` for the CI workflow), `permissions:` (read-only on contents and pull-requests, write on issues), and `safe-outputs:` to constrain what the workflow can produce. Body is natural-language instructions: read the failure logs, identify likely cause, post a comment on the PR with categorization (selector drift, network flake, real bug).
- Step 2: Commit and push. Trigger by pushing a deliberately broken test.
- Step 3: Watch the workflow run. Inspect the comment it posts.
- Step 4: Inspect the workflow logs (`gh aw logs`).
- Try-it: extend the workflow to also label the PR.

> [!IMPORTANT]
> Agentic workflows require setup at the org level (the `gh-aw` extension and Actions permissions). The implementing LLM should include a prerequisites callout at the top of this module pointing participants to the official setup guide rather than reproducing it.

### `07-hooks.md` — Hooks

- Pain point: testers occasionally commit `.only` or `.skip` and break the CI signal. Lint failures show up only after CI runs, breaking flow.
- Concept: hooks are deterministic shell commands that run at lifecycle points. Configured in `.github/hooks/qa-hooks.json` (or `hooks.json` for plugin distribution). Eight hook types are available (`sessionStart`, `sessionEnd`, `userPromptSubmitted`, `preToolUse`, `postToolUse`, `agentStop`, `subagentStop`, `errorOccurred`). The walkthrough only needs to demonstrate two.
- Step 1: Create `.github/hooks/qa-hooks.json`. Configure a `preToolUse` hook that intercepts file writes to `tests/**/*.spec.ts`, runs `scripts/block-test-skip.sh` against the proposed content, and rejects the write if `.only` or `.skip` appears.
- Step 2: Create the script. Simple grep, exit non-zero with a message if matched.
- Step 3: Test by asking Copilot to add `.only` to a test. Watch the hook block it.
- Step 4: Add a `postToolUse` hook that runs `npm run lint` against the touched file after a successful write to `tests/`.
- Step 5: Create `scripts/lint-after-test-edit.sh`. Calls the lint script, prints output.
- Step 6: Test by editing a test file and watching the lint output appear.
- Try-it: have participants add a `sessionStart` hook that prints a reminder if `tests/` has uncommitted changes.

### `08-plugins.md` — Plugins

- Pain point: every team has been building a copy of these customizations. Sharing them across the QA org currently means cloning, copying files, and maintaining drift.
- Concept: a plugin packages any combination of agents, skills, hooks, MCP server configs, and slash commands into one installable unit declared in `plugin.json`. Distributed via marketplaces (Git repos with a `marketplace.json`).
- Step 1: Create `plugins/qa-toolkit/.github/plugin.json`. Manifest fields: `name`, `description`, `version`, `author`, and source paths for `skills`, `agents`, `mcpServers`, `hooks`. Reference the existing files in `.github/` rather than duplicating them.
- Step 2: Walk through the marketplace concept. Don't have participants set up a marketplace inside the workshop time, but show what `marketplace.json` looks like and link to where they'd publish it.
- Step 3: Demonstrate (instructor-led even though the workshop is self-paced — phrase it as "here's what the install looks like once you've published") the install command: `copilot plugin marketplace add <repo>` then `copilot plugin install qa-toolkit@<marketplace>`.
- Try-it: have participants modify the plugin.json to add a custom agent of their own creation.
- Closing section: a short "what now" with three concrete next steps for taking this back to their team.

## Customization artifact specifications

These are the files in `.github/`, `plugins/`, and `.vscode/`. Each module above triggers the creation of one or more of these. This section gives the implementing LLM enough detail to build them correctly.

### `.github/copilot-instructions.md` (modify)

Append a clearly delimited "QA conventions" section at the bottom. Don't rewrite or restructure existing content. The new section covers: which test types live where, the Playwright-first stance, the no-`waitForTimeout` rule, the test ID convention used in the codebase, and a one-line pointer to the path-scoped instructions for tests.

### `.github/instructions/qa-playwright.instructions.md` (new)

Frontmatter:

```yaml
---
applyTo: "tests/**/*.spec.ts"
description: Playwright test conventions for the OctoCAT Supply QA suite.
---
```

Body covers: locator strategy in priority order, fixtures over hooks, network mocking with `route` rather than environment overrides, expectation patterns, file naming, parallel safety, and what to do when a test is flaky (don't add waits — investigate).

### `.github/prompts/qa-scaffold-e2e.prompt.md` (new)

Frontmatter: `description`, `mode: agent`. Body takes a flow name as input, references the path-scoped instructions via Markdown link, and produces a Playwright spec covering happy path, one validation error case, and one network failure case. Output goes to `tests/e2e/<flow>.spec.ts`.

### `.github/prompts/qa-api-contract-test.prompt.md` (new)

Frontmatter same shape. Body takes a resource name, locates the OpenAPI spec, and produces a Playwright API test that exercises every operation on that resource and validates response shape against the schema. Output goes to `tests/api/<resource>.spec.ts`.

### `.github/prompts/qa-visual-regression.prompt.md` (new)

Frontmatter same shape. Body takes a page name and produces a Playwright spec that captures snapshots of the key viewports (mobile, tablet, desktop). Output goes to `tests/e2e/visual/<page>.spec.ts`.

### `.github/agents/qa-engineer.agent.md` (new)

Frontmatter:

```yaml
---
name: qa-engineer
description: Persistent QA engineering persona; only modifies files under tests/.
tools: ['search/codebase', 'search/usages', 'edit/files', 'terminal/run']
model: ['Claude Opus 4.5', 'GPT-5.2']
---
```

Body covers: scope (tests/ only, never production code), preferred test framework (Playwright), default to fixtures, ask before adding new dependencies, end every test with a passing run before declaring done.

### `.github/agents/qa-bug-reproducer.agent.md` (new)

Frontmatter same shape, name `qa-bug-reproducer`. Body covers: input is a bug report, output is a failing test that reliably reproduces the bug, no fix attempts, no production code edits, end with the failing test output as evidence.

### `.github/skills/qa-playwright-page-object/SKILL.md` (new)

Frontmatter:

```yaml
---
name: qa-playwright-page-object
description: Generate a Playwright page object class for a given page in the application. Use when asked to create a page object, scaffold a POM, or refactor a Playwright spec to use a page object pattern.
---
```

Body: instructions for the generation flow. Reference `templates/page-object.template.ts` for the structure to follow. Reference `scripts/generate-page-object.mjs` for the optional script-based path. Cover naming (`<PageName>Page.ts`), location (`tests/e2e/pages/`), what should be a locator vs an action, and how to handle nested components.

### `.github/skills/qa-playwright-page-object/templates/page-object.template.ts` (new)

A minimal page object template with placeholders. Use `{{PAGE_NAME}}` and `{{LOCATORS}}` and `{{ACTIONS}}` markers.

### `.github/skills/qa-playwright-page-object/scripts/generate-page-object.mjs` (new)

A short Node script (~30 lines) that takes `--name` and `--url` arguments and writes a page object file by populating the template. Doesn't need network access; just templates from the static file.

### `.github/skills/qa-api-contract-validator/SKILL.md` (new)

Frontmatter:

```yaml
---
name: qa-api-contract-validator
description: Validate an API response or test fixture against the project's OpenAPI specification. Use when asked to check API responses against contracts, validate response shapes, or generate contract tests from an OpenAPI document.
---
```

Body: instructions to load the OpenAPI spec from its actual path in this repo (the implementing LLM should grep for it before writing this section), then validate either a captured response or a generated test against the spec.

### `.github/skills/qa-api-contract-validator/scripts/validate-against-openapi.mjs` (new)

A Node script that takes a path to a JSON file and a path to an OpenAPI spec, and reports schema violations. Use a maintained library (`@apidevtools/swagger-parser` plus `ajv`); pin versions in a comment in the script header.

### `.github/workflows/qa-test-triage.md` (new)

Agentic workflow file. Frontmatter declares trigger (`workflow_run` on the CI workflow's failure), permissions (read on contents and pull-requests, write on issues), `safe-outputs` constraining what the agent can do (post a single PR comment, no other side effects). Body in natural language: read the test failure logs, classify the failure (selector drift, network flake, real regression), post a comment with the classification and a suggested next step.

### `.github/hooks/qa-hooks.json` (new)

```json
{
  "hooks": {
    "preToolUse": [
      {
        "matcher": { "tool": "edit/files", "path": "tests/**/*.spec.ts" },
        "command": ".github/hooks/scripts/block-test-skip.sh"
      }
    ],
    "postToolUse": [
      {
        "matcher": { "tool": "edit/files", "path": "tests/**" },
        "command": ".github/hooks/scripts/lint-after-test-edit.sh"
      }
    ]
  }
}
```

The implementing LLM should verify the exact schema against the current GitHub docs before committing; the shape above is the intent, not the literal spec.

### `.github/hooks/scripts/block-test-skip.sh` (new)

Bash script. Reads stdin (the proposed file content) and the file path from environment. Greps for `\.only\b` and `\.skip\b` outside comments. Exits 1 with a message if found.

### `.github/hooks/scripts/lint-after-test-edit.sh` (new)

Bash script. Runs `npm run lint -- <file>` against the path passed in. Prints output. Exits 0 either way (postToolUse should warn, not block).

### `.vscode/mcp.json` (modify)

Keep the existing Playwright MCP server entry. Add an entry for the GitHub MCP server. The implementing LLM should pull the current GitHub MCP server URL and config shape from official GitHub docs at implementation time rather than relying on examples in this design doc.

### `plugins/qa-toolkit/.github/plugin.json` (new)

```json
{
  "name": "qa-toolkit",
  "description": "QA testing customizations for OctoCAT Supply: agents, skills, hooks, MCP, and slash commands.",
  "version": "0.1.0",
  "author": { "name": "<workshop participant>" },
  "skills": "../../../.github/skills",
  "agents": "../../../.github/agents",
  "hooks": "../../../.github/hooks/qa-hooks.json",
  "mcpServers": "../../../.vscode/mcp.json"
}
```

The implementing LLM should verify path resolution works in the current plugin format. If relative-up paths don't resolve, the alternative is to move the customizations under `plugins/qa-toolkit/` and symlink or duplicate them.

## Reference files

### `demo/walkthroughs/qa-testing-workshop/reference/customization-cheatsheet.md`

A one-page reference. Table-free; use prose paragraphs grouped by customization type. Each entry: what it is in one line, when to reach for it, where it lives, and a one-line example of activation. This is what participants pin in a tab six months later.

### `demo/walkthroughs/qa-testing-workshop/reference/troubleshooting.md`

Organized by symptom, not by feature. Likely entries: "Slash command doesn't appear in the menu," "Agent shows up but tools are missing," "Skill isn't being picked up by Copilot," "Hook isn't firing," "MCP server fails to start," "Agentic workflow runs but produces no output." Each entry: symptom, likely causes in priority order, fix.

## Implementation notes

1. **Verify file paths and frontmatter schemas at implementation time.** GitHub's customization formats are evolving. This design doc is accurate as of the user's research, but the implementing LLM should verify the current `.agent.md`, `.prompt.md`, `SKILL.md`, hook JSON, and `plugin.json` schemas against official GitHub docs before committing files. If any have changed, follow the current docs.

2. **Verify the OpenAPI spec location** in the repo before writing the contract-test prompt and skill. Likely under `api/` somewhere; grep for `openapi:` to find it.

3. **Don't duplicate the existing repo's instructions content.** When modifying `.github/copilot-instructions.md`, append; don't rewrite. Add a clearly delimited section header so participants can see the diff.

4. **Each module's walkthrough must stand alone.** A participant returning six months later should be able to do Module 4 without having done Modules 1–3 in the same session, as long as they have the artifacts from those modules in the repo. Each module's prerequisites section should state what files must exist before starting.

5. **Run Module 1 end-to-end before declaring done.** The implementing LLM should follow Module 1 as written and verify that `tests/e2e/products.spec.ts` is producible from the steps. This is the only module that needs this validation; if Module 1 is sound, the others will be.

6. **The workshop's existing `demo/walkthroughs/general/demo-steps.md` and other content should not be modified.** This workshop is additive.

7. **Stop short of writing test file contents in advance.** Participants generate them. If you need to ship reference test files for any reason (e.g., a baseline branch), put them in `demo/walkthroughs/qa-testing-workshop/reference/example-outputs/` and label them clearly as reference, not as starter code.

8. **The user will tweak voice and callouts.** Don't agonize over polish; aim for "good first draft a senior person would lightly edit." The structural decisions and technical accuracy matter more than the prose.
