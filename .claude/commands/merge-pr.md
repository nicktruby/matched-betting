---
description: Merge an approved pull request, once the user has asked for it
---

Merge the pull request: $ARGUMENTS (default: the PR for the current branch)

## This command runs only when the user invokes it

`CLAUDE.md` says never merge your own work. That still holds — this command exists because
**the user invoking it is the authorisation**, not because merging is now permitted generally.
Never run it off your own judgement, and never as the last step of doing the work.

If the user has not explicitly asked for a merge, stop.

## Check before merging

```
gh pr view <n> --json title,mergeable,mergeStateStatus,reviewDecision,statusCheckRollup
gh api repos/{owner}/{repo}/pulls/<n>/comments --paginate \
  --jq '[.[] | select(.in_reply_to_id == null)] | length'
```

- **Every review thread has a reply.** Unanswered feedback should not be merged past.
- **Checks are green**, if any exist.
- **No unresolved conversations** — the branch requires resolution before merge.
- **The title follows Conventional Commits**, because it becomes the commit message.
  Fix it before merging, not after.

Report anything failing rather than merging around it.

## Merge

```
gh pr merge <n> --squash --delete-branch
```

Squash, because `main` requires linear history. The branch is deleted automatically.

## Afterwards

```
git checkout main && git pull
```

Confirm the resulting commit message reads correctly — this is the first chance to see what
the squash actually produced. Tell the user if it came out differently from the PR title, as
that indicates a repository setting needs adjusting.
