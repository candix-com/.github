# Issues

How an issue is filed and kept in every Candix repository that has no guide of its own.

## Types

- `Bug`: the code doesn't do what it's meant to.
- `Feature`: the code should do something it doesn't do yet.
- `Task`: any other piece of work, such as a refactor, setup or cleanup.

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

- **Status**: `New` until it is triaged, then `Waiting on owner`, `Ready` or `In progress`. Closing the issue ends it.
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
- **Assignee**: whoever finishes the work.

## Links

- **Sub-issue**: only a piece of work required to complete the parent.
- **Blocked by**: only when the issue can't be finished until another one closes, in any repository, including upstream projects.
- **Related**: a different problem in the same area or with the same cause.
- **Duplicate**: the same problem. Close the issue as a duplicate of the other.

## Closing

A pull request closes the issues it resolves by saying `Closes #<n>` for each. Close any other issue as completed or not planned, with a comment that says why.
