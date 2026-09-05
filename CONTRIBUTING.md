# Contributing

This is a personal project, but it's run like a shared one — partly because it's better
practice, partly because the history is meant to be readable by someone else later.

## Workflow

`main` is protected. Every change arrives by pull request, including from the repository
owner and including from AI agents. This is enforced by GitHub, not by convention.

```
branch → commit → pull request → review → merge
```

Branch names describe the work:

| Prefix | For |
|---|---|
| `feat/` | new capability |
| `fix/` | something broken |
| `docs/` | documentation only |
| `chore/` | tooling, config, dependencies |
| `refactor/` | behaviour unchanged |

**Nobody merges their own work without reading the diff.** For AI agents, this is absolute:
open the pull request and stop. The review is the point of the process, not a formality.

## Commit messages and PR titles

[Conventional Commits](https://www.conventionalcommits.org): `type(scope): description`.

```
feat(ledger): add lay stake calculation
fix: correct arbitrage threshold for SNR bets
docs: record the offer provider position
```

| Type | For |
|---|---|
| `feat` | a new capability |
| `fix` | a bug fix |
| `docs` | documentation only |
| `test` | adding or correcting tests |
| `refactor` | neither fixes a bug nor adds a feature |
| `perf` | performance |
| `style` | formatting only, no behaviour change |
| `build` | build system or dependencies |
| `ci` | CI configuration |
| `chore` | anything else not touching source or tests |
| `revert` | reverts an earlier commit |

`!` before the colon marks a breaking change: `feat(ledger)!: …`

**Scope is the area of code** — `ledger`, `agent`, `domain` — not a ticket number. It is
optional, and worth using once there is more than one package.

**PR titles follow the same convention.** `main` requires linear history, so merges are
squashed, and the repository is configured to build the squash commit from the PR title and
body. Your PR title becomes permanent history.

**Issue references go in the footer, never the subject:**

```
fix: correct arbitrage threshold for SNR bets

The threshold varies with lay odds rather than being fixed at a single
percentage, so a target safe at long odds is arbitrage at short ones.

Closes #42
```

`Closes`, `Fixes` and `Resolves` auto-close the issue on merge. `Refs:` links without closing.

They stay out of the subject because GitHub appends the *pull request* number there
automatically, and issues and pull requests share one numbering sequence — two `#N` references
in one line would be indistinguishable. The log shows the PR; the PR links the issue.

## Pull requests

- Link the PR to its issue where one exists
- Explain *why*, not just what — the diff already shows what
- Where a change involved a judgement call, say what the alternative was

## Decisions

Any choice with a real alternative gets an architecture decision record in
`docs/decisions/`, written **before** the code that implements it. Use `/new-adr` to
scaffold one.

Decisions are not edited once accepted. A decision that turns out to be wrong is superseded
by a new record, so the history of what was believed and when stays intact.

## Issues

Issues are created for the phase currently being worked, not for phases far ahead. A backlog
full of issues describing work that has since changed is worse than no backlog. See
`docs/roadmap.md` for what's coming.

## What must never be committed

This repository is **public**.

- **Real financial data** — balances, profits, account details. Illustrative figures in
  documentation should be recognisably illustrative.
- **Anything documenting a third party's security weakness**, or recording activity contrary
  to a service's terms. Such notes stay outside the repository.
- **Credentials of any kind.** `.gitignore` covers the obvious cases, but it is not a
  substitute for judgement.
