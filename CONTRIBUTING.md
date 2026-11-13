# Issues

How an issue is filed and kept in every Candix repository that has no guide of its own.

## Types

- `Bug`: the code doesn't do what it's meant to. It crashes, returns a wrong result, shows the wrong thing, or lets someone do what they shouldn't. For example: an expired sign-in code is still accepted.
- `Feature`: the code should do something it doesn't do yet, for users or for the team. For example: users can't delete their account.
- `Task`: work that neither fixes a defect nor adds a capability. For example: a refactor, a dependency upgrade, a configuration change, a migration, or cleanup.

## The body

The issue forms write the body in this shape. An issue filed any other way uses it too:

```markdown
### The problem

What is wrong or missing, as facts.

### Impact

Who or what it hurts, and how. If users can hit a bug, assume they are hitting it.

### Context

What you were doing when you found it, and any idea about the cause or the fix, marked as speculation.
```

## Before filing

Search open and closed issues for the same problem. If it is already filed, comment there instead.

## After filing

- Never rewrite the body: it breaks the thread that follows.
- New facts go in comments.
- When the comments change what the problem is, it is a different issue. Close this one and file the new one.

## Fields

- **Status**: where the issue stands.
  - `New`: nobody has triaged it yet.
  - `Waiting on owner`: it can't move until the repository's owner answers a question or does something only they can do.
  - `Ready`: triaged and clear. It can be worked once nothing open blocks it.
  - `In progress`: someone is working on it.

  There is no `Done` status: a finished issue is closed, as Closing below says.
- **Priority**:
  - `Urgent`: security or data at risk, or a core flow broken with no workaround.
  - `High`: a core flow broken with a workaround, or another flow broken with no workaround.
  - `Medium`: another flow broken with a workaround, or a capability users need is missing.
  - `Low`: nothing users can notice.

  A core flow is what the product exists for: signing in, paying, and its main job.
- **Effort**, by how much has to be worked out:
  - `Low`: the change is fully known, and doing it is mechanical.
  - `Medium`: the outcome is known, but getting there takes design.
  - `High`: it changes something other code relies on, or the fix isn't known yet.
- **Assignee**: the one person or agent who does the work and opens its pull request. It stays the same while the issue waits on someone else, since Status says that. An issue nobody has taken is unassigned.

## Links

- **Sub-issue**: only a piece of work required to complete the parent. `gh issue edit <parent> --add-sub-issue <n>`.
- **Blocked by**: only when the issue can't be finished until another one closes. The other issue can be in any repository, including an upstream project, given by its URL. `gh issue edit <n> --add-blocked-by <m>`.
- **Related**: a different problem in the same area or with the same cause. `gh issue edit` has no option for it, so call the API with the other issue's id:

  ```bash
  id=$(gh api repos/<owner>/<repo>/issues/<m> --jq .id)
  gh api repos/<owner>/<repo>/issues/<n>/relates_to -F issue_id=$id
  ```

## Closing

An issue is closed for one of three reasons:

- **Completed**: the work is done. A pull request that says `Closes #<n>` in its description closes the issue when it merges into the default branch. An issue fixed some other way is closed as completed with a comment naming the commit or pull request that fixed it.
- **Not planned**: it won't be done, or it is no longer a problem. Close it as not planned with a comment that says why: `gh issue close <n> --reason 'not planned'`.
- **Duplicate**: another issue already describes the same problem. Copy anything this one adds into the other as a comment, then close it as a duplicate of the other: on the issue page, **Close as duplicate**, or `gh issue close <n> --duplicate-of <m>`.
