# Contributing

How issues and pull requests are handled in every Candix repository.

## Issues

### Filing

ALWAYS search open and closed issues first. If the problem is already filed, edit that issue or comment on it instead. Leave Priority, Effort and Triage to triage.

#### Type

- `Bug`: the code doesn't do what it's meant to, such as accepting an expired sign-in code.
- `Feature`: the code should do something it doesn't do yet, such as letting users delete their account.
- `Task`: any other work, such as a refactor, an upgrade or a cleanup.

#### Body

Write the body in this shape. Only Problem is required.

```md
### Problem

What is wrong or missing, as facts.

### Impact

Who or what it hurts, and how.

### Context

How it came up, and any guess at the cause or solution, marked as a guess.
```

#### Priority

How much the problem hurts users:

- `Urgent`: a security hole, data lost or wrong, or users can't use the product at all.
- `High`: users can't do something they need to, because of a bug or a missing capability.
- `Medium`: an improvement to something users can already do, or a bug in tests, tooling or the dev setup.
- `Low`: anything else, such as a cleanup, a refactor or a change to internals.

#### Effort

The expected number of changed lines, tests included:

- `High`: over 1,000.
- `Medium`: 100 to 1,000.
- `Low`: under 100.

### Working

#### Triage

- No value: the issue hasn't been triaged yet.
- `Waiting`: the issue depends on a human to move forward.
- `Ready`: nothing is left to settle, and the work can start.

#### Assignee

The assignee is whoever does the work. For automated handling, leave it empty: it will be picked up and assigned at triage. If you’re a human who plans to handle it yourself, assign yourself instead.

#### Updates

Edit the body while the issue has no comments. Once someone comments, add new information as a comment, so the conversation stays readable.

#### Links

- Blocked by: another issue, here or upstream, has to be resolved first.
- Related: another issue is a different problem in the same area or with the same cause.

### Closing

- Fixed some other way: close it as completed, naming the fix.
- Won't be done, or no longer a problem: close it as not planned, saying why.
- Replaced, because the problem changed: close it as not planned, linking the new issue.
- Duplicate: comment on the other issue with anything this one adds, then close this one as its duplicate.

## Pull requests

A pull request is squash-merged, with its title and description as the commit message.

### Title

One imperative sentence under 72 characters, with no trailing period, that says what the change does: "Post briefs entering review to the app events Slack channel", not "Slack alerts" or "Address review".

### Description

Write it for a reviewer who wasn't there and a developer reading it months later. Leave out any section with nothing to say.

```md
### Problem

What was wrong or missing, and for whom.

### What changes

The behavior or structure it adds or changes. Use a table for several similar items.

### Worth knowing

One-line bullets for what a reviewer must not miss: limitations, deploy prerequisites, surprising behavior, anything not done or not verified.

### Reviewing

Where to look first, and how the change was checked.

<details>
<summary>Choices</summary>

- Other design decisions a future reader may need, and review findings turned down, each with its reason.

</details>
```

- Keep it under 2,500 characters, not counting Choices.
- Outside Choices, mention a design decision only if a reviewer needs it to approve.
- Put `Closes #<n>` in it for each issue it resolves.
- Write plain sentences, with no em dashes.

### Size

Make one change a reviewer can follow in one reading.

### Working

- Keep it a draft until it can be merged.
- Push each update as a new commit. Don't amend, rebase or force-push.
- Keep the title and description true to the final change.
