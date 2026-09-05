---
description: Open a pull request for the current branch, following the repo's conventions
---

Raise a pull request for the current branch. $ARGUMENTS

## Before opening anything

1. **Check you are not on `main`.** If you are, the work needs moving to a branch first —
   `main` is protected and will reject the push anyway.
2. **Read your own diff.** `git diff main...HEAD`. Look for anything that shouldn't be there:
   debug output, commented-out code, real financial figures, credentials, notes about a third
   party's security weaknesses. See `CONTRIBUTING.md`.
3. **Check the branch name** matches the work: `feat/`, `fix/`, `docs/`, `chore/`, `refactor/`.

## Title

Conventional Commits — `type(scope): description`. See `CONTRIBUTING.md` for the type list.

The repository squashes on merge and builds the commit from the PR title and body, so
**the title becomes permanent history**. Write it as the commit message you want to keep.

No issue number in the title.

## Body

Explain **why**, not what — the diff already shows what. Include:

- What problem this solves, in a sentence or two
- Any judgement call made, and what the alternative was
- Anything you are unsure about, or specifically want looked at
- `Closes #N` for the issue it completes, or `Refs: #N` to link without closing

Where a change was made because a reviewer asked, say so. Where it was a correction to
something previously wrong, say that too — it is more useful than "updated".

## Open it

```
git push -u origin <branch>
gh pr create --base main --title "..." --body "..."
```

## Then stop

**Do not merge.** Report the URL and what you would look at first if you were reviewing it.
Merging is the reviewer's decision — see `/merge-pr`.
