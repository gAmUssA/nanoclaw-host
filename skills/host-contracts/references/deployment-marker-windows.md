# Deployment Window Markers

## Owner and paths

The canonical `jbaruch/nanoclaw` deployment entrypoint `scripts/deploy.sh` owns
these two append-only operational logs through `src/deploy-private-state.py`:

| Host path relative to checkout | Agent read-only projection | Meaning |
| --- | --- | --- |
| `data/host-logs/deploy-kills.log` | `/workspace/host-logs/deploy-kills.log` | Deployment force-kill interval for expected exit-137 failures |
| `data/host-logs/tessl-update-window.log` | `/workspace/host-logs/tessl-update-window.log` | Plugin-update interval for temporary registry-missing failures |

These markers grant no effect, approval, delivery or task-completion authority.
The Admin heartbeat uses matching intervals only to suppress their named expected
failure classes. Other failures and failures outside a window remain visible.

## Record formats

- Legacy records contain two ISO-8601 timestamp strings separated by one tab,
  followed by a newline. This implicit legacy format remains readable unchanged.
- New records are one JSON object per line with exactly `schema_version`, `start`
  and `end`. `schema_version` is the integer `1`; both timestamps are strings.
- Every timestamp must include a timezone. Writers emit UTC ISO-8601 with `Z`.
- `start` must be no later than `end`.
- Reject duplicate JSON fields, extra fields, invalid timestamp types and unknown
  versions. JSON booleans are not integer schema versions.
- Empty lines carry no record. Mixed legacy and schema-1 history is supported.
- Preserve historical bytes. Append only; never truncate, rewrite or silently
  migrate legacy or unknown records.

## Private writer boundary

- Run canonical deployment as the configured non-root NAS operator matching the
  actual normalized numeric agent UID/GID; refuse root before Git or build work.
- Use public umask `022` for Git checkout and builds; use private umask `077` for
  host operational state. Container-owned installation retains its own public
  artifact permission contract.
- Create and verify the log directory at `0700` and both selected logs at `0600`.
- Update existing files only through held descriptors after regular-file,
  single-link, exact-owner and mount/attachment checks. Verify final numeric
  UID/GID and modes. Preserve bytes except the intended append.
- Scope repairs to these two logs and the selected genuine registered SDK's
  `_close` input. Apply `agent-file-permissions` to that separate sentinel.
- Refuse unexpected links, owners, types or mount crossings; do not chown or
  recursively repair state.
- Report native subprocess failure category, exit status and sanitized diagnostic
  without exposing private prose, paths or credentials.

## Reader and rollout

- `jbaruch/nanoclaw-admin`'s `skills/heartbeat/scripts/heartbeat-checks.py` reads
  both formats without modifying either file.
- A missing file means no recorded window. An unreadable existing file, invalid
  UTF-8 or any malformed/unsupported record means unknown marker coverage,
  including unknown records older than the retained comparison lookback.
- Return an actionable issue rather than healthy/empty coverage. Never silently
  ignore an unknown record or use partial history to claim complete coverage.
- Readers may discard valid old intervals from memory after validation; retain
  the original file unchanged.
- Publish the dual reader and this Host contract before the Core schema-1 writer.
  Verify actual installed reader/contract bytes before native deployment.
- Source review and publication do not establish native installed acceptance.
  Permission and suppression acceptance require the actual configured identity.
