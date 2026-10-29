# Calendar-aware vehicle charging

Contract: tessie-calendar-charge-host-v1

## Owners and admission

Core `src/host-plugins/tessie-charge/` owns the native workflow and reusable
measured-energy API. `nanoclaw-tesla` `tessie-charge` is its thin consumer.
Use only the existing protected `TessieStore` at `DATA_DIR/tessie-authority`.
The private-directory, numeric-owner, no-follow descriptor, single-link,
attachment and mask rules in `tessie-host-authority.md` apply unchanged.
No callback database migration or separate charge-authority directory exists.

The native IPC source must be the unique actual registered Telegram main group.
Revalidate that registry identity and protected policy after native I/O and
before effects. Caller chat IDs, roles, tier labels, booleans, JSON and estimates
supply no authority. Require the declared installed Host and same-provider Tesla
contracts and complete regular consumer transport closure before dispatch.
Missing, unknown or unenrolled authority holds without another HTTP route.

## Reader-first protected enrollment

Install the Core reader accepting exact policies 2 and 3 before any version-3
writer. Policy 2 remains byte-shape compatible and has no enrollment. Policy 3
adds exactly nullable `charge_automation`. Null keeps charging inactive.
Unsupported versions refuse; never downgrade, clear or rewrite them from an agent.

Only the host operator provisions enrollment from an actual owner decision and grounded
native vehicle/calendar/home evidence. No consumer enrollment or policy writer
exists. Preserve the original decision and policy; source release is not approval.
The enrollment has exactly:

- `schema_version: 1`, lowercase hex32 `enrollment_id`, and existing `owner_chat`;
- nullable `default_vehicle` alias/VIN and `daily_limit_percent` (integer 50–100);
- 1–16 exact native `home_saved_locations` labels;
- `event_vehicles` (at most 1000 exact native Calendar IDs to alias/VIN selectors)
  and `person_vehicles` (at most 100 exact owner-approved driver labels to selectors);
- integer `evening_hour` (0–23) in the owner's native current IANA timezone;
- finite `cold_buffer_fraction` and `highway_buffer_fraction` (0–2),
  `reserve_percent` (0–50), and nullable positive `charge_rate_kw` (at most 1000).

Mappings are planning data inside protected enrollment. They never grant access
outside current `non_archived` inventory. Use the current native dynamic fleet;
no hardcoded VIN allowlist, frozen inventory or guessed attendee mapping exists.
An unknown selector or conflicting/unknown source holds the plan.

## Native sources and estimation

Read the maintained drive-engine schema-1 private Calendar metadata, native IDs,
revisions and zoned start/end times. Titles and city strings do not identify
managed drives. Include complete next-day pages and re-read retained original
drive identities before cancellation or restoration; partial history never
proves that a drive disappeared. Preserve ordinary meetings as non-drive data.

Use the existing normal main OneCLI route for fixed primary Calendar GETs,
fixed Google Distance Matrix driving reads and the fixed Tessie broker routes.
Maps uses the existing gateway query placeholder, never a raw key or a new
credential. No consumer may call Tessie or Google directly or change a grant.
An ASK/denied/unavailable route holds without a fallback.

Use dated Tessie battery-health capacity (at most 90 days) and positive recent
weighted driving consumption (at most 30 days), including documented consumption-
since-charge data. Tessie history distances default to miles; convert to km.
Never substitute rated range. Route distance times measured consumption includes
explicit cold/highway allowances and reserve. A charge-speed estimate requires
measured positive power or the owner's configured fallback. Stopped/no-power
charging never becomes an invented charge forecast or an unplugged assertion.

The reusable per-vehicle estimate returns measured capacity/consumption,
measurement time, conditional expected departure charge and minimum energy stops.
It is forecast data, not charger availability, a promise of full charging, or
control authority. Reject nonfinite derived values. Estimates never create a
control intent, enroll a vehicle or send a notice.

## Limit lifecycle and private gates

Read-only observations are bounded-age cached values; never wake the vehicle.
Outside 50–100, hold rather than clamp or create an unrestorable baseline.
Remember the actual native daily baseline even on an observed no-change run.
A later observed limit change becomes a sticky owner-override hold.

In the enrolled evening, raise only for the measured next-day shortfall and never
above 100. If disconnected, attempt one bounded owner notice with the shortfall.
If the round trip exceeds usable charge, report minimum energy charging stops.
Restore the exact retained daily baseline only when the drive is actually done
and the vehicle is back home, or its original native drive block has an exact
returned cancelled status or exact native HTTP 410/global/deleted error. Other
410 responses and inaccessible/404 originals remain unknown; a bounded
day query never proves cancellation merely by omitting an old event.
A home label requires matching current cached coordinates with the bounded-age
native state; do not infer home from a label on an unrelated cached position.

