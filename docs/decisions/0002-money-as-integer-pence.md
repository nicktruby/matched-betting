# 0002. Represent money as integer pence, odds as integer milli-odds

- **Status:** accepted
- **Date:** 2026-09-05

## Context

This system computes stakes, liabilities and profit, and maintains a ledger that must
reconcile to the penny against real bookmaker and exchange balances.

The spreadsheet it replaces uses floating point throughout. Its profit columns contain values
like `-0.17879999999998475` and `32.490200000000002`. In a spreadsheet that is cosmetic noise.
In a ledger that reconciles against reality, it is a defect that compounds silently.

## Options considered

### Option A — Integer minor units (pence)
Every monetary value is an integer number of pence. Exact through every arithmetic operation
in JavaScript. The approach payment systems use.

### Option B — Database `NUMERIC` / `DECIMAL`
Exact in the database. But ORMs hand these back to JavaScript as strings, and any arithmetic
either converts to `number` — reintroducing float — or requires a decimal library throughout.
Exactness at rest, fragility in use.

### Option C — Floating point with rounding at the edges
What the spreadsheet does. Simplest to write and wrong in ways that only appear at
reconciliation time.

## Decision

**Money is `integer` pence. Odds are `integer` milli-odds** (5.4 → `5400`). **Commission is
`integer` basis points** (2% → `200`).

Conversion to and from display strings happens at the boundary, in one place.

## Why

Integer arithmetic in JavaScript is exact up to 2^53, which is £90 trillion in pence — not a
constraint here. Every intermediate calculation is therefore reproducible and auditable, which
matters because the balance check is the headline feature and a penny of drift per bet would
destroy it.

The argument against is ergonomics: `12000` is less readable than `£120.00`, and every
display path needs formatting. That is a real cost, paid once in a helper module, against a
class of bug that is very expensive to find later.

Milli-odds rather than decimals for the same reason — `(b × B) / (l − c)` must be reproducible,
and odds like 1.91 are not representable in binary floating point.

## Consequences

- A `money` module owns parsing, formatting and arithmetic. Nothing else constructs these
  values by hand.
- Division (which lay stake calculation requires) must round explicitly and deliberately.
  Rounding direction is a domain decision — underlay by default, see `docs/domain.md`.
- Database columns are `integer`, not `numeric` or `real`.
