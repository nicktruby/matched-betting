# Domain: matched betting

Knowledge that every session needs and would otherwise be re-derived. **Read this before
touching any calculation.**

## The basic shape

A *matched bet* has two legs placed at the same time:

- **Back** — at a soft bookmaker, betting that something *will* happen. Placed by hand,
  always. Bookmakers detect and ban automation.
- **Lay** — at a betting exchange, betting that the same thing *won't* happen. This is the
  leg that can be automated.

Together they cancel out, so the outcome of the event barely matters. The profit comes from
the bookmaker's promotion, not from predicting anything.

## Notation

`B` back stake · `b` back odds (decimal) · `l` lay odds (decimal) · `c` commission as a
fraction (2% → `0.02`)

Stored as integers: money in **pence**, odds in **milli-odds** (5.4 → `5400`), commission in
**basis points** (2% → `200`).

## Lay stake formulas

**Qualifying bet, or a free bet where the stake is returned (SR):**

```
layStake  = (b × B) / (l − c)
liability = layStake × (l − 1)
```

**Free bet, stake not returned (SNR)** — the common case. The bookmaker keeps the stake, so
only the winnings are yours:

```
layStake  = ((b − 1) × B) / (l − c)
```

**Price boost** — identical to the qualifier formula, using the *boosted* odds. The subtlety
is that the boosted price is not in any odds feed, so `b` must come from the user.

**Refund-if** — a genuinely different formula, not a variation. You stake so that a winning
back settles it outright, while a losing back leaves you holding a free bet with its own
value:

```
layStake = (b × B − R × r) / (l − c)
```

`R` is the refund's face value and `r` is **retention** — what a free bet is actually worth
when redeemed, typically 0.70–0.80. Retention is a *configurable parameter*, not a constant:
it encodes a judgement about your own redemption ability and it moves the stake materially.

## Outcomes

Always compute **both** and report the worst case, never the average.

| | Qualifier / SR | Free bet SNR |
|---|---|---|
| Bookmaker wins | `B(b−1) − layStake(l−1)` | `B(b−1) − layStake(l−1)` |
| Exchange wins | `layStake(1−c) − B` | `layStake(1−c)` |

Exchanges round stakes to 2dp, so after rounding the two outcomes stop matching exactly.
**Underlay by default** — round the lay stake down, which biases the residual toward the
bookmaker-wins side.

## Rating and SNR percentage

Outplayed's oddsmatcher publishes two numbers. Both assume **0% commission**:

```
rating = (b / l)     × 100      → for qualifiers; want close to or above 100
snr    = ((b − 1)/l) × 100      → for free bets; want as high as possible
```

Verified against live data: back 1.91 / lay 1.76 → rating 108.52; snr 51.7.

The two objectives pull in opposite directions. Qualifiers want *low* odds with a tight
back/lay gap, minimising qualifying loss. Free bets want *high* odds, because SNR retention
rises with odds.

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

- **Accumulators** — no exchange offers a single acca lay. The technique is laying each leg
  as it settles, which *is* possible via API but is stateful, with liability shifting as legs
  land. Hard, not impossible.
- **2Up** and **extra place** — not currently part of the user's routine.

## Gubbing

A bookmaker restricting an account: stake limits, removal from promotions, or closure.
It is the normal end state for a profitable matched bettor, and it is **per account, per
bookmaker**.

Consequences for the model:

- Accounts carry a status: active, restricted, gubbed, closed.
- Accounts carry an **owner**. Eligibility is per person: a signup offer cannot be claimed
  twice by the same individual, and a gubbing applies to one person at one bookmaker. The
  ledger treats all accounts as one pot of money while tracking ownership separately, so
  offers can be matched to accounts that are actually eligible for them.
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
