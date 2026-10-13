# Spec-driven agent workflow

A ready-to-copy development process for projects where an AI coding agent (e.g. Claude Code) does most of the implementation and a human reviews. Requirements live in a spec tool, work lives in a ticket tracker, and the agent keeps the two in sync. One ticket = one branch = one PR, and the agent never merges its own work.

The process came from a real project that was built this way from first spec to production. It is written to be stack-agnostic. Anything specific to one stack is marked as an example.

## Quick start

You need [Claude Code](https://claude.com/claude-code), the `gh` CLI signed in to GitHub, and, if you use Notion for specs, a Notion connector or MCP server that Claude Code can use.

1. Clone this repo next to your project:

   ```sh
   git clone https://github.com/benr75/notion_ai_workflow.git ~/agent-workflow
   ```

2. Open Claude Code in **your project's** repo (new or existing) and paste this prompt. Change the path in the first line if you cloned somewhere else.

   ````text
   Set up the spec-driven agent workflow from ~/agent-workflow in this repository.

   1. Read ~/agent-workflow/README.md and every file under ~/agent-workflow/templates
      and ~/agent-workflow/examples before doing anything else.
   2. Look at this repo to work out what you can on your own: language and framework,
      the test, lint, security-scan, dependency-audit, dev-server and setup commands,
      the default branch, the GitHub repo slug, and any existing CLAUDE.md, CI or
      Dependabot config.
   3. Then ask me, in one batch, for whatever you couldn't find: the project name and
      a one-paragraph description, who reviews and merges PRs, where specs live
      (Notion or a docs/specs/ folder) and where tickets live, how code reaches each
      environment (staging? production?), and whether I want the optional pieces
      (changelog, no-browser-tests rule, Dependabot, CI). Put your best guess next
      to each question.
   4. Copy the repo files from the "What's in here" table into this repo and fill in
      every placeholder: PROCESS.md, CLAUDE.md, docs/workflow.md,
      .github/pull_request_template.md, and the CI and Dependabot files adapted to
      this stack. Merge into any existing CLAUDE.md or CI instead of overwriting it.
      Leave the Product decisions section of CLAUDE.md empty, because decisions come
      from the first requirements interview. When you're done,
      `grep -rn '<[A-Z_]*>' .` should find no placeholders left in the copied files.
   5. Set up the spec source by following step 3 of "Adopting it" in the README.
      In Notion, ask me which parent page to use, then create the hub page, the
      Features database with the exact schema in HUB.md (including the Template
      Stage option), the PRD Template page, the Feature Template row, a Process page,
      and a Future Features page. Use the templates' structure and gray placeholders.
      If I chose a docs/specs/ folder instead, create the same structure as markdown
      files there.
   6. Show me a summary of every file you created or changed and every Notion page
      you created, with links. Don't commit, push, or change GitHub settings until
      I approve. Never write secrets or .env values into any file or page.
   ````

3. Start the first stage. Once setup is done, in the same session:

   ```text
   Let's start Stage 1. Draft the Stage 1 PRD from the PRD Template using <SOURCE
   MATERIAL: a product brief, notes, or a link>, then interview me to fill in the
   product decisions. After that, spec each feature from the Feature Template, create
   the GitHub epics and sub-issues, and link them back into the Features database.
   ```

4. Build, one ticket at a time:

   ```text
   Work the next ticket, following PROCESS.md.
   ```

   You review and merge each PR. After a merge, the agent updates the spec as described in PROCESS.md steps 6 and 7.

## How it works

```
Spec (PRD per stage → feature specs)   ──►  Epics + tickets with acceptance criteria
        ▲                                                │
        │   check off criteria, set Status / Deployed    ▼
        └──────────────  Agent: branch → implement + tests → validate → PR  ──►  Human reviews & merges
```

1. **Spec.** The agent drafts a feature spec from the template, interviews the reviewer to fill gaps, and writes the answers back into the spec as dated decisions.
2. **Plan.** The agent cuts one epic per feature and sub-issues per slice. Each ticket carries its acceptance criteria as a checklist and links to the spec.
3. **Build.** For each ticket: branch (`feature/` · `bug/` · `chore/`), implement with tests, validate every acceptance criterion against the spec, run the full check suite, open a PR, and get CI green.
4. **Review.** A human reviews and merges. The agent addresses feedback with follow-up commits.
5. **Sync.** After merge, the agent checks off the satisfied criteria in the spec and updates the feature's Status. After deploy, it sets **Deployed to**.

The ground rules that make it work:

- **The spec is the contract.** Tickets are summaries. If they disagree, the spec wins, and the agent flags the mismatch rather than picking one.
- **Fixed decisions stay fixed.** Settled product decisions are listed in `CLAUDE.md`, and the agent doesn't redesign them mid-ticket.
- **Evidence for every criterion**, either a named test or a described manual check, written into the PR body.
- **Never weaken a test to make it pass.** Fix the code, or say explicitly that the test was wrong.
- **Stay in scope.** Anything discovered along the way becomes a new ticket, not extra commits.
- **Hostile-case tests** for security-sensitive areas.
- **The human merges.** Always.

## What's in here

| Path | Copy to | Purpose |
|---|---|---|
| [templates/PROCESS.md](templates/PROCESS.md) | `PROCESS.md` | The step-by-step ticket workflow the agent follows |
| [templates/CLAUDE.md](templates/CLAUDE.md) | `CLAUDE.md` | Agent instructions: commands, architecture, fixed decisions, conventions |
| [templates/docs/workflow.md](templates/docs/workflow.md) | `docs/workflow.md` | Human-facing overview with diagrams and a table of who owns what |
| [templates/specs/HUB.md](templates/specs/HUB.md) | spec tool: hub page | Hub page, page tree and the Features database schema |
| [templates/specs/PRD.md](templates/specs/PRD.md) | spec tool: "PRD Template" page | One PRD per build stage: goals, a dated fixed-decisions table, linked feature view, definition of done |
| [templates/specs/FEATURE.md](templates/specs/FEATURE.md) | spec tool: "Feature Template" row | One spec per feature: dated decisions, user stories, acceptance criteria annotated `(#ticket, PR #n)` as they merge |
| [templates/specs/FUTURE_FEATURES.md](templates/specs/FUTURE_FEATURES.md) | spec tool: "Future Features" page | Deferred ideas, each with the reason and a pick-up trigger |
| [templates/specs/TESTER_GUIDE.md](templates/specs/TESTER_GUIDE.md) | spec tool *(optional)* | A plain-language guide for invited testers during a staging test-pass stage |
| [templates/github/ISSUE_EPIC.md](templates/github/ISSUE_EPIC.md) | reference | Shape of an epic ticket |
| [templates/github/ISSUE_TICKET.md](templates/github/ISSUE_TICKET.md) | `.github/ISSUE_TEMPLATE/` *(optional)* | Shape of a sub-issue |
| [templates/github/PULL_REQUEST_TEMPLATE.md](templates/github/PULL_REQUEST_TEMPLATE.md) | `.github/pull_request_template.md` | PR body: criteria mapped to evidence, plus check output |
| [examples/github/dependabot.yml](examples/github/dependabot.yml) | `.github/dependabot.yml` | Weekly dependency and Actions updates *(optional)* |
| [examples/github/workflows/ci.yml](examples/github/workflows/ci.yml) | `.github/workflows/ci.yml` | Generic CI with security scan, lint and test jobs *(optional)* |
| [examples/github/workflows/ci-rails.yml](examples/github/workflows/ci-rails.yml) | reference | The same CI filled in for a Rails app *(example)* |

## Adopting it

1. **Pick your tools** and decide what the placeholders mean:

   | Placeholder | Meaning | Example |
   |---|---|---|
   | `<PROJECT_NAME>` | Your project | |
   | `<REPO>` | Repo slug | `owner/name` |
   | `<SPEC_SOURCE>` | Where specs live | Notion, Confluence, a `docs/specs/` folder |
   | `<TICKET_TRACKER>` | Where work is tracked | GitHub Issues, Linear, Jira |
   | `<REVIEWER>` | The human who answers interviews and merges | a person or team |
   | `<DEFAULT_BRANCH>` | Default branch name | `main` |
   | `<TEST_COMMAND>`, `<LINT_COMMAND>`, `<SECURITY_SCAN_COMMAND>`, `<DEPENDENCY_AUDIT_COMMAND>`, `<DEV_SERVER_COMMAND>`, `<SETUP_COMMAND>`, `<SINGLE_TEST_COMMAND>`, `<DEPLOY_COMMAND>` | Your check and run commands | Rails: `bin/rails test`, `bin/rubocop`, `bin/brakeman`, `bin/bundler-audit`, `bin/dev` |
   | `<CHANGELOG_PATH>` | Optional user-facing changelog | `CHANGELOG.md` |

2. **Copy the templates** into your repo (see the table above) and replace every placeholder. `grep -rn '<[A-Z_]*>' .` finds any that are left.
3. **Set up the spec tool.** In Notion (the reference setup):
   - Create the hub page from `HUB.md`, with an inline **Features** database using the schema in its header comment. Include a `Template` option in **Stage**.
   - Create a **PRD Template** page under the hub from `PRD.md`, with a linked Features view filtered to `Stage = Template`.
   - Create a **Feature Template** row in Features (Stage = Template) from `FEATURE.md`.
   - Add a **Process** page (a copy of `PROCESS.md` plus `docs/workflow.md`) and a **Future Features** page from `FUTURE_FEATURES.md`.
   - Write template placeholders in gray, and give each template a "How to use this template" callout (included as comments in these files).

   If you'd rather keep specs as plain files, a `docs/specs/` folder works too: one file per PRD and per feature, with Stage, Priority, Status and Deployed to in front matter.
4. **Fill in `CLAUDE.md`.** Commands and architecture first. Add product decisions as they're settled; keep the file short.
5. **Turn on CI and Dependabot.** Adapt the examples to your stack and make sure CI runs the same checks as step 4 of `PROCESS.md`. Protect the default branch: require a PR, a passing CI run and an approving review.
6. **Give the agent access** to the spec tool (e.g. a Notion MCP server) and the tracker (e.g. the `gh` CLI). Allow read actions; keep merge and deploy permissions with the human.
7. **Run the first feature.** Ask the agent to draft a feature spec, answer its questions, have it cut the epic and tickets, then say "work the next ticket".

## Adapting it

- **No spec tool?** Use markdown specs in the repo. The rules stay the same: the spec beats the ticket, and criteria get checked off after merge.
- **Different tracker?** Swap `Closes #N` for your tracker's auto-close syntax and the `gh` commands for its CLI or API.
- **Optional pieces:** the changelog entry, the no-browser-tests convention, a Staging environment, and the stage PRDs (a small project may need only one).
- **Test-pass stages:** before the first production release it can pay to run a short "Stage N.5" with the same PRD template, to make staging usable by invited testers (captured email/SMS, a demo-data reset command, a test banner, a tester guide). See `TESTER_GUIDE.md`.
- **Branch types:** add `docs/`, `refactor/` and so on if you want them, but keep the `<type>/<ticket>-<slug>` shape so branches map back to tickets.
