---
alwaysApply: false
applyTo: "src/group-snapshots*, src/ipc-deps*, src/task-scheduler*, src/message-pipeline*, container/agent-runner/src/format-task-row*, container/agent-runner/src/ipc-mcp-stdio.ts, skills/**, rules/learning-evidence-runtime.md — when writing or consuming host group/task projections, including learning evidence and watcher readers"
---

# Group IPC Snapshots

## Owner and Versions

- Host `jbaruch/nanoclaw` owns `/workspace/ipc/current_tasks.json` and `/workspace/ipc/available_groups.json`; `src/group-snapshots.ts` writes both projections.
- Task snapshots retain a top-level array; version-1 records carry integer `schema_version: 1`.
- Version-1 task records follow the field contract below; no field receives an invented default.
- `context_mode` is the actual `group` or `isolated` value, never a default inferred from prompt or script.
- Legacy task records have absent `schema_version` or integer `schema_version: 0` and the same original fields below except `context_mode`; that field is absent or ignored in version 0 and supplies no context-mode proof.
- Group snapshots carry integer `schema_version: 1`, a `groups` array and UTC ISO-8601 `lastSync`; each group record explicitly carries integer `schema_version: 1` and the field contract below.
- Ignore top-level JID-keyed agent scratch entries when resolving registered scope.
- Malformed legacy source values may be copied unchanged; they supply no usable effect proof and receive no fabricated identity, context, timestamp or other default.

| Task field | Presence, JSON type and format for a usable record |
| --- | --- |
| `schema_version` | Required integer 1 for version 1; absent or integer 0 for legacy version 0 |
| `id` | Required nonempty string containing the exact native task id; opaque, no default |
| `groupFolder` | Required string naming the registered folder; native folder syntax is `[A-Za-z0-9][A-Za-z0-9_-]{0,63}`, with reserved `global` excluded |
| `prompt` | Required stored prompt string; historical serialized Buffer values may be displayed defensively but never satisfy an exact string binding |
| `script` | Optional string or null; omission/null means no script, and an empty string stays distinct |
| `schedule_type` | Required string `cron`, `interval` or `once` |
| `schedule_value` | Required string: native cron expression, decimal interval milliseconds or native ISO-8601 once value; legacy once values may use host-local time |
| `context_mode` | Required version-1 string `group` or `isolated`; absent/ignored in version 0 |
| `status` | Required stored string `active`, `paused` or `completed`; cancellation removes the task |
| `next_run` | Required native ISO-8601 timestamp string or null; no fabricated next run |

| Group field | Presence, JSON type and format |
| --- | --- |
| `schema_version` | Required integer literal 1 on every record |
| `jid` | Required nonempty opaque native chat-id string |
| `name` | Required native display-name string; no identity or approval authority |
| `lastActivity` | Required native ISO-8601 activity timestamp string |
| `isRegistered` | Required boolean |
| `folder` | Required registered folder string when `isRegistered` is true; otherwise optional, with no default; same folder syntax as task records |
| `containerConfig` | Optional host registration-config object; never caller-supplied role authority |
| `requiresTrigger` | Optional host registration boolean; no synthesized default |

## Visibility and Context

- Main sees all task records; trusted groups see their own; untrusted groups receive no task snapshot.
- Only main receives registered-group records; other trusted groups receive an empty group array and untrusted groups receive no projection.
- The host supplies `NANOCLAW_CHAT_JID`; its SDK context supplies `NANOCLAW_GROUP_FOLDER` and `NANOCLAW_IS_MAIN` from the registered group.
- Prechecks may run before SDK-only context exists; use only the host's version-1 registered-group projection for their folder lookup.
- Never synthesize role variables or derive human identity, owner approval or tool authority from a snapshot.

## Reader and Effect Proof

- Accept task record versions 0 and 1 read-only; refuse unsupported versions and malformed, partial, linked or concurrently changed gate input.
- Require explicit version 1 for group context; missing versions supply no registered-scope proof.
- Require version 1, the exact own task id/group, active status, schedule, isolated context, script and prompt before accepting an isolated watcher binding.
- Verify snapshot freshness against the actual scheduling attempt or binding before effect readback.
- A fresh complete absence after that operation can confirm no task remains without issuing another cancellation.
- After issuing native cancellation, require fresh authoritative removal before reporting that operation successful.
- Native own-task cancellation uses `{type: "cancel_task", taskId}`; host `src/ipc-handlers/tasks.ts` derives authorization from the source folder, never caller-supplied role flags.
- These projections grant no write or migration access to the underlying host database.

## Rollout and Verification

- Publish and install this authority before enabling cross-plugin consumers.
- Deploy dual-accept readers before flipping a writer; retain old-version support through the compatibility window.
- Canonical NanoClaw deployment builds the agent reader image before the orchestrator writer; verify the actual installed reader and writer before enabling watchers.
- Keep a new producer inactive when its required proof is unavailable; never invent missing context or versions.
- Regression coverage: `jbaruch/nanoclaw` `src/group-snapshots.test.ts` and `container/agent-runner/src/format-task-row.test.ts`; watcher consumer coverage lives in `jbaruch/nanoclaw-admin` `tests/test_watcher_workflow.py`.
