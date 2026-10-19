---
alwaysApply: true
---

# Run Ledger Contract

Authority-of-record for `task_occurrences` in `/workspace/store/messages.db`, the host's scheduled-run ledger (`jbaruch/nanoclaw#1024`), under `coding-policy: stateful-artifacts`.

## Owner and schema

- Owner: the `jbaruch/nanoclaw` scheduler, through `src/db-task-occurrences.ts`; no container skill writes the table
- Schema of record: `src/state-migrations/state-022-task-occurrences.ts` and `state-024-task-occurrences-schema-version.ts` in `jbaruch/nanoclaw`
- One row per occurrence, keyed `(task_id, scheduled_for)`; `scheduled_for` is the task's `next_run` at dispatch
- A cadence-registry task's `task_id` is `cadence-registry::<group>::tessl__<skill>`
- Every timestamp is UTC ISO with milliseconds and `Z` (`2026-10-19T12:00:00.000Z`)
- Every record carries `schema_version` 1; a shape change bumps it in a new state migration

## Record fields

| Field | Type | Presence and meaning |
| --- | --- | --- |
| `group_folder` | TEXT | always; the task's group when the occurrence started |
| `attempts` | INTEGER | always, 1 or more |
| `started_at` | TEXT | always; the latest attempt's start |
| `progress_at` | TEXT | NULL until the run streams output |
| `ended_at` | TEXT | NULL while `running` |
| `state` | TEXT | `running`, then `completed`, `failed`, `killed` or `unknown` |
| `error` | TEXT | NULL on `running` and `completed` |
| `delivered_message_ids` | TEXT | JSON array of channel message ids; `[]` by default |
| `unverified_sends` | INTEGER | sends that returned no message id; 0 by default |

- `completed` is final; a retry restarts a `failed`, `killed` or `unknown` occurrence as `running` and counts `attempts`
- `unknown` means the host stopped before the run finished, never success

## Readers

- `jbaruch/nanoclaw-admin` heartbeat precheck: skips a missed-task recovery the ledger covers
- `jbaruch/nanoclaw-admin` system-audit: reports unverified sends
- Each reader names the versions it accepts and counts only rows of those versions
- A row of another version is no usable prior state for that reader, and the reader says so
- A missing table or column, or a query error, makes the ledger unreadable: heartbeat decides on the follow_me cursors alone and says so on stderr; system-audit records an `audit_errors` entry
- Readers are read-only and never migrate

## Rollout

- Owner and readers ship through separate pipelines: the host deploy and the admin plugin publish
- A version bump follows `coding-policy: stateful-artifacts` Cross-Pipeline Schema Bumps: readers accepting both versions publish first, then the writer flips, then readers drop the old version
- Every no-usable-state path only drops a suppression or adds an audit finding, never a wake-always or alert storm