Use fixed host-owned `charge-state-<digest>.json` records in the existing store.
Schema1 binds owner chat, VIN, enrollment epoch, planning day, exact original
native drive identities, retained baseline/managed limits and the native attempt.
The exact record keys are `schema_version`, `kind: calendar_charge`, `owner_chat`,
`vin`, `enrollment_id`, `day`, `phase`, `hold_reason`, `baseline_limit`,
`managed_limit`, `leg_keys`, `event_refs`, `last_end`, and `pending`.
`event_refs` contains exact `{id, leg_key}` pairs, one per retained leg.
Phases are idle, active, held and owner_override. The pending attempt binds the
opaque native request ID, raise/restore, from/to limits and actual timestamps:
`request_id`, `action`, `from_limit`, `to_limit`, `attempted_at`, `accepted_at`.
An accepted timestamp cannot precede its attempt. Active state has a managed limit
above baseline and no pending attempt. Unconfirmed state requires acceptance;
uncertain/refused state cannot fabricate it.
The gate-store exception in Stateful Artifacts applies: unknown/future, malformed,
misbound or uncertain records refuse new controls; absence never clears a retained
gate. Only the host owner may migrate a supported older version on read.
No consumer reset, erase, prune, re-enroll or replay path exists.

Durably retain the charge intent before the existing broker's command intent and
single provider POST. Recheck current calendar, native identity, policy, dynamic
vehicle/alias, private pending gate and actual observed limit before dispatch.
Set Tessie's attempts to 1 and do not retry an unknown control. Acceptance stays
unconfirmed until a state timestamp at/after dispatch shows the requested limit.
Retain accepted receipts through failed confirmation and allow read-only matching
readback recovery without another POST. Refusal/uncertainty keeps the intent.

Tessie supplies no expected-limit/CAS parameter. A Tesla-app mutation after the
last cached observation, including an ABA change, is not observable. Do not claim
absolute protection or atomicity across Calendar, Tessie and external apps.
The host operator must present this limitation for the owner's acceptance; visible changes
hold, and no source or fixture can prove the unobservable race absent.

## Notices and wire

`charge-notice-<digest>.json` is a schema-1 reservation for one owner/local-day
notification. Reserve before send; crash/timeout consumes that day's slot.
Its exact keys are `schema_version`, `kind: calendar_charge_notice`, `owner_chat`,
`day`, `phase`, and `plan_sha256`. Phases reserved/submitted/uncertain never permit replay. Submitted means the
native send call resolved, not a fabricated delivery acknowledgment.

Use one bounded Russian message per enrolled evening. Keep all structured vehicle
outcomes in the native poll result; aggregate long fleets without silent text
truncation or channel chunking. Do not send model duplicates. Confirmed raises,
accepted-but-unconfirmed requests and unplugged shortfalls remain distinct.
No action or pending notification means silence; restoration is silent.

Native `tessie_charge_request` payload schema 1 has exact operations:

- `poll`: no caller fields beyond `schema_version` and `operation`;
- `status`: private bounded counts, no provider/vehicle request;
- `estimate`: additionally exact `vehicle`, finite `distance_km` (0–100000), and
  zoned ISO `departure`, no more than 14 days from the native clock.

Payload route/body/role/enrollment/approval/receipt fields are forbidden.
The transport uses actual native request IDs and source IPC directory; it does
not create main context. Fixed safe errors preserve holds without provider prose.

## Source and native acceptance

Publish this authority first; merge/install the actual producer before releasing
the consumer. Verify installed bytes and dual-policy compatibility before the host operator
provisions policy 3. Keep consumer cadence absent until Root separately registers
and verifies the actual main cadence after enrollment. No source test or publish
operation performs that activation.

The host operator owns native allowed/denied scope checks, current inventory/zone/Calendar and
Maps read acceptance, preserved private masks/identities, and an expressly approved
reversible raise→restore with fresh state, cancellation, unplugged silence/budget
and manual-override checks. Verify actual channel receipt separately. Keep Tesla #2
open until that runtime acceptance and the stated unobservable-race limitation
are resolved with the owner. Deterministic source and fake-provider fixtures
supply no vehicle, delivery, enrollment or genuine-owner runtime proof.
