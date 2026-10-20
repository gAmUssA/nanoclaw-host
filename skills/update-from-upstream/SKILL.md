---
name: update-from-upstream
description: Bring changes from Baruch's read-only upstream into Viktor's gAmUssA fork through a reviewable sync branch and pull request. Use when asked to update NanoClaw or one of its plugin forks.
---

# Update Viktor's Fork from Upstream

The current checkout must have writable `origin` under `gAmUssA/*` and a
fetch-only `upstream` under `jbaruch/*`. Stop if either relationship differs.

```bash
origin_url=$(git remote get-url origin)
upstream_url=$(git remote get-url upstream)
case "$origin_url" in *github.com*gAmUssA/*) ;; *) exit 1 ;; esac
case "$upstream_url" in *github.com*jbaruch/*) ;; *) exit 1 ;; esac

git fetch origin
git fetch upstream
git switch main
git pull --ff-only origin main
git switch -c sync/upstream-YYYY-MM-DD
git merge upstream/main
```

Resolve conflicts deliberately and run the repository's checks. Then push only
the sync branch to Viktor's fork and create a PR there:

```bash
git push -u origin sync/upstream-YYYY-MM-DD
gh pr create --repo gAmUssA/<repo> --base main \
  --head sync/upstream-YYYY-MM-DD --body-file <prepared-body-file>
```

Never push to `upstream`, address a mutating `gh` command to `jbaruch/*`, or
publish a Viktor-specific plugin to Baruch's Tessl namespace.
