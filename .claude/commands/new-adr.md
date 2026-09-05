---
description: Scaffold a new architecture decision record in docs/decisions/
---

Create a new ADR for: $ARGUMENTS

1. Find the highest existing number in `docs/decisions/` and use the next one, zero-padded
   to four digits.
2. Copy `docs/decisions/_template.md` to `docs/decisions/NNNN-<kebab-case-title>.md`.
3. Fill in the title, set status to `proposed` and the date to today.
4. Draft the Context section from what you know of the project — read `docs/objectives.md`
   and `docs/roadmap.md` if you need the framing.
5. List the genuine alternatives with their real trade-offs. Do not invent weak options to
   make the preferred one look better; if there is only one viable option, say so and explain
   why.
6. Leave Decision, Why and Consequences for discussion with the user unless the choice has
   already been made in conversation.

Then show the user the draft and ask what they want to change before setting status to
`accepted`.
