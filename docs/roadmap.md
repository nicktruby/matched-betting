# Roadmap

Each phase is judged by one question: **what can actually be used when it ends?** A phase
that finishes with nothing the user can do was scoped wrong.

Phases 1–3 are the MVP. They replace the spreadsheet and touch no external service — no
Outplayed, no exchange, no agents.

---

## Phase 1 — Log a matched bet

Enter back stake, back odds and lay odds. It calculates lay stake, liability and profit/loss
for both outcomes, saves the bet, and lists what's been logged.

Replaces **both** Outplayed's calculator and the month sheet in the spreadsheet, which is why
it's first — it's useful on its own, on day one.

Includes the full calculation library with tests: qualifier, SR, SNR, price boost, refund-if
with the retention parameter, rounding behaviour and worst-case reporting.

*Not in this phase:* accounts, balances, history, offers, notifications.

## Phase 2 — Accounts and balances

Bookmaker and exchange accounts with owner and gubbed status. Deposits and withdrawals.
Balances **derived** from logged bets and money movements, never hand-typed. And the balance
check: expected position against actual observed balances, with the gap surfaced and
resolved by an explicit adjustment that records *why*.

Replaces the `ACCOUNTS` and `SUMMARY` sheets.

## Phase 3 — Import history

Import the 2025 and 2026 workbooks: every bet row, mapped to ledger entries, with event and
selection split apart.

**The spreadsheet is retired at the end of this phase.** That is the milestone that matters.

Expect a residual in the balance check afterwards — the workbooks record bets but not
deposits and withdrawals beyond an aggregate. Explaining or writing off that residual is the
first real use of the reconciliation feature.

## Phase 4 — Free bets and a todo list

Free bets tracked through due → credited → used → expired, with expiry warnings. A manual
queue of work: add a task, order it, work through it, mark it done.

Still no external integration — tasks are entered by hand. This proves the workflow shape
before anything automates it.

## Phase 5 — Phone and notifications

Progressive web app, installed to the home screen, with Web Push. This is where the
"open phone, place bet, confirm, done" experience becomes real.

Requires HTTPS — a secure context is mandatory for service workers and push, and an iPhone
on a plain local address will never receive a notification. How that is served is a hosting
decision recorded in `docs/decisions/`.

## Phase 6+ — Automation

Offers ingested automatically; lay bets placed via an exchange API. This is where the earlier
design work went, and where all of the terms-of-service and integration complexity lives.

**Deliberately last.** It is the most fragile part, the most externally dependent, and it
delivers nothing until everything above it works. Manual entry from Phase 1 remains the
permanent fallback.

See `docs/backlog.md` for what has already been researched here — including existing
matched-betting skills in this environment that may already cover parts of it.

---

## Principles that apply from Phase 1

**The UI talks to the app only through its own HTTP API.** Never straight to the database.
Agents calling those same endpoints later then costs nothing.

**Money is integer pence. Never floats.** See `docs/domain.md`.

**Ship the smallest thing that is genuinely usable**, then use it. Phases exist to force that.
