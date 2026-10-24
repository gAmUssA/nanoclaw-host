---
alwaysApply: false
applyTo: "src/group-snapshots*, src/ipc-deps*, src/task-scheduler*, src/message-pipeline*, container/agent-runner/src/format-task-row*, container/agent-runner/src/ipc-mcp-stdio.ts, skills/watchers/** — when writing or consuming host group/task projections"
---

# Group IPC Snapshots

## Owner and Versions

- Host `jbaruch/nanoclaw` owns `/workspace/ipc/current_tasks.json` and `/workspace/ipc/available_groups.json`; `src/group-snapshots.ts` writes both projections.
- Task snapshots retain a top-level array; version-1 records carry integer `schema_version: 1`.
- Each task record supplies `id`, `groupFolder`, `prompt`, `schedule_type`, `schedule_value`, `context_mode`, `status` and nullable `next_run`; `script` may be absent or null.
- `context_mode` is the actual `group` or `isolated` value, never a default inferred from prompt or script.
- Historical unversioned task records are legacy version 0; they supply no context-mode proof.
- Group snapshots carry integer `schema_version: 1` and a `groups` array; each group record carries integer version 1, `jid`, `isRegistered` and its registered `folder`.
- Ignore top-level JID-keyed agent scratch entries when resolving registered scope.

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
