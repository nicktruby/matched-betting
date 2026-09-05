# Matched Betting

A personal control centre for matched betting: it keeps the records, does the maths, and
automates the parts that can safely be automated.

> **Status: early.** Phase 1 of six. This repository currently holds the design and decision
> record; application code follows.

## Why it exists

Matched betting is profitable but costs sitting-down time, so it doesn't get done. More profit
comes from completing more offers quickly than from chasing the maximum on any single one — so
**time saved is what this optimises for**.

The second problem is bookkeeping. Everything currently lives in a spreadsheet with
hand-maintained balances. That is slow, it drifts, and it is open to human error.

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

## Stack

Undecided, deliberately. Being chosen one decision at a time, each recorded as an ADR.

## Contributing

`main` is protected: every change arrives by pull request. Decisions of any significance get
an ADR before the code that implements them.
