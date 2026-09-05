---
description: Revise a pull request in response to review feedback, replying to every thread
---

Address the review on: $ARGUMENTS (default: the PR for the current branch)

## Gather

```
gh pr view --json number,title,url
gh api repos/{owner}/{repo}/pulls/{n}/comments --paginate \
  --jq '.[] | "\(.id)\t\(.path):\(.line // .original_line)\t\(.body)"'
gh api repos/{owner}/{repo}/pulls/{n}/reviews --jq '.[] | select(.body != "") | .body'
```

A thread with no reply whose `in_reply_to_id` is null is unaddressed. Work through those.

## Work through them

**Group related comments** and tell the user the grouping before starting — structural changes
first, since they often remove the need for detailed edits elsewhere. Then take them **one at
a time**.

For each, decide honestly which it is:

- **A correction** — the reviewer caught something wrong. Say so plainly. Do not soften it into
  a preference; a reviewer needs to know which comments caught real errors.
- **A preference** — apply it. Where they supplied wording, use theirs rather than rewriting it.
- **Something to push back on** — say so and explain why. Silent compliance is worse than
  disagreement, and a review is a conversation. If they reaffirm, do it their way.
- **A genuine question** — answer it. Show the working if there is any.

Where a decision changes scope or design, ask before acting rather than assuming.

## Reply

**Make the change first, then reply describing what was done.** Never post "will do".

**One reply per thread. Never a single summary comment** — a reviewer reading their own
comments should find the response attached to each one.

```
gh api -X POST repos/{owner}/{repo}/pulls/{n}/comments/{comment_id}/replies \
  --input - <<< '{"body": "..."}'
```

Every reply begins with `🤖 Claude:` and a line break. Be specific: what changed, and where.
Quote the resulting text when the reviewer supplied wording.

**Never resolve a thread.** Resolving is the reviewer's call.

## Finish

- Commit with a message explaining *why* the review changed things, not just what moved
- Push, so the PR reflects the replies
- **Never merge.** Open the PR and stop — that is the whole point of the gate
- Tell the user which replies pushed back or flagged errors, so they know where to look
