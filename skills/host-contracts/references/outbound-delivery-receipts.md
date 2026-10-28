# Protected Outbound Delivery Receipt Contract

## Owner and Boundary

Runtime `jbaruch/nanoclaw` is the sole owner of the public receipt schema1 and private receipt record2 and its `outbound_delivery_tracking` / `outbound_delivery_attempts` tables in private `DATA_DIR/owner-callbacks/callbacks.db`. Consumers never open, migrate or rewrite that database.

The receipt producer remains held in unmerged core PR1175. Its rebased owner migration is `src/state-migrations/callback-005-outbound-deliveries.ts`, creating private database5 after the canonical callback1 → conference2 → Tasks3 (`callback-003-promise-tasks.ts`) → Tessie4 (`callback-004-tessie-running-late.ts`) chain. Construction migration006 (`callback-006-construction-journal.ts`) then extends the shared private database to6 under `construction-journal-authority.md`. `src/callback-store.ts` validates the complete retained predecessor schema and constraints before upgrading canonical versions1–5. Outbound005 adds its tables and tracking-start metadata transactionally without changing existing cards, Task gates or Tessie budgets; Construction006 preserves every existing domain. An existing historical outbound-shaped database3 is not Tasks3: preserve it unchanged and require the owner's verified backup/migration path. Never reset unknown state or infer absent delivery gates. Publishing this contract supplies no installed producer or native acceptance; outbound readiness remains false until both are proved.

The producer covers native confirmed-operation `send_message` IPC requests from the uniquely registered main group to its own Telegram chat, on the direct bot path. The generated operation request ID identifies transport continuity, not semantic intent or owner approval. A request ID reused with changed content refuses; an existing uncertain request never sends again. A fresh request ID does not authorize retry of an earlier uncertain send.

The host admits a new covered intent, callback-card arming and receipt/card attachment in one synchronous transaction, with no channel I/O before commit. A failure proven inside that pre-effect transaction rolls back its new undispatched candidate; existing replay gates remain intact. Before the covered channel send, the committed uncertain intent contains the exact original MCP text hash and normalized effect binding. A pre-effect storage refusal prevents the send. Native delivery becomes confirmed only after every expected chunk returns a distinct valid native message ID through the normal rendering path. A missing ID, partial send, thrown transport failure, degraded fallback or unavailable ACK persistence leaves the durable intent uncertain. The producer preserves genuine tool success when native acknowledgment succeeded but receipt persistence failed; it reports the protected-store fault in host diagnostics. No receipt failure initiates another send.

Confirmed delivery does not depend on mutable public message history or later mirror, pin or callback failures. It proves no message read, owner identity, owner answer, effect approval, pin, archive, reply or calendar operation. A historical receipt grants no new send authority.

## Host Configuration Activation Gate

`OUTBOUND_RECEIPTS_ENABLED` is a normal host-owned configuration flag, default false; only the literal `true` enables it. The runtime owner enables it only after actual matching image, native fixture and reader acceptance are recorded in the existing private rollout journal. Publication, installed contract files, source tests, model output and caller fields do not enable it. This adds no acceptance-record schema, proof store or manual `ready:true` artifact. Consumer/cadence readiness remains separately false until its own genuine acceptance.

The host overwrites `ContainerInput.outboundReceiptsEnabled` from its own configuration and main status. Agent-runner derives the internal MCP registration hint `NANOCLAW_OUTBOUND_RECEIPTS_ENABLED` from that trusted input; only main with the enabled hint registers the receipt tool. This hint supplies no authority: the host independently checks its current configuration for every receipt query and covered send. An agent-supplied environment variable, query field, request flag or copied context cannot enable host receipt production or readback. Disabling the host flag refuses requests even from an older agent whose tool remains registered.

