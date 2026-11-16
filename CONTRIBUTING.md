# Contributing

How issues and pull requests are handled in every Candix repository.

## Issues

### Filing

Search open and closed issues first. If the problem is already filed, comment there instead.

Otherwise, write the issue as JSON to a file with an editor or a file-writing tool, not the shell. That way its text never passes through a shell, and each field id sits in one object with its value. Create the issue with one request, which also sets the fields below:

```json
{
  "title": "<title>",
  "body": "<body>",
  "type": "<type>",
  "issue_field_values": [
    { "field_id": 36918742, "value": "<priority>" },
    { "field_id": 36918745, "value": "<effort>" }
  ]
}
```

```bash
gh api repos/candix-com/<repo>/issues --input <file>
```

To set fields on an existing issue, write `{ "issue_field_values": [...] }` to a file and post it to the issue's `issue-field-values` endpoint. Fields the file leaves out keep their values, but an empty array clears them all.

```bash
gh api repos/candix-com/<repo>/issues/<n>/issue-field-values --input <file>
```

To clear a field:

```bash
gh api -X DELETE repos/candix-com/<repo>/issues/<n>/issue-field-values/<field-id>
```

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

#### Status

- `New`: the status every issue is filed with.
- `Waiting`: the issue needs an answer or action only @tavoyne can give. Set it, and ask in a comment that mentions them.
- `Ready`: nothing is left to settle, and the work can start.
- `Started`: the assignee is doing the work. Set it when you start.

#### Triage

Once a day, triage takes each `New` issue. It checks the issue against the code, digs for deeper problems, links related issues, and corrects the type, Priority and Effort where needed. Then it sets the issue to `Waiting` or `Ready`, or closes it, for example as a duplicate.

#### Assignee

The assignee does the work. Assign @owreed, unless the work needs access @owreed lacks, such as creating a resource in Azure, signing up for a vendor account or changing a repository's branch rules. Then assign @tavoyne.

#### Updates

Edit the body only while the issue has no comments. Once it has one, the comments answer the body as written, so add new facts as comments instead. If the comments change what the problem is, file a new issue and close this one, as Closing says.

#### Links

- Sub-issue: the issue only makes sense as part of another.

  ```bash
  gh issue edit <parent> --add-sub-issue <child>
  ```

- Blocked by: the issue makes sense on its own, but can't be finished until another closes. The blocker can be in any repository, an upstream project included; give its URL. It keeps its status while it waits.

  ```bash
  gh issue edit <blocked> --add-blocked-by <blocker>
  ```

- Related: two issues are different problems in the same area or with the same cause.

  ```bash
  id=$(gh api repos/<other-owner>/<other-repo>/issues/<other> --jq .id)
  gh api repos/<owner>/<repo>/issues/<n>/relates_to -F issue_id=$id
  ```

### Closing

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

Where to look first, and how the change was checked, in a few sentences, including whether `/reviewers:review` ended clean.

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

- Make one change a reviewer can follow in one reading.
- Split a bigger change into stacked pull requests, each based on the branch of the one before.

### Working

- Open it as a draft.
- Push each update as a new commit. Don't amend, rebase or force-push.
- To bring in changes from the base branch, merge it in.
- When the pull request changes what it does, rewrite the title and description to match. Don't append to them.
- Mark it ready for review once the work is finished, the checks pass, and `/reviewers:review` has ended clean: every finding fixed, or turned down and listed in Choices. @tavoyne reviews, approves and merges it.
