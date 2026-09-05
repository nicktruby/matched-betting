# Matched Betting

A personal control centre for matched betting: it keeps the records, does the maths, and
automates the parts that can safely be automated.

> **Status: early.** Phase 1 of six. This repository currently holds the design and decision
> record; application code follows.

## Why it exists

Matched betting is profitable, but it costs sitting-down time — so it doesn't get done. The
pattern in the spreadsheet this replaces is stark: the best month of the year cleared roughly
£900, while two other months cleared essentially nothing between them.

That variance isn't skill or margin. It's whether there was time. **The constraint is
attention, so that's what the system optimises for** — offers completed per unit of effort,
not profit squeezed out of each bet.

The second problem is bookkeeping. Everything currently lives in a spreadsheet with
hand-maintained balances, which drift. It presently disagrees with its own derived expectation,
and there's no way to find out why.

## What it does

- **Calculates** lay stakes and liability for qualifying bets, free bets, price boosts and
  refund-if offers, reporting the worst-case outcome rather than the average
- **Records** every matched bet in an append-only ledger, with balances derived rather than
  stored
- **Reconciles** expected position against real account balances, so mistakes surface instead
  of accumulating
- **Later:** ingests offers, places lay bets via an exchange API, and notifies a phone when
  something is worth doing

The back bet is always placed by a human. Soft bookmakers detect and ban automation, so that
step stays manual by design — everything around it is fair game.

## Design notes

The interesting decisions are written down rather than implied:

| | |
|---|---|
| [Objectives](docs/objectives.md) | The problem and how success is measured |
| [Domain](docs/domain.md) | Matched betting concepts and the maths |
| [Roadmap](docs/roadmap.md) | Six phases, each judged by what's usable when it ends |
| [Decisions](docs/decisions/) | Architecture decision records |
| [Backlog](docs/backlog.md) | Researched and deliberately deferred |

A few principles that shape everything else:

- **Money is integer pence, never floats.** The spreadsheet being replaced contains values
  like `-0.17879999999998475` in a profit column — that's the class of bug this prevents.
  ([ADR 0002](docs/decisions/0002-money-as-integer-pence.md))
- **The ledger is append-only.** Corrections are new entries, so any balance can be explained
  by decomposing it. ([ADR 0003](docs/decisions/0003-append-only-ledger.md))
- **The UI talks to the app only through its own HTTP API**, so automated clients later cost
  nothing. ([ADR 0004](docs/decisions/0004-ui-talks-to-app-via-http-api.md))

## Stack

Undecided, deliberately. Being chosen one decision at a time, each recorded as an ADR.

## Contributing

`main` is protected: every change arrives by pull request. Decisions of any significance get
an ADR before the code that implements them.
