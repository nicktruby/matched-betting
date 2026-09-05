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

## How work happens here

**Read `CONTRIBUTING.md` before making changes.** It is the source of truth for process.
The rules most easily broken, stated here because this file is always loaded:

1. **`main` is protected.** Branch, then open a pull request. This applies to you.
2. **Open the PR and stop.** Never merge your own work.
3. **This repository is public.** Never commit real financial figures, credentials, or notes
   documenting a third party's security weakness or terms breach.
4. **Every real decision gets an ADR** in `docs/decisions/`, written before the code. `/new-adr`.
5. **Sign every comment you post.** Replies to pull request review threads, issue comments and
   PR comments all begin with `🤖 Claude:` followed by a line break, then the content. A reader
   should never have to work out whether a human or an agent wrote something.
6. **Answer review comments in their own thread**, one reply each — never a single summary
   comment. Make the change first, then reply describing what was done. `/address-review`.

