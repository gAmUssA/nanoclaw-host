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
- New appends use schema 1. Only the canonical owner migrates accepted legacy
  records through the migration procedure below. Readers remain read-only.
- Preserve unknown records unchanged; never truncate or silently reinterpret them.

## Owner migration

- `prepare-logs` and each append validate both complete logs before changing
  their permissions or active bytes. Refuse malformed, unknown-version or
  invalid UTF-8 history without altering either original.
- Bound each original and migrated log to 16 MiB. Oversized history requires
  inspection and remains unchanged; never truncate to fit.
- Serialize supported owner operations through an exclusive held log-directory
  lock. Reuse fixed numeric-owner, attachment and mount checks.
- For every log containing legacy records, preserve its exact original bytes in
  `<log-name>.legacy-<sha256-of-original-bytes>.archive` inside the private log
  directory. These archives are inactive evidence, never suppression inputs.
- Create archives exclusively at `0600`, verify numeric owner, regular type,
  single link, mount, named attachment and exact bytes, then fsync file and
  directory before publishing any replacement. Reuse an existing archive only
  after the same complete verification; never overwrite a conflicting archive.
- Convert every accepted interval to schema 1, preserving timestamp strings,
  order and duplicates across the entire history. Keep schema-1 intervals too.
- Write the complete replacement to an exclusive private temporary file in the
  held directory. Verify its bytes and descriptor metadata, recheck original
  bytes and named attachments, then atomically replace the fixed active log.
- Verify final attachment, owner and `0600` mode, and fsync the directory.
- Publication failure retains the original or verified published replacement
  and private evidence. A later explicit owner invocation verifies any retained
  archive before resuming; it never resets history or replays an effect.
- Already-versioned valid logs retain their bytes and inode. Empty logs need no
  migration. Never migrate another path or recursively repair state.

## Private writer boundary

- Run canonical deployment as the configured non-root NAS operator matching the
  actual normalized numeric agent UID/GID; refuse root before Git or build work.
- Use public umask `022` for Git checkout and builds; use private umask `077` for
  host operational state. Container-owned installation retains its own public
  artifact permission contract.
- Create and verify the log directory at `0700` and both selected logs at `0600`.
- Update existing files only through held descriptors after regular-file,
  single-link, exact-owner and mount/attachment checks. Verify final numeric
  UID/GID and modes. Preserve bytes except the intended append or verified
  owner migration above.
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
- Dual-format reading is transitional: publish the dual reader and this Host
  contract first, then deploy the owner migration and schema-1 writer.
- Drop legacy acceptance only after actual native proof confirms both active
  logs are versioned and exact originals remain in inactive private archives.
- Verify actual installed reader/contract bytes before native deployment.
- Source review and publication do not establish native installed acceptance.
  Permission and suppression acceptance require the actual configured identity.
