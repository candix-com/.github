# Issues

How to file and keep an issue in every Candix repository that has no guide of its own.

## Before filing

Search open and closed issues for the same problem. If it is already filed, comment there instead of filing.

## Type

Pick the type when filing:

- `Bug`: the code doesn't do what it's meant to. For example, an expired sign-in code is still accepted.
- `Feature`: the code should do something it doesn't do yet. For example, users can't delete their account.
- `Task`: any other work. For example, a refactor, an upgrade or a cleanup.

## Body

Write the body in this shape. The issue forms produce it; write it the same way when filing from the API:

```markdown
### The problem

What is wrong or missing, as facts.

### Impact

Who or what it hurts, and how. If users can hit a bug, assume they are hitting it.

### Context

What you were doing when you found it, and any guess at the cause or fix, marked as such.
```

Once the issue is filed, don't edit the body: later comments answer it, and editing it breaks them. Add new facts as comments. If the comments change what the problem is, file a new issue for it and close this one as not planned, linking the new one.

## Fields

Set Priority and Effort when filing.

- **Priority**, from the Impact:
  - `Urgent`: a security hole, data lost or wrong (money included), or users can't use the product at all.
  - `High`: a bug users can hit: something they do fails or gives a wrong result.
  - `Medium`: a missing capability users need, or a bug only the team can hit (tests, tooling, dev setup).
  - `Low`: nothing anyone notices: cleanup, refactors, internals.
- **Effort**, from how much has to be worked out:
  - `Low`: the change is fully known, and doing it is mechanical.
  - `Medium`: the outcome is known, but getting there takes design.
  - `High`: it changes something other code relies on, or the fix isn't known yet.
- **Status**:
  - `New`: leave it when filing. Triage sets the next one.
  - `Waiting`: set it when only the repository's owner can unblock the issue, and ask them in a comment that mentions them. Set it to `Ready` once they have.
  - `Ready`: set it when the issue is triaged and nothing is left to settle.
  - `Started`: set it when you start the work.
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

- **When a pull request resolves the issue**: put `Closes #<n>` in its description. Merging it into the default branch closes the issue.
- **When something else already fixed it**: close it as completed, with a comment naming the commit or pull request.
- **When it won't be done, or is no longer a problem**: close it as not planned, with a comment that says why: `gh issue close <n> --reason 'not planned'`.
- **When another issue already describes the same problem**: copy anything this one adds into the other as a comment, then close this one as a duplicate of it: **Close as duplicate** on the issue page, or `gh issue close <n> --duplicate-of <m>`.
