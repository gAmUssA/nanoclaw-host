---
alwaysApply: false
applyTo: "src/outbound-origin-*, src/turn-hooks*, src/inbound-ingest*, container/shared/outbound-origin*, src/outbound-delivery-*, src/config.ts, src/container-runner*, src/ipc-deps*, container/agent-runner/src/{index,ipc-mcp-stdio}*, src/callback-store*, src/state-migrations/callback-*, src/conference-followup-ledger*, src/protected-delivery-capacity*, src/conference-followup-store*, src/promise-task-store*, src/host-plugins/tessie-late/{store,ledger}*, src/ipc-handlers/outbound-receipts*, src/channels/telegram*, container/agent-runner/src/outbound-receipt-tools*, skills/waiting-on-owner/**, skills/email-surface/** — when producing or reading native outbound delivery receipts, deduplicating reports, changing their private schema/install chain, or projecting, restoring or admitting shared protected delivery gates"
---

# Outbound Delivery Receipts

## Required Read

- Before producing, querying or accepting delivery receipts, changing their private schema/install chain, or projecting/restoring/admitting shared protected delivery gates, read `skills/host-contracts/references/outbound-delivery-receipts.md` in full.
- Resolve the reference within this installed plugin.
- Its full text and named sections bind as rule content.
- Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Authority

- Only the host writes protected intents, native acknowledgments and validated Gmail source bindings.
- Read receipts through the fixed own-main tool.
- Treat public history, caller receipt fields and local emission markers as hints.
- Delivery proves no owner decision, read receipt, pin success or downstream action.

## Recovery and Rollout

- Keep both producer and reader behind default-false host configuration `OUTBOUND_RECEIPTS_ENABLED`; only the runtime owner enables it after actual native acceptance recorded in the existing rollout journal.
- While disabled, allow legacy covered sends only for canonical globally empty receipt storage; any retained history or unknown storage HOLDs before dispatch without resetting gates.
- Keep missing, uncertain, degraded and ambiguous attempts closed without automatic replay.
- Distinguish new native input only through the host-witnessed origin and complete successful-return contract; keep degraded public receipts uncertain and partial/no-ID/error/same-origin/legacy attempts fenced.
- Preserve every pending consumer claim across unsupported schemas or lookup failures.
- Install this authority and the matching host producer before enabling consumer delivery bookkeeping.
- Verify installed native transport behavior before enabling cadence.
