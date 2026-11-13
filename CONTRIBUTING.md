# Issues

How an issue is filed and kept in every Candix repository that has no guide of its own.

## Types

- **Bug**: the code doesn't do what it's meant to.
- **Feature**: the code should do something it doesn't do yet.
- **Task**: any other piece of work, such as a refactor, setup or cleanup.

## The body

- **The problem**: what is wrong or missing, as facts. Mark any idea about the cause or the fix as speculation ([Mozilla](https://bugzilla.mozilla.org/page.cgi?id=bug-writing.html)).
- **Found while**: what you were doing when you found it.

Search open and closed issues before filing. Whoever filed an issue keeps its body up to date; everyone else comments.

## Fields

- **Status**: New until it is triaged, then Waiting on owner, Ready or In progress. Closing the issue ends it.
- **Priority**: Urgent when something users rely on is broken now. Otherwise High, Medium or Low, by how it ranks against the rest of the backlog.
- **Effort**: Low is under an hour, Medium under half a day, High more.
- **Assignee**: whoever finishes the work.

## Links

- **Sub-issue**: only a piece of work required to complete the parent.
- **Blocked by**: only when the issue can't be finished until another one closes, in any repository, including upstream projects.
- **Related**: a different problem in the same area or with the same cause.
- **Duplicate**: the same problem. Close the issue as a duplicate of the other.

## Closing

A pull request closes the issues it resolves by saying `Closes #<n>` for each. Close any other issue as completed or not planned, with a comment that says why.
