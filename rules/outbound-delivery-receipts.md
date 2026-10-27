---
alwaysApply: false
applyTo: "src/outbound-delivery-*, src/ipc-handlers/outbound-receipts*, src/channels/telegram*, container/agent-runner/src/outbound-receipt-tools*, skills/waiting-on-owner/**, skills/email-surface/** — when producing or reading native outbound delivery receipts or deduplicating reports from confirmed delivery"
---

# Outbound Delivery Receipts

## Required Read

- Before producing, querying or accepting delivery receipts, read `skills/host-contracts/references/outbound-delivery-receipts.md` in full.
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

- Keep missing, uncertain, degraded and ambiguous attempts closed without automatic replay.
- Preserve every pending consumer claim across unsupported schemas or lookup failures.
- Install this authority and the matching host producer before enabling consumer delivery bookkeeping.
- Verify installed native transport behavior before enabling cadence.
