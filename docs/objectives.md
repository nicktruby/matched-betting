# Objectives

## The problem

Matched betting is profitable but costs too much sitting-down time, so it doesn't get done.

The record-keeping itself is manual. Every bet is typed into a spreadsheet and balances are
hand-maintained. That is slow, it drifts, and it is open to human error.

## What success looks like

1. **Time saved.** More profit comes from completing more offers quickly than from chasing the
   maximum on any single one. Where speed and margin conflict, speed wins.

2. **Balance checking that works.** Checking balances is manual today. Since the ledger already
   records bets placed at bookmakers, it should be able to record the balances at those
   bookmakers too — which also requires logging deposits and withdrawals. Balance checking then
   becomes expected against actual, with corrections made for any mistakes rather than
   discrepancies quietly accumulating.

3. **A reviewable trail.** This is intended to be portfolio-reviewable, so it is run like a
   normal project at work: commits, pull requests and issue tracking. That history is evidence
   of the thinking — a large problem broken into measurable, achievable milestones — which is
   worth as much as the code.

## On the offer provider

This depends on a paid subscription to a matched betting service, and the intent is not to get
around what that subscription is for. Their value is the work they do finding offers and
matching them to exchanges, and this project is built to consume that through the channels
they provide.

Where automation touches their service, their terms govern. Anything unclear gets asked rather
than assumed. **This is a personal productivity tool**, not an attempt to extract something
that hasn't been paid for.

## Scope

In scope: price boosts, bet-and-get, refund-if, bet clubs, and bet builders matched against
correct-score markets.

Deferred: accumulators, 2Up, extra place. See `docs/domain.md`.

## Who uses it

The **ledger** is intended to support more than one person. It replaces a spreadsheet, and
that is a problem other people have — so it should not be built in a way that assumes a single
user. Every table carries a user from the start, even while there is only one.

The **automation** is personal. A local agent making decisions on a machine at home is not
something to share, and it has different constraints. The two talk over the app's HTTP API,
which means they can be deployed together or separately without changing either.

Authentication arrives when the app becomes reachable over a network — required at that point
whether there is one user or several, so multi-user support is largely a free rider on work
that has to happen anyway.

## Non-goals

- **Selling this.** The offer data comes from a paid third-party product whose terms rule it
  out. Sharing the ledger with someone who wants it is not the same as selling a product.
- **Replacing the offer provider.** Their offer discovery and odds matching is the thing being
  paid for. This system organises the work around it and keeps the records.
