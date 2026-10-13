# <PROJECT_NAME> Testing Guide

<!--
OPTIONAL: for a test-pass stage, e.g. "Stage N.5: Staging Test Pass", where invited
people (often non-technical) try a staging copy before production. Write it for the testers,
not for developers: plain language, no ticket numbers.

What usually makes a test pass work (these make good PRD decisions for that stage):
- A single switch (e.g. a DEMO_MODE env var) that is never set in production.
- Outbound email/SMS captured and shown in the app instead of sent, so testers can finish
  reset, invite and verification flows with fake addresses.
- A visible "test site" banner.
- One command that resets staging to a realistic mid-workflow snapshot and prints fresh
  logins for every role. It must refuse to run unless the switch is on.

CREDENTIALS: decide on purpose where test logins go. Rotate them every reset. Don't commit
them to the repo or write them into logs. If you put them on a shared page, keep it
restricted to testers and treat it as exposed.
-->

> 👋 **Thanks for helping test <PROJECT_NAME>.** <One sentence on what it is.> You'll be using a test copy filled with made-up data, so click anything and try to break things. **No real email or text message is ever sent.** <When the app would send one, it appears on your screen instead.>

## Getting started

- **Test site:** <STAGING_URL>
- **Your login:** <how testers get one: handed out directly / listed on a restricted page>. Logins change whenever the site is reset between test rounds.
- **Phone and computer:** please try both.
- **The banner** at the top of every page means you're on the test site.

## Test logins

| Role | Email / identifier | Password |
|---|---|---|
| <Role> | <test identifier> | *(handed out per round)* |

## How <the core thing> moves through <PROJECT_NAME>

<The main lifecycle in plain language, one numbered step per stage, naming who does each step. This is what helps testers understand why there are several kinds of user.>

1. **<State>.** <Who does what.>
2. **<State>.** <…>

## Who can do what

### <Role> (<how they sign in, or "no login">)
- <Capability.>

### <Role>
- <Capability.>

## Where to go

| What | Link | Who |
|---|---|---|
| <Screen> | <path> | <Roles> |

## What's already in the test site

<The snapshot: what data exists and in which states, so every screen has something to act on. Everything testers add stays until the next reset.>

## Things to try

**As <role>:** <a short scenario, including an edge case or two.>

**Anyone:** try things you shouldn't be able to do, like opening an admin link while signed in as a lower role, or reusing an old reset link.

## Reporting what you find

<Where to report: email, a shared doc, comments on this page.> For each problem or suggestion, please include:
- What you were trying to do, and which login you used
- What you expected, and what happened instead
- Phone or computer, and which browser
- A screenshot if you can

Confusing wording, slowness and "I didn't know where to click" are all worth reporting, not just errors.
