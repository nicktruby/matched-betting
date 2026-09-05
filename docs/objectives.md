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

## Non-goals

- **Multi-user support.** Single user. No auth, roles or tenancy.
- **Selling this.** The offer data comes from a paid third-party product whose terms rule it
  out. Personal tool only.
- **Replacing the offer provider.** Their offer discovery and odds matching is the thing being
  paid for. This system organises the work around it and keeps the records.
