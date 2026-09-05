# 0003. Append-only ledger with derived balances

- **Status:** accepted
- **Date:** 2026-09-05

## Context

The system must answer two questions at any time: what is my total profit, and how much
should be in each account right now? The second is the one the user actually cares about —
they reconcile it against real balances to confirm no mistakes have been made.

The spreadsheet maintains balances by hand in an `ACCOUNTS` sheet. It currently disagrees with
its own derived expectation by about £105, with no way to find out why.

## Options considered

### Option A — Append-only ledger, balances derived by summation
Every money movement is an immutable row. An account balance is `SUM(amount)` over its
entries. Corrections are new adjustment rows carrying a reason.

### Option B — Mutable balance column per account
Update the balance as bets settle. Simple and fast to read. Provides no history, no way to
explain a discrepancy, and a bug silently corrupts state with nothing to recompute from.

### Option C — Ledger plus a cached balance column
Both, with the cache updated on write. Faster reads, at the cost of a cache that can diverge
from its source.

## Decision

**Append-only ledger. Balances are derived, never stored and mutated.** Rows are never updated
or deleted; corrections are new `ADJUSTMENT` entries that record why.

## Why

The reconciliation feature is only meaningful if the expected figure can be *explained*. A
derived balance can always be decomposed into the entries that produced it; a mutated column
cannot. When the check shows a £105 gap, the ledger can show which entries contribute and what
is missing — which is exactly the question the user is asking.

Immutability also makes settlement replay-safe: posting the same event twice is a no-op if
entries carry an idempotency key, which matters once settlement is automated.

The argument against is read performance, and it is not serious at this volume — a few
thousand rows a year. If it ever matters, Option C becomes available *without* changing the
source of truth, which is the point.

## Consequences

- No `UPDATE` or `DELETE` on ledger entries, ever. Enforceable in review and by convention.
- Every entry carries an idempotency key, so replaying settlement is safe.
- The balance check compares derived position against a user-entered observed balance, and any
  gap is closed by an explicit adjustment rather than by editing history.
- Deposits and withdrawals are first-class entries — without them, derived balances cannot be
  correct.
