# Task Restart Recovery

## Owner and schema

- Artifact: `task_restart_recoveries` in `/workspace/store/messages.db`.
- Owner: `jbaruch/nanoclaw` scheduler through `src/db-task-occurrences.ts`.
- Schema of record: `src/state-migrations/state-026-task-restart-recoveries.ts`.
- Every record carries integer `schema_version` 1.
- Key: `(task_id, scheduled_for, attempt)`; attempt is the occurrence's attempt number before retry.
- Required fields: `group_folder`, `started_at`, `interrupted_at`.
- All timestamps are canonical UTC ISO with exactly milliseconds and `Z`.
- An interval means the host lost an attempt's terminal result. A detached worker may remain alive; it is never evidence of successful delivery or actual worker termination.

## Writer

- Snapshot supported running occurrences before retry can replace their start timestamps.
- Insert intervals and mark those occurrences `unknown` in one transaction.
- Do not modify completed occurrences or unsupported occurrence versions.
- Reject noncanonical, invalid or future start times as unusable intervals; emit a recovery diagnostic and mark the occurrence unknown without inventing a time window.
- A persistence failure rolls back the whole operation and prevents dispatch until startup can retry. Ordinary delivery telemetry remains best-effort.
- Preserve intervals while their task exists; task deletion removes its intervals through the foreign key.
- Only the owner writes or migrates the table. Container readers never mutate it.

## Email reader

- `jbaruch/nanoclaw-admin` `skills/email-surface/scripts/email_surface_retry.py` reads supported intervals through its precheck.
- Accept only version 1, the exact task/group identity, canonical ordered timestamps, and positive integer attempts.
- Use the interval only with the email owner's durable emission receipt and an exact matching live seen row; preserve foreign and subsequently refreshed claims.
- A later successful or skipped retry does not hide an unresolved restart interval.
- Unknown record versions are unusable; skip them with an actionable diagnostic. Do not migrate or downgrade them.
- A missing table during rollout is normal and retains the existing failed-run-log recovery path. No wake-always, global deletion or alert storm fallback.
- A malformed supported row or query failure reports recovery uncertainty and releases no claims from that row.
- Repeating recovery is safe: an emission inserted after the old interval cannot match that interval's ownership proof.

## Rollout and verification

- Publish/install this authority first.
- Publish and deploy the email reader that accepts both the pre-table host and version-1 interval records.
- Deploy the host migration/writer after that reader is present.
- Verify restart-to-retry recovery, exact millisecond boundaries, unsupported versions, foreign/successful claims, and atomic failure before dispatch with deterministic fixtures.
- This contract does not authorize full worker reattachment, external replies, or an automatic resend of an already delivered message.
