# Objectives

## The problem

Matched betting is profitable but costs too much sitting-down time, so it doesn't get done.
The pattern in the tracking spreadsheet is stark: the best month of the year cleared roughly
£900, while two other months cleared essentially nothing between them. That variance is not
skill, luck or margin — it is whether there was time to sit down and work through offers.
**The binding constraint is attention, not edge.**

A second, quieter problem: the record-keeping itself is manual. Every bet is typed into a
spreadsheet, and balances are hand-maintained. That is slow, and it drifts — the 2026 sheet
currently shows an unexplained discrepancy between expected and actual position, with no
way to investigate where it came from.

## What success looks like

**Primary: offers completed per unit of attention.** Not profit per offer. A system that
turns a June into an average month is worth far more than one that squeezes an extra 2% out
of each bet. When a trade-off appears between speed and margin, speed wins.

**Secondary: a balance check that can be trusted.** The user's own description of what they
actually want: *"I like to do a balance check and ensure everything is accurate, then I know
I haven't made any mistakes anywhere."* Expected position derived from the ledger, compared
against real account balances, with any gap surfaced and explained rather than absorbed.

**Tertiary: portfolio-quality code.** Git-tracked from the first commit, with decisions
recorded and reasoning visible. This is intended to be readable by someone else.

## Non-goals

- **Maximising profit per offer.** Explicitly traded away for throughput.
- **Selling this.** The offer data comes from a paid third-party product whose terms rule it
  out. This is a personal productivity tool.
- **Multi-user support.** Single user. A `user_id` column exists from day one as cheap
  insurance, but no auth, roles or tenancy is built.
- **Replacing Outplayed.** Their offer discovery and odds matching is the product being paid
  for. This system organises the work around it and keeps the records.

## Scope of the betting itself

In scope: price boosts, bet-and-get, refund-if, bet clubs, and bet builders matched against
correct-score markets.

Deferred: accumulators, 2Up, extra place. See `docs/domain.md` for why accumulators are hard
rather than impossible.
