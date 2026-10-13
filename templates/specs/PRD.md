# Stage <N>: <Short name>

<!--
PRD TEMPLATE: one page per build stage, kept under the hub. In <SPEC_SOURCE>, write every
placeholder in gray so unfilled ones stand out.

How to use this template:
1. Duplicate this page under the hub and rename it `Stage N: Short name`.
2. Add a matching option to the **Stage** select on the Features database.
3. Change the filter on the copy's "Features in this stage" view to the new Stage option.
4. Hold the requirements interview with <REVIEWER> and fill every placeholder, then delete
   this comment. Decisions are recorded here before any code is written.

A stage doesn't have to be a product release. A short "test pass" stage (e.g. Stage 1.5:
make staging ready for invited testers) uses the same template; see TESTER_GUIDE.md.
-->

> 🎯 **Stage <N> of <PROJECT_NAME>: <Short name>.** <One or two sentences on what this stage delivers and for whom.> Feature status below is the source of truth; <TICKET_TRACKER> issues on `<REPO>` track the day-to-day work.

## Overview

<Two or three short paragraphs: the problem, the outcome for users, and what "done" looks like from the outside. Name the previous stage and what this one builds on.>

### Goals

- <Outcome a user can observe.>
- <Outcome a user can observe.>

### Non-goals

- <Something deliberately left out of this stage, with the reason.>

### At a glance

| 📦 What changes for users | 🛠️ What changes in the stack |
|---|---|
| <Visible change.> | <Model, route, service or infrastructure change.> |
| <Visible change.> | <New dependency, or "none".> |

## Product decisions

Settled in the requirements interview on <YYYY-MM-DD> and fixed for this stage. Mirror the developer-relevant rows into `CLAUDE.md` when the first ticket starts. Nothing here changes an earlier stage's decision unless the row says so explicitly.

| Area | Decision | Why |
|---|---|---|
| <Area, e.g. Stack, Hosting, Accounts, Email, Design, Testing, Limits> | <The decision, stated so a ticket can be checked against it.> | <Short reason.> |
| <Area> | <Decision. Later changes are appended with their date: "(decided YYYY-MM-DD)".> | <Why.> |

## Features

Each row is a full feature spec: overview, decisions, user stories, acceptance criteria, technical notes and an out-of-scope list. Acceptance criteria are checked off as PRs merge, so a feature page always shows what has actually been built. <Suggested build order: deployment and design foundations first, so every PR ships to a deployed, styled app; then Feature A, Feature B, …; features that wait on an outside decision (e.g. a vendor) last.>

<Linked view of Features, filtered to Stage = this stage. On the template itself it is filtered to Stage = Template.>

Ideas that come up during the build but are deliberately deferred go in **Future Features**, each with the reason and the trigger for picking it up.

## How a build stage works

This page and its feature specs are the pattern every stage follows. The full developer workflow is on the **Process** page and in `PROCESS.md` in the repo.

1. **Write the PRD.** Overview, fixed decisions, and the list of features for the stage. Decisions are made in a requirements interview and recorded here before any code.
2. **Spec each feature.** One page per feature in **Features**, duplicated from **Feature Template** and tagged with this stage in the Stage property. Acceptance criteria are written as checkboxes so they can be ticked as work lands.
3. **Create the tracker work.** Each feature gets an epic (or a single issue for small ones) and sub-issues. The feature page stores the epic link and the sub-issue list.
4. **Work tickets one at a time.** Pull a ticket, branch as `feature/`, `bug/` or `chore/`, implement with tests, validate against the issue and the spec, open a PR. <REVIEWER> reviews and merges; nothing self-merges.
5. **Update the spec after every merge.** The acceptance criteria that PR satisfied are checked off, with the issue and PR numbers noted inline. When the last sub-issue merges, the feature's Status becomes Done.
6. **Deploy and mark it.** After a deploy, set Deployed to on the affected features. A feature is shipped only when it is both Done and deployed.

> ✅ **Definition of done for the stage:** every feature is Done, every acceptance criterion is checked or has an explicit inline deferral, Deployed to shows <Production> on each one, and <the end-to-end check that proves the stage works for real users, e.g. one full cycle exercised on production>.

## Related

- Repository: `<REPO>`
- Previous stage: <link, or "none" for Stage 1>
- Future Features
- Features database
