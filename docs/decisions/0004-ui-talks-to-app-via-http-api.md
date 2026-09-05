# 0004. The UI talks to the app only through its own HTTP API

- **Status:** accepted
- **Date:** 2026-09-05

## Context

The long-term goal is for AI agents to log bets, post offers and read state — doing
programmatically what the user does by hand today. That is Phase 6; the app is being built
now.

Frameworks in play make it easy to query the database directly from server-rendered
components, which is convenient and would leave no API for anything else to call.

## Options considered

### Option A — All access through the app's own HTTP API
The UI is one client among several. Agents later become another client of the same endpoints.

### Option B — Direct database access from the UI, add an API later
Less code now. Business rules end up embedded in UI code, so the later API either duplicates
them or requires extracting them — a refactor of exactly the parts that handle money.

## Decision

**The UI talks to the app only through its own HTTP API.** No direct database access from
components.

## Why

The cost now is small — route handlers that would largely exist anyway — and it is paid at the
moment when changing course is cheapest. The cost of retrofitting is a refactor of validation
and business rules under a working system that already holds real financial records.

It also forces the rules to live in one place. If an agent can post a bet that the UI would
have rejected, the rules were in the wrong layer.

The argument against is genuine: an extra hop, some boilerplate, and slightly more ceremony
for simple reads. Accepted deliberately, because the alternative concentrates the eventual
pain exactly where the money logic lives.

## Consequences

- Validation and business rules sit behind the API, not in components.
- The API is designed as a real interface from Phase 1, not an afterthought.
- Agent access later is authentication and authorisation work only — no restructuring.
- Any input arriving from an agent is treated as a proposal subject to the same server-side
  rules as the UI's, never as an instruction.
