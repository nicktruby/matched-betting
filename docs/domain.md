# Domain: matched betting

Knowledge that every session needs and would otherwise be re-derived. **Read this before
touching any calculation.**

## The basic shape

A *matched bet* has two legs placed at the same time:

- **Back** — at a soft bookmaker, betting that something *will* happen. Placed by hand,
  always. Bookmakers detect and ban automation.
- **Lay** — at a betting exchange, betting that the same thing *won't* happen. This is the
  leg that can be automated.

Together they cancel out, so the outcome of the event does not matter. The profit comes from
the bookmaker's promotion, not from predicting anything.

## What the calculation has to handle

Each offer type stakes differently, and the lay stake follows from the back stake, both sets
of odds, and the exchange commission:

- **Qualifying bet** — the ordinary case. A small controlled loss, in exchange for unlocking
  the promotion.
- **Free bet, stake returned (SR)** — behaves like a qualifier.
- **Free bet, stake not returned (SNR)** — the common case. The bookmaker keeps the stake, so
  only the winnings are yours, and the lay stake is correspondingly smaller.
- **Price boost** — mechanically a qualifier at enhanced odds. The boosted price appears in no
  odds feed, so it can only come from the person placing the bet.
- **Refund-if** — genuinely different, see below.

The formulas themselves are standard and belong in tested code rather than in prose, where
they cannot be verified. Phase 1 specifies them as tests.

### Conventions that are decisions, not maths

**Refund-if staking depends on a judgement.** You stake so a winning back settles the offer
outright, while a losing back leaves you holding a free bet — so the correct stake depends on
what that free bet is actually *worth* when redeemed. Typically somewhere around 70–80% of its
face value, but that figure encodes your own ability to redeem it well. It must be a
configurable parameter, never a hardcoded constant.

**Report the worst case, never the average.** Exchanges round stakes to two decimal places, so
after rounding the two outcomes stop matching exactly. Both should be calculated and the worse
one shown.

**Balance both outcomes by default.** Lay so the bookmaker-wins and exchange-wins outcomes pay
the same. Underlaying or overlaying shifts money between those outcomes at **identical expected
value** — it only changes variance — so there is no long-run gain to be had by favouring
either side, and the default should be the one with no swings.

## Rating, SNR, and why the best price is the wrong target

Two numbers describe the same pair of odds:

```
rating = b / l          snr = (b − 1) / l
```

They differ only by `1/l`, so `rating = snr + 1/l`.

**Rating above 100% is arbitrage** — back odds exceed lay odds, so the bet profits whatever
happens. It is also the clearest signal a bookmaker has that an account is not a normal
punter, and it is the fastest route to being gubbed.

**So the objective is not the best available price.** It is a small, controlled loss that
looks like ordinary losing punting, while the profit comes from the promotion. An account that
survives is worth far more than an extra few pence per bet.

Expressed as SNR, the arbitrage threshold **moves with the odds**:

```
snr is arbitrage above (1 − 1/l) × 100
```

At lay odds of 5 that threshold is 80%; at 10 it is 90%; at 20 it is 95%. A fixed SNR target
that is safe at long odds can therefore be arbitrage at short ones — which is one reason free
bets are taken at high odds.

**The safe check is always on rating, whatever the bet type.** Target bands are a matter of
personal risk appetite and belong in configuration, not in this document.

## Offer types

| Type | Mechanics |
|---|---|
| **Bet-and-get** | Bet £X, receive a free bet. Qualifier, then the free bet. |
| **Price boost** | Enhanced odds on a selection, usually time-limited |
| **Refund-if** | Stake refunded as a free bet if a condition occurs |
| **Bet club** | Weekly recurring: bet £X across the week, get a free bet |
| **Bet builder / correct score** | Bookmaker combination bet, hedged against a correct-score market |

A bet builder's price isn't known until it has been constructed at the bookmaker, and it
lands slightly below the equivalent direct correct-score price.

### Deferred

- **Accumulators** — Smarkets supports multiples directly: select two or more selections,
  choose "multiple", and the resulting bet can be sold or laid. This is straightforward
  manually on their website. Whether it is reachable through their API is unconfirmed, and
  that is the open question rather than whether it is possible at all. The fallback technique,
  laying each leg as it settles, is stateful and awkward — liability shifts as legs land.
- **2Up** and **extra place** — not currently part of the user's routine.

## Gubbing

A bookmaker restricting an account: stake limits, removal from promotions, or closure.
It is the normal end state for a profitable matched bettor, and it is **per account, per
bookmaker**.

Consequences for the model:

- Accounts carry a status: active, restricted, gubbed, closed.
- Accounts are recorded by **account name**, which is enough to make ownership obvious from
  the email address used. Eligibility is per person — a signup offer cannot be claimed twice
  by the same individual — but that does not need a separate ownership model to represent.
  All accounts are one pot of money.
- A gubbed account must be excluded from offer suggestions for that bookmaker.

## Free bet lifecycle

`DUE → CREDITED → USED → EXPIRED`

Offer terms state both when a free bet arrives and how long it survives — *"by 18:00 on
Tuesday… must be used within 5 days"*. Both are parsed from the terms and stored, so the
follow-up task is **scheduled at the moment the qualifier is placed**, not discovered by
polling later.

## The spreadsheet being replaced

`~/Downloads/Matched Betting 2026.xlsx` (and the 2025 equivalent).

**Bet rows**, one per matched pair:
`STATUS · DAY · TYPE · BOOKIE · DESCRIPTION · BACK STAKE · ODDS · RETURNS · EXCHANGE ·
LAY ODDS · LAY STAKE · LIABILITY · RESULT · PROFIT/LOSS · RUNNING PROFIT`

with `TYPE ∈ {QUALIFIER, FREE BET}` and `RESULT ∈ {EXCHANGE WIN, BOOKIE WIN}`.

`DESCRIPTION` packs two fields into one — `"19:25 Tramore | MISS GITANA"`. **Store event and
selection separately.** The capitals are visual hierarchy and belong in CSS.

**`ACCOUNTS`** — per account: balance, exposure, pending withdrawals, a gubbed flag, and a
note of which person the account belongs to.

**`SUMMARY`** — where the real requirement lives:

```
Combined Total Profits    X       ← sum of all logged bets
Withdrawals              -Y       ← money taken out
Expected Balance        X-Y       ← what should be held across all accounts
Actual Balance            Z       ← summed from ACCOUNTS
                        ─────
discrepancy       (X-Y) - Z       ← must be explainable, not absorbed
```

**This reconciliation is the feature**, not the profit total. The live spreadsheet currently
shows a non-zero discrepancy that cannot be investigated, which is the problem being solved.

Other sheets: `DAILY FREE GAMES` (a separate small income stream, not matched bets) and an
`EXTRA PLACES` section.