A disabled cold installation may keep existing legacy direct-send behavior only after the owner verifies canonical receipt tables/constraints, valid initialization metadata and zero receipt attempts globally. The mandatory empty `outbound_delivery_tracking` singleton is initialization metadata, not evidence that tracking was activated. Legacy sends produce no receipt and establish no positive coverage. If ANY retained confirmed or uncertain outbound attempt exists, disabled covered sends HOLD globally before channel dispatch, including after a main source/chat registration change; they never fall back to a legacy send. Missing, malformed or unavailable storage also HOLDs instead of implying a cold store. Existing receipt gates and native ACK facts remain authoritative and unchanged while disabled; no flag transition erases uncertainty or permits replay. Existing protected backup/restore remains available independently of public receipt exposure.

## Protected Backup and Cross-Pipeline Rollout

Outbound-only deployment remains a supported intermediate: private database5 uses protected envelope4, with exactly `schema_version`, `kind`, `entries`, `promise_tasks`, `tessie_late` and `outbound`. Publishing the Construction extension does not require PR1175 alone to install database6 or emit envelope5. Install the matching Construction-capable reader before the later database6/envelope5 writer transition; no unknown forward schema may be interpreted as empty history.

Core owns protected backup envelope5 at `host/conference-followup-ledger.json`. Its exact top-level fields are `schema_version`, `kind`, `entries`, `promise_tasks`, `tessie_late`, `outbound` and `construction`; `kind` remains `conference-followup-delivery-ledger`. The outbound section is null when projecting canonical private databases1–4. The corresponding database5/envelope4 and database6/envelope5 owners project the same outbound section2: `tracking_started_at` and complete bounded private record2 receipts through `src/outbound-delivery-backup.ts`. Active callback choices are never exported or restored.

The owner ledger reader accepts canonical envelope1 (conference), envelope2 (plus Tasks), envelope3 (plus Tessie), envelope4 (plus outbound) and envelope5 (plus Construction). The historical unactivated outbound-only envelope2 is not the canonical Tasks envelope2 and is refused. Missing outbound or Construction history in an older canonical backup remains unknown; restoring it never erases current rows or append gates. One restore transaction preserves every domain's gates. Outbound confirmation can grow only monotonically from uncertain to matching complete ACK facts; immutable identity/hash, settled native IDs/time and typed Gmail references cannot be retconned or lost. Tracking start can move only toward the earliest retained observation. No restored receipt or opaque callback-card identifier recreates an approval or permits replay.

Publish/install this authority before enabling receipt consumers. Deploy the matching owner reader before its writer: canonical envelopes1–4 for the outbound-only database5 stage, and canonical envelopes1–5 before the Construction database6 stage emits envelope5; the matching core release supplies both implementations. Keep old backups usable throughout the transition. The public MCP query/response and receipt records stay schema1, so consumers do not migrate private headers or receive a new send capability. During mixed host/agent installation, unavailable or unsupported lookup preserves every pending consumer claim and leaves its cadence disabled. Host-operator-controlled installation, backup/restore tools and installed-native acceptance remain separate from source publication. Do not apply a database4 recovery tool to a new database5/6 journal.

## Exact Protected Record and Backup Mapping

The non-null `outbound` backup object has exactly `schema_version:2`, `tracking_started_at` and `receipts`. `tracking_started_at` is canonical UTC ISO milliseconds. `receipts` is an array of zero to10,000 complete records with distinct `receipt_id` values; truncation and eviction are forbidden. Every record has every field below. Nulls are explicit and no missing-field defaults are accepted.

