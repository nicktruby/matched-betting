# Backlog

Researched and deliberately deferred. **Check here before re-investigating anything** — this
was established during design and should not be re-derived.

None of it is needed before Phase 6.

---

## Offer ingestion

The offer provider's terms restrict automated access to their service. **Before building any
automated ingestion, this needs resolving with them directly** — the reasonable first step is
simply asking whether a paying subscriber may automate their own workflow.

Until that is settled, the design assumes:

- Manual offer entry is a permanent, first-class path — not scaffolding to be removed later
- Any integration is isolated behind a single module so it can be replaced or dropped without
  touching anything else
- Failures degrade to manual entry rather than breaking a queue item

Detailed technical findings are kept out of this repository deliberately.

## Existing tooling to evaluate first

Three matched-betting skills already exist in the author's environment and may cover parts of
Phase 6 before any of it is built. **Evaluate before writing anything new:** offer checking
and prioritisation, lay placement through an existing exchange integration, and tracking of
restricted bookmaker accounts.

## Exchange APIs

Comparison from public documentation, for whenever automated lay placement is built.

**Matchbook** — free API, session-token auth, full coverage: prices and depth, order
submission and cancellation, positions, settled bets, balances. Rate limits are generous
except on reporting endpoints, which caps how often settlement can be polled. Exposes a
**heartbeat** endpoint — a dead-man's switch that cancels unmatched orders if the client stops
responding, which is the right safety net for unattended placement.

**Smarkets** — one-time administration fee plus a discretionary approval process. API
customers are ineligible for affiliate commission offers, so an existing 0% rate would revert
to standard commission. Terms restrict use to pre-approved markets and treat high volumes of
price requests without corresponding bets as grounds for termination.

**Betfair** — highest base commission of the three, reducible only by accruing loyalty points,
plus a substantial one-time fee for a live application key. Not a candidate.

**BetConnect** — peer-to-peer bet requests rather than an order book. Not automatable the same
way.

### On accepting commission

Commission-induced qualifying loss is `B × c × (b−1) / (b−c)` — roughly 2% of back stake at
even odds. At realistic stake sizes that is pennies per offer, or low tens of pounds across a
month. Set against months where nothing was completed at all, throughput is obviously the
right variable to optimise. See `objectives.md`.

## Execution safety rules

To apply if and when lay placement is automated. These are the rules that prevent losing real
money, and none of them are optional:

- **Limit orders only.** A market order into a thin book is an unbounded loss, and observed
  liquidity is frequently only a few pounds.
- **Idempotency key per attempt**, sent to the exchange. A retry after a timeout must never
  create a second lay. This is the largest financial risk in the system.
- **Write the bet record before calling the exchange**, never after, so a crash mid-placement
  leaves something reconciliation can find.
- **Cancel any unmatched remainder in the same operation** — never leave orders resting.
- **Fetch depth only at execution time**, never on a polling loop. Every price query should
  have a bet behind it.
- **Caps enforced server-side** per order and per day, plus a global kill switch checked
  immediately before every submission.
- **Not hedged until matched size covers required size.** Partial fills escalate to the user
  rather than being treated as complete.

## Architecture sketch

Deferred, but the shape arrived at during design:

- A resolver process owns one warm browser session and **polls the app for work** rather than
  being called. No inbound connection to a home network is needed, and it works whether the
  app runs locally or is deployed.
- Deterministic scripts handle anything with a known shape. An LLM belongs at the front door
  only — turning offer prose into structured parameters — and never on the money path.
- Scheduled work (ingestion, notifications, settlement) is cron-shaped and tolerates minutes
  of latency. Only lay placement is latency-sensitive, and it happens inside a request the
  user triggered.
