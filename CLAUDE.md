# Matched Betting

A personal matched-betting control centre. It replaces a hand-maintained spreadsheet
with an app and a database, and later automates the slow parts of the workflow.

**Current phase: 1 — Log a matched bet.** See `docs/roadmap.md`.

## Read these before proposing anything

| Document | What it holds |
|---|---|
| `docs/objectives.md` | Why this exists and how we know it worked |
| `docs/roadmap.md` | The phases, and what is deliberately *not* in each |
| `docs/domain.md` | Matched betting concepts and the maths. Read before touching any calculation. |
| `docs/decisions/` | Architecture decision records — the "why" behind every technical choice |
| `docs/backlog.md` | Researched but deferred. Check before re-investigating anything. |

## Rules that must not be broken

1. **Money is integer pence. Never floats.** Odds are integer milli-odds (5.4 → `5400`);
   commission is integer basis points (2% → `200`). The spreadsheet this replaces contains
   `-0.17879999999998475` in a profit column — that is the class of bug this prevents.
2. **The UI talks to the app only through its own HTTP API**, never straight to the database.
   Agents will call the same endpoints later; this makes that free rather than a refactor.
3. **The ledger is append-only.** Corrections are new adjustment rows. Balances are derived
   by summing entries, never stored and mutated.
4. **Domain logic stays free of I/O** — no database, no fetch, no UI. That is what makes the
   money maths exhaustively testable.

## How work happens here

**`main` is protected. Nothing is pushed to it directly — not by the user, not by an agent.**
Every change goes: branch → pull request → human review → merge. This is enforced by GitHub,
including for repository admins.

- Work on a branch named for what it does: `docs/…`, `feat/…`, `fix/…`, `chore/…`
- Open a PR and **stop**. Do not merge your own work; the review is the point.
- Link the PR to its issue where one exists.
- Every non-obvious technical choice gets an ADR in `docs/decisions/`. Use `/new-adr`.

## Other agreements

- Phases ship something usable. If a phase ends with nothing the user can actually do, it was
  scoped wrong.
- Issues are created for the phase currently being worked, not for phases far ahead. Stale
  issues describing work that has since changed are worse than no issues.

## This repository is public

- **Never commit real financial data** — balances, profits, account details. `.gitignore`
  excludes spreadsheets and databases, but the judgement is yours as well.
- **Never commit anything that documents a third party's security weakness**, or that records
  activity contrary to a service's terms. Keep such notes outside the repository.
- Illustrative figures in documentation should be recognisably illustrative.
