# 0005. Name the repo for the domain, the apps for their role

- **Status:** accepted
- **Date:** 2026-09-05

## Context

This will likely become a monorepo containing several deployable things: the ledger app that
replaces the spreadsheet, an automation agent, and a browser-driving resolver process. A name
was needed for the container.

Candidates considered leading with the technology — `matched-betting-agent`,
`matched-betting-agentics` — which would signal the automation work prominently on a public
GitHub profile, since this is also intended as portfolio work.

## Options considered

### Option A — Domain as the umbrella, role as the app name
`matched-betting/` containing `apps/ledger`, `apps/agent`, `apps/resolver`.

### Option B — Technology in the repo name
`matched-betting-agentics` or `matched-betting-agent` as the container.

## Decision

**The repository is named for the domain: `matched-betting`.** Individual apps and packages
carry names describing their role.

## Why

The domain is the stable part. "Agentic" describes how one component is built, and components
get rewritten — if the automation lands differently than planned, a repo named after the
approach is left describing something that no longer exists. The domain will still be matched
betting.

It also puts the descriptive weight where it's useful. `apps/agent` tells a reader exactly
what that directory is; a repo called `matched-betting-agent` containing a ledger, a resolver
and an agent tells them less.

The argument against is real and was weighed: on a public profile, `matched-betting-agentics`
catches an eye in a list where `matched-betting` doesn't, and the automation is the technically
interesting part. Rejected on the grounds that a README and a pinned description do that job
better than a name that may date, and that the repo spends its first several phases being a
ledger rather than an agent.

## Consequences

- Apps and packages are named for what they do, not what they're built with.
- The interesting technical signal lives in the README and in `docs/decisions/`, which is
  where a reader who cares will actually look.
- Renaming later is cheap while the repo is private and unlinked; it stops being cheap once
  anything references it.
