# <PROJECT_NAME>

<!--
HUB PAGE: the top-level page in <SPEC_SOURCE> (e.g. Notion). Everything else hangs off it:

  <PROJECT_NAME>                      ← this page
  ├── Stage 1: <Short name>           ← one PRD per build stage (PRD.md)
  ├── Stage 2: <Short name>
  ├── Features (inline database)      ← every feature spec, all stages (FEATURE.md as a row)
  │   └── Feature Template            ← a row with Stage = "Template"
  ├── PRD Template                    ← a page, duplicated to start each stage
  ├── Process                         ← mirror of PROCESS.md + docs/workflow.md
  └── Future Features                 ← deferred ideas with pick-up triggers (FUTURE_FEATURES.md)

FEATURES DATABASE SCHEMA (put the descriptions on the properties themselves so they show in the UI):

  | Property          | Type         | Options / description                                                                 |
  |-------------------|--------------|---------------------------------------------------------------------------------------|
  | Feature           | Title        |                                                                                       |
  | Stage             | Select       | "Stage 1: <Short name>", …, plus "Template". One option per PRD page.                 |
  | Priority          | Select       | P0 must-have, P1 core, P2 nice-to-have, P3 later.                                     |
  | Status            | Status       | To-do: Backlog, BLOCKED · In progress: In progress · Complete: Done.                  |
  |                   |              | Backlog until the first ticket starts; In progress on the epic's first sub-issue;     |
  |                   |              | Done when the last sub-issue merges.                                                  |
  | Deployed to       | Select       | The furthest environment the feature has reached, e.g. Staging → Production.          |
  |                   |              | Add options as environments appear.                                                   |
  | <TRACKER> epic    | URL          | The epic ticket for this feature.                                                     |
  | <TRACKER> issue   | URL          | For single-ticket features with no epic.                                              |
  | <TRACKER> sub-issues | Text      | Format: `#12 Short name, #13 Short name`                                              |
  | Created           | Created time |                                                                                       |

  The Template row (Stage = Template) keeps the Feature Template inside the database. Each
  PRD embeds a linked view of Features filtered to its own Stage, so template rows never
  show up in a real stage.

Delete this comment once the hub is set up.
-->

> 🎯 **<PROJECT_NAME> is <one sentence: what it is and for whom>.** <One or two sentences on what it replaces or enables.> This page is the hub. Each build stage has a PRD page below, and the **Features** database holds every feature spec across all stages. Feature status in <SPEC_SOURCE> is the source of truth; <TICKET_TRACKER> tracks the day-to-day work.

## What <PROJECT_NAME> is

<Two or three paragraphs: the mission or problem, how it's handled today, and what changes.>

### The people in the loop

- **<Actor>**: <what they do with the product>.
- **<Actor>**: <…>.

### Source material

<Inputs the specs were written from: product overview, captured forms or spreadsheets, research notes. These feed the feature specs but are not the specs themselves.>

- <link>: <what it is>

## How this hub is organized

- **One PRD page per build stage.** Each stage page holds the stage overview, the fixed product decisions from the requirements interview, and a live view of that stage's features. Duplicate **PRD Template** to start a new stage.
- **Features database.** One row per feature spec, tagged with its Stage, Status, Priority and Deployed to, plus links to the epic and sub-issues. Duplicate **Feature Template** to start a new feature.
- **Process.** How work moves between <SPEC_SOURCE>, <TICKET_TRACKER>, the agent and <REVIEWER>, plus the ticket-by-ticket workflow that the repo's `PROCESS.md` mirrors.
- **Future Features.** Ideas deliberately deferred, each with the reason and the trigger for picking it up.

## Stages

- Stage 1: <Short name> *(link)*

## Features

<Inline Features database: all rows, grouped by Stage.>

## Templates and process

- PRD Template · Process · Future Features

## Related

- Repository: `<REPO>`
- <Sibling projects or prior art, if any>
