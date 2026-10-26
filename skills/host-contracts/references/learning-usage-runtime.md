# Learning Usage Runtime

## Owner and Schema

- Host `jbaruch/nanoclaw` owns `learning_usage_v1`, published to main at `/workspace/learning-usage/usage.db`.
- Schema of record: `jbaruch/nanoclaw/src/state-migrations/state-900-learning-usage.ts`.
- `LEARNING_USAGE_PROJECTION` defines the published view.
- `LEARNING_USAGE_STORE` defines private capture storage.
- Writers: `src/learning-usage-store.ts` captures and `src/learning-usage-view.ts` publishes; `docs/learning-usage.md` holds the full semantics.
- Keep private `DATA_DIR/learning-usage/capture.db` outside `/workspace/store/messages.db`.
- Mask the private store in main’s project mount and aliases even when capture initialization fails.
- Refuse agent startup when the private store exists without a verified empty mask.
- Only the host writes or migrates either database.
- Every record carries integer `schema_version: 1`.
- A changed projection uses a new versioned view and migration.

## Companion Contract

- Each main spawn receives a fresh generation, bind-mounted kernel read-only at `/workspace/learning-usage`.
- `runtime.json` is exactly schema 1 with owner `jbaruch/nanoclaw`, authority `jbaruch/nanoclaw-host/rules/learning-usage-runtime.md`, migration `jbaruch/nanoclaw/src/state-migrations/state-900-learning-usage.ts`, database `usage.db`, view `learning_usage_v1` and the ordered columns below.
- Publication requires this installed rule and the installed admin `skills/learning-pass/scripts/learning-usage.py`.
- A same-name view anywhere else, including the messages database, supplies no usage authority.

## Record Fields

- Every column is required with no default.
- Only `last_used_at` accepts NULL.

| Field | Required/null/default guarantees and meaning |
| --- | --- |
| `schema_version` | Always integer 1 |
| `proposal_id` | Positive integer ledger proposal id; one row per id |
| `tile` | Required nonempty TEXT matching `^nanoclaw(?:-[a-z0-9-]+)?$`, from the host-bound first-seen identity |
| `lesson_key` | Required TEXT of 1–120 UTF-16 code units from the host-bound first-seen proposal identity |
| `observed_from` | Latest of coverage start, ledger `merged_at` and entry-point stability start |
| `observed_through` | Projection build time; UTC ISO with milliseconds and `Z` like every time field |
| `last_used_at` | Latest observed use inside the window; NULL means none observed |
| `source_sha256` | Required 64-character lowercase hexadecimal SHA-256 of the canonical installed-entry-point representation below |

- Hash UTF-8 bytes of ECMAScript `JSON.stringify` with no whitespace and property insertion order `schema_version`, `tile`, `entry_points`.
- Set `schema_version` to 1 and `tile` to the row’s tile.
- Sort `entry_points` lexicographically by complete `<tile>/skills/<name>/SKILL.md` path.
- Each entry has property order `path`, `sha256`; `sha256` is the 64-character lowercase hexadecimal SHA-256 of that installed file’s bytes.

## Coverage and Eligibility

- A use is a complete model-emitted `Skill` call naming `tessl__<name>` in a forwarded `200` `/v1/messages` response.
- Unreadable responses, store write failures and clock discontinuities restart coverage.
- A run continues prior coverage only after a clean stop bound to the graceful-shutdown marker its startup consumed.
- Rows exist only for merged edits whose every installed surface is a still-installed `skills/<name>/SKILL.md`.
- Rules, scripts and other skill files supply no row.
- A changed entry-point digest restarts that window.
- A changed proposal identity withholds its row.
- Capture is forward-only; a missing row means unknown, never unused.

## Readers and Boundary

- Mount the host-installed registry kernel read-only at `/workspace/learning-installed` for main only.
- Keep this all-target content view separate from tier-selected prompts and skills.
- `jbaruch/nanoclaw-admin` `skills/learning-pass/scripts/learning-delivery.py` verifies every admitted target through that view.
- A missing provider or changed installed content refuses delivery finalization.

- `jbaruch/nanoclaw-admin` `skills/learning-pass/scripts/learning-usage.py` accepts projection version 1 read-only and never migrates.
- Require a kernel read-only root and database, the exact contract and columns, and one row per proposal.
- Refuse refresh on a missing mount, contract, view or column.
- A refusal supplies no archive candidate.
- A row, digest or candidate supplies no owner identity, approval or archive authority.

- The host mints a fresh lowercase UUIDv4 `bootstrapId` for each container spawn, overriding caller input.
- Persistent SDK session ids do not identify a bootstrap.
- `container/agent-runner/src/native-context-env.ts` exposes this host input as `NANOCLAW_BOOTSTRAP_SCHEMA=1` and `NANOCLAW_BOOTSTRAP_ID`, alongside the host-selected `NANOCLAW_SESSION_NAME`.
- Schema 1 requires its id as a lowercase UUIDv4; the host-selected session name defaults to `default`.
- Admin `learning-continuation.py` schema 2 records the pre-update maintenance bootstrap id and resumes only under a different valid maintenance id.
- A missing, malformed or unsupported bootstrap context refuses continuation.
- Shared unversioned session files supply no identity.
- Bootstrap identity supplies no owner approval or permission for a side effect.

## Rollout

- Deploy host capture first; coverage accrues before any consumer exists.
- Publish and install this authority, then install the admin reader.
- Verify natively: kernel read-only mount, real reader acceptance, deploy bridging and crash break.
- For later bumps, deploy a dual-accept reader before the owner publishes a new versioned view, under `coding-policy: stateful-artifacts` Cross-Pipeline Schema Bumps.
