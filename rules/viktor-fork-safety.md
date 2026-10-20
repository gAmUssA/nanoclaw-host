---
alwaysApply: true
---

# Viktor Fork Safety

All GitHub writes for this installation belong in Viktor's `gAmUssA/*` forks.
This covers pushes, branches, pull requests, issues, comments, releases, tags,
workflow dispatches, and automated backups.

Repositories owned by `jbaruch` are upstream sources. Fetch and compare them,
but never push, publish, open a pull request, create an issue, comment, or
trigger a workflow there. Never run `tessl publish` for a `jbaruch/*` package.

Before a GitHub mutation:

1. Resolve the current `origin` owner and require `gAmUssA`.
2. Pass `--repo gAmUssA/<repo>` to every mutating `gh` command.
3. Stop if the destination is absent, ambiguous, or owned by `jbaruch`.

Local checkouts enforce the same boundary with `remote.pushDefault=origin`, a
disabled upstream push URL, and a pre-push hook that rejects `jbaruch/*`.
