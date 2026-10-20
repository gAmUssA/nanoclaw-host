---
name: ship-code
description: Ship a committed change through a branch and pull request in Viktor's gAmUssA fork. Use when asked to push a change, open a PR, merge reviewed work, or clean up a shipped branch. Never writes to Baruch's upstream repositories.
placement-admin-content-ok: true
skip-optimize: true
---

# Ship Code to Viktor's Fork

Use the current checkout's `origin` only after verifying it belongs to
`gAmUssA`. Treat `upstream` and every `jbaruch/*` repository as read-only.

```bash
origin_url=$(git remote get-url origin)
case "$origin_url" in
  *github.com*gAmUssA/*) ;;
  *) echo "refusing: origin is not a gAmUssA fork: $origin_url" >&2; exit 1 ;;
esac
repo=$(printf '%s
' "$origin_url" | sed -E 's|.*github.com[:/]([^/]+/[^/]+)(\.git)?$|\1|' | sed -E 's|\.git$||')
```

Require `repo` to equal `gAmUssA/<current-repository-name>`. Never push directly
to `main`.

```bash
git push -u origin <branch>
gh pr create --repo "$repo" --base main --head <branch> \
  --title "<type>(<scope>): <summary>" --body-file <prepared-body-file>
```

Run the repository's checks and keep every issue, comment, review, workflow, and
merge command explicitly scoped to `"$repo"`. Merge only after checks and review:

```bash
gh pr merge <number> --repo "$repo" --merge --delete-branch
git switch main
git pull --ff-only origin main
git branch -d <branch>
git remote prune origin
```

Never publish these Viktor-specific plugin changes under the `jbaruch` Tessl
namespace. Local file dependencies are the delivery source for this install.
