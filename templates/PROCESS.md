# PROCESS.md

The workflow the coding agent follows for every ticket in this repo. One ticket = one branch = one PR.

**Sources of truth:** <TICKET_TRACKER> (`<REPO>`) tracks the work; <SPEC_SOURCE> holds the specs and product decisions. In <SPEC_SOURCE>, the **<PROJECT_NAME>** hub page has one PRD page per build stage (the stage's fixed decisions table and its feature list) and the **Features** database holds every feature spec across all stages, tagged with a **Stage** property. If a ticket and the spec disagree, the spec wins. Flag the discrepancy on the ticket rather than silently picking one. Decisions listed in CLAUDE.md are fixed; don't redesign them mid-ticket.

## 1. Pull a ticket

- Default order: open sub-issues whose dependencies are done, higher-priority epics first (e.g. P0 before P1), lowest ticket number first. This applies unless <REVIEWER> names a ticket.
- Read the full chain before writing code: the sub-issue, its epic, the linked feature spec (requirements, decisions, acceptance criteria), and the decisions table on that feature's stage PRD.
- Comment on the ticket that work is starting. If this is the epic's first sub-issue, set the feature's Status to **In progress**.
- If the ticket is ambiguous or the spec has a gap, resolve it with <REVIEWER> *before* implementing. Put the question on the ticket or ask directly. If work can't proceed until something outside the ticket happens (a vendor choice, credentials, another epic), set the feature's Status to **BLOCKED** and say why on the ticket.

## 2. Create a branch

- Branch from an up-to-date default branch: `git pull origin <DEFAULT_BRANCH>` first.
- Naming: `<type>/<ticket-number>-<short-slug>`, where type is one of:
  - `feature/`: new functionality (e.g. `feature/12-user-login`)
  - `bug/`: fixing broken behavior
  - `chore/`: tooling, docs, dependencies, refactors with no behavior change

## 3. Implement with full tests

- Follow the conventions in CLAUDE.md (style guide, framework generators, project-specific constraints).
- Every change ships with tests: unit tests for models/logic, request or controller tests, and integration tests for user-facing flows.
- Security-sensitive areas (auth, file uploads/extraction, file serving, access gates, anything that parses untrusted input) require **hostile-case tests**, e.g. path traversal, archive-slip entries, brute force, session reuse across boundaries.
- Never rewrite code or weaken a test to make it pass. Fix the underlying code. If a test is genuinely wrong, say so explicitly in the PR.
- Stay in scope: implement only this ticket. Discoveries that belong elsewhere become new tickets (or comments on existing ones), not extra commits.
- *(Optional)* User-facing changes add a dated entry to the changelog (`<CHANGELOG_PATH>`) in the same PR.
- Commit in small, coherent steps with imperative-mood messages ("Add login rate limiting", not "Added…" / "WIP").

## 4. Validate against ticket and spec

- Walk the ticket's acceptance criteria one by one. Each needs evidence: a named test or a manual verification.
- Re-read the feature spec and confirm nothing in Requirements/Decisions is violated or missed. Tickets are summaries; the spec is the contract.
- Run the full check suite locally and get it green:
  - `<TEST_COMMAND>`
  - `<LINT_COMMAND>`
  - `<SECURITY_SCAN_COMMAND>` *(static analysis)*
  - `<DEPENDENCY_AUDIT_COMMAND>`
- For user-facing changes, run the app (`<DEV_SERVER_COMMAND>`) and exercise the flow. Passing tests are not the same as a working feature.

## 5. Submit a PR

- Push the branch and open a PR (e.g. `gh pr create`), using the PR template (`.github/pull_request_template.md`):
  - Title: imperative summary, e.g. "Add password reset for signed-in users".
  - Body: what changed and why, `Closes #<ticket>`, the acceptance-criteria checklist with how each is satisfied, and test evidence (suite output summary).
- Confirm CI is green; fix failures on the same branch.
- **Do not merge.** <REVIEWER> reviews and merges. Address review feedback with follow-up commits on the branch.

## 6. After merge

- Pull the default branch and delete the local and remote feature branches.
- Verify the ticket auto-closed (the `Closes #` keyword); close it manually if not.
- Check off the acceptance criteria on the feature spec that this merged ticket satisfied, appending *(#ticket, PR #n)* to each and noting any deferrals inline. The spec is the record and must reflect what was merged. The PR body's criteria mapping is the evidence.
- Add a short dated line to the feature's Technical notes recording what landed and anything the next ticket needs to know.
- If a gap turned up during the ticket, add it to the spec as a user story or criterion marked *(gap found YYYY-MM-DD, filed as #N)*. If an idea is worth keeping but not for now, add it to Future Features with a pick-up trigger.
- If that was the epic's last sub-issue, close the epic and set the feature's Status to **Done**.

## 7. After deploy

- When merged work reaches an environment, set **Deployed to** on the affected feature(s) to the furthest environment reached, so everyone knows what's live and where. Add environments to the field's options as they appear.
  - Example with staging: merging to `<DEFAULT_BRANCH>` with green CI auto-deploys to **Staging**; **Production** is reached by promoting that build (`<DEPLOY_COMMAND>`), never by deploying straight from the branch.
  - Example without staging: `<DEPLOY_COMMAND>` deploys to **Production** directly.
- A feature isn't "shipped" until Status is **Done** *and* **Deployed to: Production** is set.

## Conventions at a glance

- Ticket numbers are the tracker's. An epic is an issue with sub-issues; a small feature can be a single issue.
- Criterion annotation on merge: append `(#ticket, PR #n)` to each ticked criterion.
- Status: Backlog → In progress (epic's first ticket starts) → Done (last sub-issue merges); BLOCKED when waiting on something outside the ticket.
- Shipped means Status **Done** and **Deployed to: Production**, both.
- Requirements interviews produce dated decisions: a `Decisions (from requirements interview, date)` section on the feature page and a dated decisions table on the stage PRD.
