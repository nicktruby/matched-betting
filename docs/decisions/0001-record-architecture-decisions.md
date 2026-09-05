# 0001. Record architecture decisions

- **Status:** accepted
- **Date:** 2026-09-05

## Context

An earlier design session covered a great deal of ground — database engines, hosting, project
layout, exchange integrations, terms of service — and all of it lived in a chat log. Decisions
were revisited repeatedly because the reasoning behind them was not written down anywhere a
later session could find.

This project is also intended as portfolio work, where showing *why* a choice was made is
worth more than the code that resulted from it.

## Options considered

### Option A — Architecture Decision Records
One short markdown file per decision, numbered, in `docs/decisions/`. Context, options,
decision, reasoning, consequences.

### Option B — A single running design document
Simpler to start. Tends to become a wall of text where superseded reasoning is edited away,
so the history of *why* is lost.

### Option C — Rely on commit messages
Free, but a commit explains a change, not a choice between alternatives, and it is not
discoverable months later.

## Decision

Architecture Decision Records in `docs/decisions/`, one per non-obvious technical choice.

## Why

The failure mode this prevents is real and already happened: re-litigating settled questions
because the reasoning was not recoverable. ADRs are immutable — superseding a decision adds a
file rather than editing history, so the record of what was believed and when survives.

The argument against is ceremony: a five-minute write-up per decision, which is friction on
small choices. Mitigated by keeping them short and only writing them for choices that had a
genuine alternative.

## Consequences

- A new session can read `docs/decisions/` and understand the shape of the system without
  archaeology.
- `/new-adr` scaffolds one, so the friction is a filename and a title.
- Decisions are not edited once accepted. They are superseded.