| Required field | Persisted normalized type and bound |
| --- | --- |
| `schema_version` | Integer2 for the private record; public receipt stays1. |
| `receipt_id` | Lowercase hex64 SHA-256 of exact UTF-8 JavaScript `JSON.stringify([source_group,chat_jid,transport_key])`; recomputed on every decode. |
| `source_group` | ASCII string matching `^[A-Za-z0-9][A-Za-z0-9_-]{0,127}$`. |
| `chat_jid` | Exact native destination string matching `^tg:-?[0-9]{1,16}$` (maximum20 ASCII characters including `tg:` and optional minus); not a caller-selectable query scope. The [native Bot API limit](https://core.telegram.org/bots/api#chat) is52 significant bits, which fits16 decimal digits. The owner applies this finite width in database, projection and restore validation. |
| `transport_key` | ASCII base64url-style string matching `^[A-Za-z0-9_-]{1,128}$`. |
| `binding_sha256` | Lowercase hex64 of the host-normalized complete effect binding. |
| `payload_sha256` | Lowercase hex64 of the original MCP text's UTF-8 bytes. |
| `visible_text_sha256` | Lowercase hex64 of the host's logical post-strip/prefix text. |
| `attempted_at` | Canonical UTC ISO milliseconds, normalized from valid four-digit-year UTC input with optional1–3 fractional digits; impossible normalized dates refuse. |
| `status` | Literal `uncertain` or `confirmed`. |
| `host_sent_at` | Null iff uncertain; otherwise canonical UTC ISO milliseconds at or after `attempted_at`. |
| `native_message_ids` | Array, maximum128, of distinct strings matching `^[1-9][0-9]{0,23}$`; empty iff uncertain, nonempty iff confirmed. Order preserves native chunk order. |
| `gmail_refs_state` | Literal `unavailable`, `none` or `bound`; uncertain requires unavailable. |
| `gmail_source_refs` | Null iff unavailable; empty array iff none; nonempty array iff bound. Maximum8 exact objects `{kind:"thread"|"message",id:<lowercase hex1..64>}`, distinct by kind/ID; bound requires a non-null `callback_card_id`. |
| `callback_card_id` | Null or exact ASCII base64url22 historical private card ID. It never restores a capability. |
| `origin` | Null or the complete supported native-origin object defined below. |
| `success_observation` | Null or the complete native-return observation defined below; never exact-payload ACK. |
| `tool_stdout` | Null, `Message sent.` or `Message sent and pinned.`; uncertain requires null. Native ACK alone does not manufacture this later tool result or pin success. |

Private record2 additionally requires `origin` and `success_observation`, each explicitly null or a complete supported schema1 object. `origin` is `{schema_version:1,kind:"owner_input",chat_jid,message_id,request_sha256}` or `{schema_version:1,kind:"cadence",task_id,scheduled_for}`. Owner chat equals the receipt destination, native message IDs are positive decimal strings of1–24 digits, request SHA is lowercase64hex, task ID is1–512 valid Unicode code points, excluding ASCII controls, and nonblank, preserved verbatim (including actual `cadence-registry::<group>::<skill>` IDs), and scheduled time is exact canonical UTC milliseconds. `success_observation` has exactly `{schema_version:1,native_message_id,completed_at}`, with the same native ID grammar and canonical UTC time at/after attempt. On a confirmed record its native ID must equal the final confirmed chunk ID and its time cannot predate host ACK time. It is a complete native-return observation, not exact-payload delivery authority. These two private fields are omitted from every public receipt.

The section reader explicitly accepts the previously published outbound section1/record1 shape, validates all legacy fields, then converts to section2/record2 with both new fields null. Legacy conversion never infers origin or completion from text, an opaque hash, a tool result or missing evidence. Section2 requires record2 and both fields. Historical private DB5 record1 tables remain held byte-for-byte with supported host owner upgrade/verified backup guidance; no guessed identity migration, reset or claim clearing is supported. Canonical DB4 upgrades create record2 directly. Shared database5/6 and top-level envelopes4/5 are unchanged.

Private `outbound_delivery_tracking` has exactly `singleton` (integer1 primary key), `schema_version` (integer1) and `started_at` (canonical UTC milliseconds). The owner creates exactly one row using its initialization clock. Its backup mapping is `started_at`→`tracking_started_at`. `outbound_delivery_attempts` has exactly the normalized fields above except that `native_message_ids` is stored as JSON text `native_message_ids_json`, and `gmail_source_refs` as null or JSON text `gmail_refs_json`; `origin` and `success_observation` use nullable JSON text columns `origin_json` and `success_observation_json` before `schema_version`. Each JSON text is bounded to65,536 UTF-16 units before parsing; parsed arrays still obey the smaller bounds above. Other field names/types are unchanged. SQL has primary key `receipt_id`, unique `(source_group,chat_jid,transport_key)` and schema/status/ref-state checks; the canonical owner additionally validates every complete row and recomputed identity. No SQL column has an implicit receipt-field default. The producer creates uncertain rows with null ACK/card/tool/reference data and an empty native-ID array.

Projection parses and validates the four JSON columns into their normalized native-ID array, Gmail references, origin and completion fields, never includes raw text/choices, and emits all retained receipts. Restore validates the exact normalized object keys and invariants, serializes all four values back to their fixed SQL columns, and monotonically merges within the whole multi-domain transaction. It preserves the earliest tracking start; no opaque card ID recreates callback rows. Unknown fields, versions, duplicate identities or conflicting native facts refuse the entire restore before commit.

`callback_card_id` refines only from null to one actual historical card ID while the null side is uncertain. A confirmed null-card receipt describes a plain message and cannot acquire a card through restore. A retained non-null card survives an older uncertain null; two different non-null card IDs refuse the complete transaction. `tool_stdout` refines from null to one of its two allowed literal values only in a confirmed receipt, retains a known value when incoming data is null, and refuses any two different non-null values. In particular, `Message sent.` cannot upgrade to `Message sent and pinned.` through restore. These fields never supply approval or additional delivery permission.

## Finite Shared Capacity Admission

The permanent gate store has no automatic archival or pruning. `src/protected-delivery-capacity.ts` owns transaction-local admission against the exact canonical full-ledger projection on the same SQLite handle, including uncommitted candidates. Before a new outbound intent, conference send intent, Task creation intent, existing-Task observation, Tessie delivery slot or Construction append intent can commit, the owner checks every domain's complete bounds, the10,000 outbound-row limit and the32 MiB UTF-8 JSON-plus-newline archive limit. It reserves conservative remaining growth for native IDs, confirmation times, callback-card IDs, typed Gmail references, fixed tool results and bounded private completion metadata on all unsettled records, including the bounded Construction append ACK and confirmation timestamp. An uncertain null `callback_card_id` reserves24 JSON bytes for its quoted22-character ID (20 bytes growth from null); attachment consumes that reservation before native send. Conference confirmation writes cap time strings at27 UTF-16 units; older retained timestamps remain intact. Native acknowledgments of already-admitted intents do not create a new intent or discard evidence to fit.

An exhausted bound rolls back only the new undispatched candidate transaction; prior acknowledged/uncertain gates, slots and original claims remain unchanged. Whole offline restore performs the same check on the merged state before committing any domain. Known storage/schema faults HOLD instead of becoming empty capacity. The fixed capacity diagnostic contains `schema_version:1`, `constraint` (`outbound_rows`, `archive_bytes`, `future_ack_bytes` or `gate_state_unavailable`), `used_bytes`, `reserved_bytes`, `maximum_bytes` and, for the row bound, `observed_rows`/`maximum_rows`. Usage may be null when unavailable; a bounded row query reports at least the first disallowed count, not an exact count beyond its sentinel. No body, account credential or approval is included.

At capacity, new dispatch/import stays held; existing exact replay checks and retained readbacks remain authoritative. Expansion requires a supported reviewed host-owner upgrade that keeps every gate and the complete backup/restore contract usable. This release implements bounded admission and reserved growth, not an archive store, eviction, manual raw-database repair, claim clearing or permission to retry uncertain sends.

## Fixed Read Route

Main invokes `mcp__nanoclaw__get_outbound_delivery_receipts`. The immutable client registers `get_outbound_delivery_receipts` only in main while its trusted host-derived readiness hint is enabled, and sends IPC `outbound_receipts` with `outboundReceiptQuery`. The host rechecks `OUTBOUND_RECEIPTS_ENABLED`, derives source and destination from verified IPC identity plus fresh unique main registration, checks that registration again before returning the projection, and accepts no caller-selected account, chat, role, path, body or acknowledgment.

Supply exactly these query fields:

| Field | Contract |
| --- | --- |
| `schema_version` | Integer 1 |
| `since` | UTC ISO timestamp with `Z`, optional 1–3 fractional digits |
| `before` | Same timestamp form; at or after `since`, no later than current host time |
| `payload_sha256` | Null or lowercase 64-hex SHA-256 of the exact original MCP text's UTF-8 bytes |
| `cursor` | Null for the first page, otherwise the returned opaque continuation |
| `limit` | Integer 1–50 |

Both time endpoints are **inclusive**, including the same millisecond. Maximum interval is 31 days. An intent matches if its `attempted_at` **or** its confirmed `host_sent_at` lies in the interval. This retains a send attempted before midnight and acknowledged after midnight. Results sort by immutable `attempted_at`, then `receipt_id`.

A cursor binds the original source scope, time interval, payload selector and page size. Follow `next_cursor` until null without changing selectors. This is a bounded **live status view**, not an atomic snapshot: a row may become confirmed or acquire a validated source binding between pages. Restart with a null cursor for a fresh recovery read. Exhaustion does not prove delivery absence or complete coverage. Exact-payload recovery validates all same-hash scoped records before time filtering, up to 1,000 records; excess or malformed records refuse without reducing ambiguity. Responses are bounded to 256 KiB.

The existing confirmed-operation result channel returns one JSON object as tool text. Storage/query failure returns an actionable tool error without usable receipt data. Unsupported or malformed queries/results and transport timeouts preserve all consumer claims. They never become an empty successful result.

## Response Schema 1

The exact top-level fields are:

| Field | Contract |
| --- | --- |
| `schema_version` | Integer 1 |
| `scope` | Literal `own_main_direct_mcp` |
| `observed_at` | Current host UTC observation, canonical milliseconds |
| `tracking_started_at` | Protected host timestamp when this producer's tracking metadata was first installed |
| `coverage` | Object described below |
| `receipts` | Array of schema-1 receipt objects |
| `next_cursor` | Null or opaque continuation string |

`coverage` contains exactly `complete:false`, `view:"bounded_live_status"`, `legacy`, `excluded_paths` and `absence:"unknown_never_retry"`. `legacy` is `unknown_before_tracking` when `since` precedes `tracking_started_at`, otherwise `tracked_scope_only`. `excluded_paths` lists `pool_bots`, `sdk_final_output`, `legacy_unconfirmed_ipc`, `cross_chat` and `non_telegram`. Tracking start does not backfill old cards or prove that every message after that point used the covered route. Untracked/legacy messages remain unknown.

Each receipt contains exactly:

| Field | Contract |
| --- | --- |
| `schema_version` | Integer 1 |
| `receipt_id` | Lowercase 64-hex host receipt identity |
| `payload_sha256` | SHA-256 of exact original MCP text UTF-8 bytes, before internal-tag stripping or maintenance prefix |
| `visible_text_sha256` | SHA-256 of logical channel text after host stripping/prefixing, before Telegram rendering; not a hash of Telegram wire bytes |
| `attempted_at` | Protected pre-effect host UTC timestamp, canonical milliseconds |
| `status` | `uncertain` or `confirmed` |
| `host_sent_at` | Null for uncertain; canonical UTC milliseconds observed after the complete native ACK for confirmed |
| `native_message_ids` | Empty for uncertain; ordered distinct positive native decimal IDs for every acknowledged chunk when confirmed |
| `gmail_refs_state` | `unavailable`, `none` or `bound` |
| `gmail_source_refs` | Null for unavailable; empty array for none; typed references for bound |

`host_sent_at` is the host's post-ACK time, not a Telegram server timestamp. It remains unchanged on readback. Confirmed ACKs retain all native chunk IDs rather than treating a last-chunk ID as whole-payload evidence. Hashes bind the submitted logical text; normal Telegram formatting still applies.

`gmail_source_refs` entries have exactly `{kind:"thread"|"message",id:<lowercase hex native ID>}`. The host copies them only from the protected, actually native-message-bound callback card after validating source group, chat, native message ID, original text and canonical `gmail.archive.v1` link/payload binding. Subject, visible URL or generic history text cannot supply a reference. `none` means this tracked message has no bound Gmail reference; `unavailable` means reference binding is not established and supplies no negative conclusion. A message ID becomes a thread association only through actual fetched thread membership; never infer decimal/hex conversion or thread identity from display text.

## Native Input Origin and Successful Return

Only the actual channel sender checked against the configured native owner at ingest may establish an owner-input witness. The host stores the witnessed request digest before writable message-table insertion and requires the same rendered digest at handoff. Owner origin is the canonical native chat/message/digest tuple. Scheduler origin is the host task/occurrence tuple captured at spawn and checked against the actual running occurrence and group. Names, `is_from_me`, bootstrap UUIDs, later sender rows and caller-supplied taskOccurrence are not owner evidence.

A separate outbound HMAC domain uses the existing trusted handoff lifecycle without depending on Herdr activation. The actual actor receives only its own private readonly carrier. The opaque handoff ID grants nothing alone. The MAC binds the complete serialized IPC payload; the host checks private actor/source/chat/session, current main registration, committed handoff and expiry. Uncommitted, discarded, expired, forged or sibling proofs refuse. Exiting invalidates the actor, and a host restart cannot adopt an old carrier as proof. A different container/bootstrap/request ID for the same native input or scheduled occurrence keeps the same semantic origin.

The native channel separately observes that every chunk completed with a distinct valid native ID, including degraded rendering, while withholding exact-payload ACK for degraded/transformed text. Only that complete in-process observation, the matching returned final ID and the successful host tool-result path can record the private successful return. Partial/no-ID/throw/pin-failure, late callbacks and unavailable persistence grant no such fact. The public degraded receipt remains uncertain with null ACK time and no native IDs. Original consumer claims remain held and cannot use this distinction for positive recovery or automatic retry.

Every retained matching uncertain effect must have BOTH a genuine successful-return observation and a different verified semantic origin before a new invocation proceeds. Same, missing or legacy origin, absent completion and any matching later unknown attempt stay `unknown_never_retry`. A fresh request ID does not reopen them. Earlier uncertainty is never deleted or upgraded to exact confirmation. Protected backup merge preserves every origin/completion fact and refuses conflicts or knowledge loss; supported capacity admission reserves its bounded future growth before effect.

## Consumer Claims and Positive Recovery

Persist the exact planned MCP text/hash and stable source/item/event set before one send attempt. Preserve immutable historical claims and acknowledgment facts. Accept a report acknowledgment only from one uniquely matching confirmed receipt with the expected original payload hash and time binding after reading every relevant page. Bind its native message IDs and host ACK time to the persisted plan; never rebuild the plan from mutable text history.

Missing, uncertain, malformed, degraded, unsupported, ambiguous or unavailable records keep the original sending claim held. Do not retry or clear it from absence, query exhaustion or tracking-start metadata. Distinct receipts with identical payloads remain ambiguous until actual source/plan binding resolves them. Positive typed Gmail-card references may seed today's report ledger; old/untracked cards never imply an unreported thread. A new full morning report can establish positive tracked history without rewriting unknown prior history.

This gate store retains uncertain and confirmed intents without automatic pruning. Unknown private record versions or broken constraints refuse rather than treating an existing send as new. Only the host owner restores or evolves protected state; a consumer cannot discard an uncertain intent to reclaim send permission.

## Rollout and Acceptance

Publish/install this host authority before enabling its consumer. Merge/build the matching core producer and verify the installed actual agent's main-only MCP registration and current host's protected store. Keep consumer readiness false and cadence disabled until native fixture acceptance demonstrates complete native IDs, same-payload readback, uncertain/no-replay behavior and private source projection. Synthetic provider fixtures are transport evidence, not a real owner delivery, approval or active cadence.

Regression coverage includes `src/outbound-delivery-readiness.test.ts` and lives in core `src/outbound-delivery-store.test.ts`, `src/outbound-delivery-query.test.ts`, `src/ipc-handlers/messages.test.ts`, `src/ipc-handlers/outbound-receipts.test.ts`, `src/channels/telegram.test.ts`, `src/channels/telegram-delivery-ack.test.ts`, `src/callback-store.test.ts` and `container/agent-runner/src/outbound-receipt-tools.test.ts`. Existing send-message success text and read-only receipt cleanup remain compatible.
