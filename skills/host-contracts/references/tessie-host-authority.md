# Tessie host authority, schema 1

Producer: `jbaruch/nanoclaw` `src/host-plugins/tessie/`. Consumer:
`jbaruch/nanoclaw-tesla` deterministic scripts. Default disabled
(`TESSIE_ENABLED=true` is required). This producer creates no OneCLI actor,
credential or grant and performs no provider operation on import or registration.

## Identity and protected policy

Native `tessie_request` uses `IpcHandlerContext.sourceGroup`, verified from the IPC
task directory. Resolve exactly one current registered JID for that folder,
rechecking after native I/O and before dispatch. Caller chat IDs and role fields
are never authority. No main-chat bypass exists.

Host owner provisions `DATA_DIR/tessie-authority` at `0700`, owned by effective
host UID, and empty child `.agent-empty` at `0555`. Existing unsafe directories
block agent startup even when the feature is disabled or initialization fails.
`volume-mounts.ts` masks this directory in main's project view and additional
project aliases; direct additional mounts of private authority are refused.

`policy.json` is a regular single-link host-owned `0600` file, bounded to 1 MiB.
Reads use held, no-follow directory/file descriptors and validate identity and
metadata before/after reading. Linux leaf access stays beneath `/proc/self/fd`.
The same held directory anchors private command journals. No agent write API or
agent scratch fallback exists.

Policy has exactly `schema_version: 1`, arrays
`owner_chats`, `control_chats`, `read_chats`, `vehicles`, nullable
`default_vehicle`, nullable integer `low_battery_percent` (1–100). Vehicles have
exactly `vin`, normalized nonempty `alias`, and boolean `shared`. VINs/aliases are
unique. Default alias must be listed. Empty roles/vehicles authorize nobody.
Host owner supplies actual existing IDs only during separately authorized setup.

Live inventory must contain the selected listed VIN exactly once. Non-shared
entries additionally require native `last_state.access_type === 'OWNER'` and
`is_active === true`. The broker normalizes the nested access field for its
client inventory projection. Explicit protected `shared: true` still requires
presence in live inventory. Inventory output contains admitted vehicles only.

## Existing OneCLI gateway

Use the existing normal `main` OneCLI route through `resolveOneCliRoute` and the
bounded no-redirect JSON helpers. The gateway injects the existing vaulted Tessie
key for exact `api.tessie.com`; no key value enters host request bodies, agent
containers, output, logs or chat. Existing OneCLI configuration is sufficient.
Missing gateway configuration refuses the request without direct HTTP fallback. The host's protected VIN/chat
checks apply before its provider requests.

## Wire schema

Task-file envelope uses native `requestId` (32 lowercase hex), native session name,
`type: 'tessie_request'`, and `payload`. `payload: 'policy'` returns only own-role
presentation and listed vehicles, excluding credential ID/other role JIDs.
Otherwise payload has exactly:

- `schema_version: 1`, `method: 'GET' | 'POST'`, fixed `path`;
- string-valued `query`, nullable JSON-object `body`;
- nullable `confirmation`, exact `<alias>:<action>` for controls.

`operations.ts` owns fixed routes, supported commands and parameter bounds. No
wake, uncached read, arbitrary query, header, URL or unsupported control exists.
Reads preserve provider cache defaults. Controls allow explicit Tessie attempts
1–3 and no host retry. Non-owner location/drivers requests are refused; every
other non-owner response uses a positive documented field projection. Unknown
fields are dropped on the host before IPC. Owner output still follows the
consumer's explicit location-disclosure rules.

Success has `schema_version: 1`, integer HTTP `status`, projected JSON `data`, and
for control receipts `dispatched_at_epoch` from the actual host dispatch boundary.
Errors have `schema_version: 1` and fixed safe `error: {code, message}`. Provider
error bodies, native credential metadata, paths, proxy configuration and headers
never enter this projection. Native atomic `writeScriptResult` publishes replies.

## Durability and confirmation

A fixed per-broker queue serializes authorizations and requests; pending work is
bounded to 32. Before a control POST, exclusively create and fsync a private
`command-<digest>.json` intent, then fsync its directory. Bind request ID to source
JID and complete normalized request. No POST occurs before intent durability.

The broker-owned `DATA_DIR/tessie-authority/command-<digest>.json` journals use
the Stateful Artifacts **Gate Store carve-out**: an existing uncertain intent
refuses a POST, while absence permits admitting a new intent. A reader meeting
an unsupported journal version refuses with the safe actionable `journal_version`
diagnostic: update the host reader for the retained schema, preserve the intent,
inspect cached vehicle state, and never delete the journal or replay the prior
command to recover. Only version 1 is currently accepted. If the owner later
accepts an older version, it must migrate and rewrite that older record on read.

The host owns journal schema 1. An intent has exactly integer `schema_version: 1`,
`binding` (lowercase SHA-256 of the source JID and complete normalized request),
and `outcome: 'uncertain'`. A completed record has exactly `schema_version: 1`,
the same `binding`, `outcome: 'complete'`, and `reply`. Its reply has exactly
`schema_version: 1`, integer HTTP `status` (100–599), `data`, and finite
nonnegative `dispatched_at_epoch`. A 2xx receipt's data has exactly boolean
`result`; every non-2xx receipt's data is null. The filename hashes source JID
and native request ID, independently of the request binding. Only version 1
completed records with all fields validated can be returned. Unsupported,
partial, malformed, mismatched or uncertain state is preserved and refused;
there is no implicit migration, guessed default, rewrite or fresh POST.

Completed identical native retries return their original receipt without a second
POST. Different bindings, torn journals, timeouts and unknown outcomes never
replay automatically, including after restart. Keep private intent history until
the owner resolves uncertainty; no implicit pruning or manual erasure recovery.
Tessie `result: true` means acceptance only. The consumer requires matching state
with timestamp at or after host dispatch. Failed/stale readback preserves that
receipt as unconfirmed. No automatic resend follows uncertainty.

## Delivery and acceptance

Publish/install the matching `nanoclaw-host` authority before enabling consumers.
Merge this producer before publishing a reader. Keep broker disabled until native
VIN/chat policy is provisioned by the host owner. Deploy producer/consumer
through canonical host workflow, then separately verify installed bytes, real
role denial, cached sleeping-car reads, and one explicitly authorized reversible
control on a selected awake vehicle with post-dispatch readback. Verify actual
`tessie-watch` cadence registration before calling the recurring feature shipped.
This source change exercises synthetic fixtures only; Tesla issue #1 remains open
for runtime acceptance. Tesla2–7 follow the foundation; Tesla8 remains held.

Regression owners: `src/host-plugins/tessie/*.test.ts`,
`src/container-runner.security.test.ts`, consumer `tests/test_native.py` and
accepted/confirmation regressions. No real vehicle, account, calendar or live
OneCLI policy operation is part of those fixtures.
