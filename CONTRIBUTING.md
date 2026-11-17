# Contributing

How issues and pull requests are handled in every Candix repository. @tavoyne is the owner: they answer what only they can, and review and merge every pull request.

## Issues

### Filing

Search open and closed issues first. If the problem is already filed, comment there instead.

Otherwise, file it with a title, a type and a body, through the issue form, or by writing the body to a file and running `gh issue create --type <type> --title <title> --body-file <file>`, so its body never passes through a shell. Leave Priority, Effort and Triage to triage.

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

Pick the first that applies. Treat a bug users can hit as one they are hitting.

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

The Triage field says whether an issue can be worked:

- `Waiting`: the issue needs an answer only the owner can give. Set it, and ask in a comment that mentions them.
- `Ready`: nothing is left to settle, and the work can start.

Triage takes each open issue with no Triage value. It checks the issue against the code, digs for deeper problems, links related issues, and corrects the type, Priority and Effort where needed. Then it sets Triage to `Waiting` or `Ready`, or closes the issue, for example as a duplicate.

Priority, Effort and Triage are issue fields. `gh api orgs/{owner}/issue-fields` lists each field's id and options. To set one:

```bash
echo '{"issue_field_values": [{"field_id": <id>, "value": "<option>"}]}' | gh api repos/{owner}/{repo}/issues/<n>/issue-field-values --input -
```

Fields left out of the request keep their values, but an empty array clears them all. To clear one:

```bash
gh api -X DELETE repos/{owner}/{repo}/issues/<n>/issue-field-values/<id>
```

#### Assignee

The assignee is whoever does the work. For automated handling, leave it empty: it will be picked up and assigned at triage. If you're a human and intend to handle it yourself instead, assign yourself.

#### Updates

Add what you learn as a comment (`gh issue comment <n> --body-file <file>`), and leave the body as its filer wrote it. If the comments change what the problem is, file a new issue and close this one, as Closing says.

#### Links

- Blocked by: the issue makes sense on its own, but can't be finished until another closes. The blocker can be in any repository, an upstream project included; give its URL. It keeps its Triage value while it waits.

  ```bash
  gh issue edit <blocked> --add-blocked-by <blocker>
  ```

- Related: two issues are different problems in the same area or with the same cause.

  ```bash
  id=$(gh api repos/<other-owner>/<other-repo>/issues/<other> --jq .id)
  gh api repos/<owner>/<repo>/issues/<n>/relates_to -F issue_id=$id
  ```

### Closing

When an issue closes in favor of another, as a replacement or a duplicate, first move any issue it blocks onto the other.

- Fixed by a pull request: it closes when the pull request merges into the default branch.

- Fixed some other way: close it as completed, naming the commit or pull request.

- Won't be done, or no longer a problem: close it as not planned, saying why.

- Replaced by a new issue, because the problem changed: close it as not planned, linking the new one.

- Duplicate: comment on the other with anything this issue adds, then close this one as its duplicate.

  ```bash
  gh issue close <n> --duplicate-of <other>
  ```

## Pull requests

A pull request is squash-merged, with its title and description as the commit message.

### Title

One imperative sentence under 72 characters, with no trailing period, that says what the change does: "Post briefs entering review to the app events Slack channel", not "Slack alerts" or "Address review".

### Description

Write it in this shape, for a reviewer who wasn't there and a developer reading it months later. Leave out any section with nothing to say.

```md
### Problem

What was wrong or missing, and for whom, in a few sentences.

### What changes

The behavior or structure the pull request adds or changes. Use a table for several similar items.

### Worth knowing

One-line bullets for what a reviewer must not miss: limitations, deploy prerequisites, surprising behavior, anything not done or not verified.

### Reviewing

Where to look first, and how the change was checked, in a few sentences.

<details>
<summary>Choices</summary>

- Other design decisions a future reader may need, and review findings turned down, each with its reason.

</details>
```

- Keep it under 2,500 characters, not counting Choices.
- Outside Choices, mention a design decision only if a reviewer needs it to approve. Leave out how the design evolved.
- Put `Closes #<n>` in it for each issue the pull request resolves, or `Closes <owner>/<repo>#<n>` for an issue in another repository.
- Write plain sentences, with no em dashes.

### Size

- Make one change a reviewer can follow in one reading, targeting the default branch.
- Work bigger than one pull request is several issues, each blocked by the one it builds on.

### Working

- Open it as a draft.
- Push each update as a new commit. Don't amend, rebase or force-push.
- To bring in changes from the base branch, merge it in.
- When the pull request changes what it does, rewrite the title and description to match. Don't append to them.
- Mark it ready for review once the work is finished and the checks pass. The owner reviews it: they approve and merge it, or request changes in a review.

