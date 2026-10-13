# CLAUDE.md

Guidance for Claude Code when working in this repository.

**Working a ticket? Follow PROCESS.md**: pull ticket → branch (`feature/`/`bug/`/`chore/`) → implement with tests → validate against the ticket and spec → PR (never self-merge).

<!--
How to fill this in:
- Keep it short. This file is loaded into every agent session; every line costs context.
- Write facts and rules, not aspirations. If a rule has a reason, give it in one clause.
- Delete any section that doesn't apply. Replace every <PLACEHOLDER>.
-->

## What this app is

<PROJECT_NAME>: <one or two sentences on what it does and for whom>. Full requirements live in <SPEC_SOURCE>. The **<PROJECT_NAME>** hub page holds one PRD per build stage (<list stages, e.g. Stage 1 "MVP", Stage 2 "Integrations">), and the **Features** database holds every feature spec, tagged with its Stage. Work is tracked in <TICKET_TRACKER> at `<REPO>`.

## Status

<One short paragraph: which stage is under way, what has merged (with ticket numbers), what's open, and where each environment stands. Check the tracker and the Features database for current status rather than trusting this paragraph.>

Local setup needs <runtime + version, database + version>. <How the first admin/user is created, e.g. env vars passed to the seed task. Never put real values here.>

## Commands

```sh
<SETUP_COMMAND>                # bootstrap dependencies, database, etc.
<DEV_SERVER_COMMAND>           # dev server on http://localhost:<PORT>
<TEST_COMMAND>                 # full test suite
<LINT_COMMAND>                 # style / lint
<SECURITY_SCAN_COMMAND>        # static security analysis
<DEPENDENCY_AUDIT_COMMAND>     # known-vulnerability audit of dependencies
<DEPLOY_COMMAND>               # deploy
```

Run a single test: `<SINGLE_TEST_COMMAND>`

<!-- Example (Rails): bin/setup, bin/dev, bin/rails test, bin/rubocop, bin/brakeman, bin/bundler-audit -->

<How code reaches each environment, e.g. "Merging to main with green CI auto-deploys to staging. Production is released by promoting the staging build, never deployed straight from main.">

## Architecture

- <Language/framework and versions; where the versions are pinned>
- <Datastores, background jobs, caching. Also note what is deliberately absent, e.g. "No Redis">
- <Frontend approach, e.g. "no JS build step; vanilla JS under app/javascript">
- <Deployment shape at a high level, and where its config lives>

## Product decisions (fixed, don't redesign)

Settled in the Stage <N> requirements interview, <YYYY-MM-DD>. The full table with reasons is on the Stage <N> PRD in <SPEC_SOURCE>.

<!--
Decisions already made with the product owner. The agent must not reopen them mid-ticket;
if a ticket seems to need one changed, it asks first. Mirror the decisions tables in your
stage PRDs. One bullet per decision, bold the topic.
-->

- **<Topic>**: <decision>. <One-clause reason, if useful.>
- **<Topic>**: <decision>.
- **Out of scope for this stage**: <things intentionally not built, so the agent doesn't add them>.

## Conventions

- **Generators first**: use the framework's generators/scaffolding for anything they can produce (models, migrations, controllers, auth). Generate, then edit the output. Don't hand-write those files from scratch. *(Example: `bin/rails generate …`.)*
- **Changelog** *(if you adopt this)*: every user-facing change adds a dated entry to `<CHANGELOG_PATH>` in the same PR, newest first.
- **Never rewrite code or weaken a test to make it pass.**
- **Lint before commit**: follow <STYLE_GUIDE>; run `<LINT_COMMAND>` before every commit.
- **Hostile-case tests for security areas**: <list this project's sensitive areas, e.g. uploads, file serving, auth, access gates> need tests for the attack cases (<e.g. path traversal, archive-slip, brute-force rate limiting>).
- **No flaky browser tests** *(if you adopt this)*: cover user flows with integration/request tests. Don't add a browser-driver test layer back without a decision.
- <Project-specific constraints the agent would otherwise get wrong, e.g. "files served from disk must not go through the ORM's attachment layer".>
