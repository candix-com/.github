# Issues

How to file and keep an issue in every Candix repository that has no guide of its own.

## Before filing

Search open and closed issues for the same problem. If it is already filed, comment there instead of filing.

## Type

Pick the type when filing:

- `Bug`: the code doesn't do what it's meant to. It crashes, returns a wrong result, shows the wrong thing, or lets someone do what they shouldn't. For example: an expired sign-in code is still accepted.
- `Feature`: the code should do something it doesn't do yet, for users or for the team. For example: users can't delete their account.
- `Task`: anything that neither fixes a defect nor adds a capability. For example: a refactor, a dependency upgrade, a configuration change, a migration, or cleanup.

## Body

Write the body in this shape. The issue forms produce it; write it the same way when filing from the API:

```markdown
### The problem

What is wrong or missing, as facts.

### Impact

Who or what it hurts, and how. If users can hit a bug, assume they are hitting it.

### Context

What you were doing when you found it, and any idea about the cause or the fix, marked as speculation.
```

Once the issue is filed, don't edit the body: later comments answer it, and editing it breaks them. Add new facts as comments. If the comments change what the problem is, file a new issue for it and close this one as not planned, linking the new one.

## Fields

Set Priority and Effort when filing. Status and Assignee change as the work moves.

- **Priority**, from the Impact:
  - `Urgent`: security or data at risk, or a core flow broken with no workaround.
  - `High`: a core flow broken with a workaround, or another flow broken with no workaround.
  - `Medium`: another flow broken with a workaround, or a capability users need is missing.
  - `Low`: nothing users can notice.

  A core flow is what the product exists for: signing in, paying, and its main job.
- **Effort**, from how much has to be worked out:
  - `Low`: the change is fully known, and doing it is mechanical.
  - `Medium`: the outcome is known, but getting there takes design.
  - `High`: it changes something other code relies on, or the fix isn't known yet.
- **Status**:
  - `New`: leave it when filing. Triage moves it on.
  - `Waiting on owner`: set it when the issue can't move until the repository's owner answers a question or does something only they can do. Ask the question in a comment that mentions them. Set it back once they have.
  - `Ready`: set it when the issue is triaged and nothing about it is left to settle.
  - `In progress`: set it when you start the work.
- **Assignee**: assign the issue to the person or agent who will do the work, once that is decided. Leave it unchanged while the issue waits on someone else: Status says that.

## Links

- When the issue is a piece of work required to complete another, make it a sub-issue of that one: `gh issue edit <parent> --add-sub-issue <n>`.
- When the issue can't be finished until another one closes, mark it blocked by that one: `gh issue edit <n> --add-blocked-by <m>`. The other issue can be in any repository, including an upstream project: give its URL.
- When another issue is a different problem in the same area or with the same cause, link the two as related. `gh issue edit` has no option for it, so call the API with the other issue's id:

  ```bash
  id=$(gh api repos/<owner>/<repo>/issues/<m> --jq .id)
  gh api repos/<owner>/<repo>/issues/<n>/relates_to -F issue_id=$id
  ```

## Closing

- **When the work is done**: put `Closes #<n>` in the description of the pull request that does it, so merging it into the default branch closes the issue. If something else fixed it, close it as completed with a comment naming the commit or pull request.
- **When it won't be done, or is no longer a problem**: close it as not planned, with a comment that says why: `gh issue close <n> --reason 'not planned'`.
- **When another issue already describes the same problem**: copy anything this one adds into the other as a comment, then close this one as a duplicate of it: **Close as duplicate** on the issue page, or `gh issue close <n> --duplicate-of <m>`.
