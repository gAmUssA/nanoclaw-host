# Tessie running-late: proposal and wire1, private database4

Contract: tessie-running-late-host-v1

Producer: `jbaruch/nanoclaw` `src/host-plugins/tessie-late/`. Consumer:
`jbaruch/nanoclaw-tesla` `tessie-running-late`. This is the bounded Tesla7
workflow, separate from vehicle controls, conference follow-ups, Tasks and refund
proposals. Import/registration performs no provider operation or grant change.

## Native identity and transport

Require the actual registered main source and its current protected Tessie owner
role. IPC directory identity, the current in-process registered-group map and the
host callback claim own these facts. Caller JIDs, role labels, environment hints,
JSON approvals and imported receipts grant nothing. Recheck registration and
protected policy after awaited I/O and immediately before effect dispatch.

Calendar reads use the existing normal-main OneCLI route and fixed primary
Calendar GETs, with complete bounded pagination. Vehicle discovery/status/state
compose the existing Tessie broker, including fresh dynamic inventory and cached
reads. No vehicle control, wake, uncached read, location test or alternate API
credential path is added.

Recipient email uses only the host-provisioned OneCLI identity
`nanoclaw-host-tessie-running-late`. Fresh native agent/grant readback must prove
that exact unique BYO identity, no channels or generic secrets, one Gmail
connection, custom tools `get_profile`, `search_messages`, `get_message`,
`send_email`, empty ASK and `otherReads/otherWrites: never`. Resolve only that
identity's route. Do not create/attach/grant through a consumer or reuse the
conference identity. Missing setup holds email delegation. The gateway injects
the credential; no key/proxy/userinfo is printed or returned.

The native Gmail profile binds the sender account displayed for approval.
Subsequent account changes refuse dispatch. Use the fixed Gmail profile,
Sent-message query/raw read and `messages/send` endpoints only. The finite shared
MIME/Sent transport is not an approval mechanism.

## Event and navigation evidence

Read the actual primary Calendar event. Bind its native ID, ETag, title, timed
start/end, location and contact fields into the private source hash. Instance IDs
keep recurring meetings distinct. Refuse cancelled, declined, malformed or
ambiguous source. Re-read native event state at approval and delivery.

Only current drive-engine schema1 `meeting_outbound` blocks qualify. Their
`dengine_leg` must identify the actual meeting, with consistent anchor and
location. Return, airport, unknown and unassociated blocks cannot trigger reads.
A fresh D-gear matching route may outlast the planned drive end, until the meeting
ends. No match or multiple matching vehicles means hold, never default selection.

Require native navigation timestamp no older than five minutes, nonfuture except
30 seconds of clock tolerance, recognized D gear, finite bounded minutes to
arrival and conservative destination identity. Case/spacing normalization grants
no substring, fuzzy place or semantic match. Navigation minutes already provide
the arrival estimate; do not add traffic delay twice.

A non-declined other attendee is waiting-contact evidence. A non-attendee external
contact requires actual event-bound owner approval after the model identifies
that contact; the selected mailbox must occur literally in current native event
contact evidence. Literal membership is not a semantic classifier. Never infer
approval or a recipient from arbitrary description prose alone.

## Pre-event approval

A proposal is native host data, not a caller draft. Prepare from actual event and
sender-profile reads. Store schema1, opaque proposal ID, event ID/source hash,
title, start/expiry, recipient, sender account, fixed template version and creation
time in the private callback database. Bound proposal expiry to the event end and
the supported 24-hour window. A changed event needs a fresh proposal; an old
proposal cannot silently adopt changes.

The visible card names the event, start, expiry, recipient and sending account,
shows the fixed lateness template and states the total-two budget below. Native
`tessie.late.v1` callbacks carry only schema1, opaque proposal ID and the
approve/cancel decision. Require the exact private card text/buttons, actual
Telegram message binding, real owner sender and live host claim. Later polling
requires that claim to be resolved/applied, unexpired, source-bound and unchanged.
A generic `owner.answer.v1` reply is not this delegation.

Cancellation removes delegation without a provider call. It cannot erase prior
attempts or restore delivery slots. No automatic approval cards are sent while
driving; preparation is a separately requested pre-event owner workflow.

## Shared delivery budget and confirmation

At most **two automatic lateness messages TOTAL per native event**, across owner
Telegram notices and recipient email. Reserve each slot transactionally in the
private host ledger before dispatch. Initial delay must exceed ten minutes. An
optional worsening follow-up must exceed twenty minutes and the first delay.

The initial owner notice contains the delay and ready-to-send text. Without
recipient delegation, one worsening owner follow-up may use slot2. With recipient
delegation, the owner notice plus one recipient email consumes both slots; no
later follow-up is allowed. Separate proposals, changed recipients, cancellation,
restart and reapproval never reset the event budget. Pre-event owner-requested
approval cards are not automatic lateness notifications.

Each intent starts uncertain before external I/O. Failed, interrupted and
unconfirmed attempts consume their slots. Never automatically replay them.
Owner delivery requires the real native message ACK. Gmail requires the exact
account/recipient/subject/body/Message-ID in actual Sent evidence, with no second
POST on uncertainty. Provider IDs and apparent receipts never confer approval.
Only the explicit scoped readback path may confirm a retained original attempt.

Account/Sent preflight is awaited I/O. Re-observe the native event, current drive
block and fresh matching route before reserving recipient dispatch. Recheck
current owner scope, policy, approval and exact retained intent immediately
before POST. No third-party message contains coordinates, street addresses or a
vehicle alias; it uses the visible approved event-title/ETA template.

## Storage, wire and deployment

Host callback database4 adds Tessie-owned schema1 proposal, event-budget and
attempt records. Migration3 belongs to Tasks; the sole private database installer
owns ordered migration3 to4 without changing Tasks authority. These tables use
the Stateful Artifacts **gate-store exception**: unknown schemas or malformed
records hold with a preservation/update diagnostic. Never treat them as empty,
downgrade, delete or reinitialize to recover permission or delivery slots.
Backups and recovery preserve consumed budgets/attempts monotonically and do not
restore active owner approval from shared or imported data.

`tessie_late_request` is a separate schema1 native operation. Its finite actions
are prepare, poll, status and cancel, with only the action's declared event,
recipient or opaque proposal IDs. No caller body, route, credential, role,
callback claim or receipt import exists. Fixed response envelopes expose only
owner-visible proposal cards or bounded status counts. Provider bodies, proxy
metadata, raw mail and private callbacks remain on the host.

The scheduled consumer polls every few minutes. The host owns deterministic
notification dispatch and exact delivery slots; the precheck does not ask the
model to send another lateness message. Failures remain visible in task/host
status and never route through an alternate API or credential.

Publish/install this authority and the matching Google-access authority, merge
the actual producer, then publish the consumer. Require its declared regular
callback adapter before registration. The host owner separately provisions and
verifies grants and genuine native context, deploys canonically and registers the
cadence. Synthetic fixtures, source merges and publication do not establish
installed or live delivery. Keep Tesla7 open until that acceptance is proven;
Tesla1's separate physical-control acceptance is unchanged.
