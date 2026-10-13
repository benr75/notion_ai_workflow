# <Feature name>

<!--
FEATURE TEMPLATE: a row in the Features database (Stage = Template). This page is the
contract the agent implements against. Tickets summarize it; it wins when they disagree.

Properties: Stage · Priority (P0 must-have, P1 core, P2 nice-to-have, P3 later) ·
Status (Backlog → In progress → Done, or BLOCKED) · Deployed to · <TRACKER> epic ·
<TRACKER> issue (single-ticket features) · <TRACKER> sub-issues (`#12 Short name, #13 Short name`)

How to use this template:
1. Duplicate this row, rename it to the feature, and set **Stage**.
2. Set **Priority**. Leave **Status** as Backlog until the first ticket starts.
3. Fill every placeholder, then delete this comment.
4. Once the epic and sub-issues exist, fill the epic (or issue) link and the sub-issues field.
5. As PRs merge, tick the acceptance criteria and append *(#issue, PR #n)* to each one.
   The spec is the record of what was built.
-->

## Overview

<Two or three sentences: what the feature is, who it is for, and how it fits the stage. Link to any earlier feature it builds on.>

## Decisions (from requirements interview, <YYYY-MM-DD>)

- **<Topic>:** <the decision, stated so a ticket can be checked against it, plus a short why.>
- **<Topic>:** <decision.>
- **<Topic>:** <decision. Later changes keep the date: "(decided YYYY-MM-DD)".>

## User stories

**<Actor, e.g. Admin (authenticated)>:**
- As a <actor>, I can <do something> so that <outcome>.
- As a <actor>, I can <do something>. *(gap found YYYY-MM-DD, filed as #N)*

**<Actor, e.g. Visitor (no account)>:**
- As a <actor>, I can <do something>.

## Requirements

<Optional. Use when the feature has infrastructure or configuration work that isn't naturally a user story (a deploy feature would use this). Group with bold sub-labels. Delete the section if every requirement is already a story or a criterion.>

**<Group>**
- <Requirement.>

## Acceptance criteria

<Every item is a checkbox, testable, and specific enough that a PR can cite the test that proves it. Group by theme with bold labels. Security-sensitive areas need hostile-case criteria (brute force, spoofed identifiers, cross-account access, mass submission).>

**<Theme, e.g. Creation>**
- [ ] <Criterion.>
- [x] <Criterion that has merged.> *(#12, PR #57)*

**<Theme, e.g. Access control>**
- [ ] <Criterion.>
- [ ] Hostile case: <what must be rejected and how.>
- [ ] <Partly done criterion.> *(first half: #12, PR #57; second half is verified when <Feature> lands, #29)*

**<Theme, e.g. Management>**
- [ ] <Criterion.>

## Technical notes

<Before the build: constraints the tickets must honor. During the build: a short dated note per merged ticket recording what landed and any gotchas, so the next ticket starts with full context.>

- <Models, routes, storage layout, or constraints the tickets must honor.>
- <Known gotchas, migration concerns.>
- <Tests worth calling out explicitly.>
- <e.g. "Base merged YYYY-MM-DD (#11, PR #55): what it established; what it left for #13.">

## Out of scope

- <Explicitly not built in this stage, with a reason if it is not obvious.>
- <Anything with enough shape to keep goes to Future Features with a pick-up trigger.>
