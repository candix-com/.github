# Contributing

How issues and pull requests are handled in every Candix repository, unless its own CONTRIBUTING.md says otherwise.

## Issues

### Filing

Search open and closed issues first. If the problem is already filed, comment there instead.

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

Who or what it hurts, and how. Treat a bug users can hit as one they are hitting.

### Context

How it came up, and any guess at the cause or solution, marked as a guess.
```

#### Priority

Pick the first that applies:

- `Urgent`: a security hole, data lost or wrong, or users can't use the product at all.
- `High`: users can't do something they need to, because of a bug or a missing capability.
- `Medium`: an improvement to something users can already do, or a bug in tests, tooling or the dev setup.
- `Low`: anything else, such as a cleanup, a refactor or a change to internals.

#### Effort

Pick the first that applies:

- `High`: the solution isn't known yet, or the change alters something other code relies on.
- `Medium`: the outcome is known, but reaching it takes design.
- `Low`: the change is fully known, and making it is mechanical.

### Working

#### Status

- `New`: the issue isn't triaged yet. Every issue is filed with it.
- `Waiting`: only @tavoyne can unblock the issue. Set it, and ask in a comment that mentions them.
- `Ready`: the issue is triaged, and nothing is left to settle.
- `Started`: the assignee is doing the work. Set it when you start.

#### Assignee

The assignee does the work. Assign @owreed, unless the work needs access @owreed lacks, such as creating a resource in Azure, signing up for a vendor account or changing a repository's branch rules. Then assign @tavoyne.

#### Comments

Don't edit the body once the issue is filed: the comments answer it as written. Add new facts as comments. If the comments change what the problem is, file a new issue and close this one as not planned, linking the new one.

#### Links

- Sub-issue: the issue only makes sense as part of another.

  ```bash
  gh issue edit <parent> --add-sub-issue <child>
  ```

- Blocked by: the issue makes sense on its own, but can't be finished until another closes. The blocker can be in any repository, an upstream project included; give its URL.

  ```bash
  gh issue edit <blocked> --add-blocked-by <blocker>
  ```

- Related: two issues are different problems in the same area or with the same cause.

  ```bash
  id=$(gh api repos/<owner>/<repo>/issues/<other> --jq .id)
  gh api repos/<owner>/<repo>/issues/<n>/relates_to -F issue_id=$id
  ```

### Closing

- Fixed by a pull request: put `Closes #<n>` in its description, or `Closes <owner>/<repo>#<n>` for an issue in another repository. The issue closes when the pull request merges into the default branch.

- Fixed some other way: close it as completed, naming the commit or pull request.

- Won't be done, or no longer a problem: close it as not planned, saying why.

- Duplicate: comment on the other with anything this issue adds, then close this one as its duplicate.

  ```bash
  gh issue close <n> --duplicate-of <other>
  ```

## Pull requests

### Opening

- A pull request makes one change a reviewer can hold in their head at once. When the description can't fit it, split it into pull requests stacked on each other.
- Open it as a draft. Mark it ready only when nothing is left but approval: the work is finished, the checks pass, and `/reviewers:review` has ended clean.
- Push each change as a new commit. Don't amend or force-push: the pull request is squash-merged, with its title and description as the commit message.

### Title

One imperative sentence under 72 characters, with no trailing period, that says what the change does: "Post briefs entering review to the app events Slack channel", never "Slack alerts" or "Address review".

### Description

Write it for a reviewer who wasn't there, and for a developer reading it months later. Use this shape, leaving out any section with nothing to say:

```md
### Problem

What was wrong or missing, and for whom, in a few sentences.

### What changes

The behavior or structure it adds or changes. A table when listing several similar items.

### Worth knowing

One-line bullets for what a reviewer must not miss: limitations, deploy prerequisites, surprising behavior, anything not done or not verified.

### Reviewing

Where to look first, and how the change was checked, in a few sentences, including whether `/reviewers:review` ended clean.

<details>
<summary>Choices</summary>

- Each other design decision a future reader may need, including review findings turned down, with its reason.

</details>
```

- Keep it under 2,500 characters, not counting Choices.
- Outside Choices, mention a design decision only if a reviewer needs it to approve.
- Leave out how the design evolved.
- When review changes the pull request, rewrite the description to match. Never append to it.
- Plain sentences, no em dashes.
