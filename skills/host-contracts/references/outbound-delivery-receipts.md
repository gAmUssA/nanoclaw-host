# Protected Outbound Delivery Receipt Contract

## Owner and Boundary

Runtime `jbaruch/nanoclaw` owns receipt schema 1. Its private callback database is `DATA_DIR/owner-callbacks/callbacks.db`, outside agent authority. `src/state-migrations/callback-003-outbound-deliveries.ts` owns the `outbound_delivery_tracking` and `outbound_delivery_attempts` tables. The callback-store owner upgrades private database versions 1 and 2 to 3 while preserving existing callback and conference records; consumers never open, migrate or rewrite this database.

The producer covers native confirmed-operation `send_message` IPC requests from the uniquely registered main group to its own Telegram chat, on the direct bot path. The generated operation request ID identifies transport continuity, not semantic intent or owner approval. A request ID reused with changed content refuses; an existing uncertain request never sends again. A fresh request ID does not authorize retry of an earlier uncertain send.

Before any covered send or callback-card arming, the host commits an uncertain intent containing the exact original MCP text hash and normalized effect binding. A pre-effect storage refusal prevents the send. Native delivery becomes confirmed only after every expected chunk returns a distinct valid native message ID through the normal rendering path. A missing ID, partial send, thrown transport failure, degraded fallback or unavailable ACK persistence leaves the durable intent uncertain. The producer preserves genuine tool success when native acknowledgment succeeded but receipt persistence failed; it reports the protected-store fault in host diagnostics. No receipt failure initiates another send.

Confirmed delivery does not depend on mutable public message history or later mirror, pin or callback failures. It proves no message read, owner identity, owner answer, effect approval, pin, archive, reply or calendar operation. A historical receipt grants no new send authority.

## Fixed Read Route

Main invokes `mcp__nanoclaw__get_outbound_delivery_receipts`. The immutable client registers `get_outbound_delivery_receipts` only in main and sends IPC `outbound_receipts` with `outboundReceiptQuery`. The host derives source and destination from verified IPC identity plus fresh unique main registration, checks that registration again before returning the projection, and accepts no caller-selected account, chat, role, path, body or acknowledgment.

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

## Consumer Claims and Positive Recovery

Persist the exact planned MCP text/hash and stable source/item/event set before one send attempt. Preserve immutable historical claims and acknowledgment facts. Accept a report acknowledgment only from one uniquely matching confirmed receipt with the expected original payload hash and time binding after reading every relevant page. Bind its native message IDs and host ACK time to the persisted plan; never rebuild the plan from mutable text history.

Missing, uncertain, malformed, degraded, unsupported, ambiguous or unavailable records keep the original sending claim held. Do not retry or clear it from absence, query exhaustion or tracking-start metadata. Distinct receipts with identical payloads remain ambiguous until actual source/plan binding resolves them. Positive typed Gmail-card references may seed today's report ledger; old/untracked cards never imply an unreported thread. A new full morning report can establish positive tracked history without rewriting unknown prior history.

This gate store retains uncertain and confirmed intents without automatic pruning. Unknown private record versions or broken constraints refuse rather than treating an existing send as new. Only the host owner restores or evolves protected state; a consumer cannot discard an uncertain intent to reclaim send permission.

## Rollout and Acceptance

Publish/install this host authority before enabling its consumer. Merge/build the matching core producer and verify the installed actual agent's main-only MCP registration and current host's protected store. Keep consumer readiness false and cadence disabled until native fixture acceptance demonstrates complete native IDs, same-payload readback, uncertain/no-replay behavior and private source projection. Synthetic provider fixtures are transport evidence, not a real owner delivery, approval or active cadence.

Regression coverage lives in core `src/outbound-delivery-store.test.ts`, `src/outbound-delivery-query.test.ts`, `src/ipc-handlers/messages.test.ts`, `src/ipc-handlers/outbound-receipts.test.ts`, `src/channels/telegram.test.ts`, `src/channels/telegram-delivery-ack.test.ts`, `src/callback-store.test.ts` and `container/agent-runner/src/outbound-receipt-tools.test.ts`. Existing send-message success text and read-only receipt cleanup remain compatible.
