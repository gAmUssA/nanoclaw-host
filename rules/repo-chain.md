---
alwaysApply: true
---

# Repository Chain

Each NanoClaw checkout has two roles:

- `origin`: Viktor's writable `gAmUssA/<repo>` fork.
- `upstream`: Baruch's read-only `jbaruch/<repo>` source.

Updates flow from `upstream/main` into a branch based on `origin/main`, then
through a pull request in Viktor's fork. Never merge upstream changes by
pushing directly to either repository's `main` branch.

```bash
git fetch origin
git fetch upstream
git switch main
git pull --ff-only origin main
git switch -c sync/upstream-YYYY-MM-DD
git merge upstream/main
git push -u origin sync/upstream-YYYY-MM-DD
gh pr create --repo gAmUssA/<repo> --base main --head sync/upstream-YYYY-MM-DD
```

Always give every `gh pr`, `gh issue`, `gh workflow`, and mutating `gh api`
command an explicit `--repo gAmUssA/<repo>`. A command that would address
`jbaruch/*` must stop before it performs a write.
