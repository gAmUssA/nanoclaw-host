---
alwaysApply: false
applyTo: "src/host-plugins/tessie/**, src/volume-mounts.ts, skills/tessie-*/**, tests/test_native.py, docs/tessie-host-authority.md — when implementing or using native Tessie vehicle authorization, routing, receipts or location projection"
---

# Tessie Host Authority

## Required Read

- Before producing or consuming native Tessie requests, changing protected vehicle policy or native isolation, or claiming installed readiness, read `skills/host-contracts/references/tessie-host-authority.md` in full.
- Resolve the reference within this installed plugin.
- Its full text and named sections bind as rule content.
- Missing or unreadable content stops the affected action.
- Read explicitly when triggered.
- Never `@`-import the reference.

## Native Ownership

- Host `jbaruch/nanoclaw` owns schema-1 `tessie_request` and protected VIN/chat policy.
- Agent scratch files, roles, payload chat IDs and tier labels grant no vehicle or location authority.
- Require a dedicated host actor plus actual published org-wide OneCLI Tessie isolation.
- Never substitute a representative credential status or a finite tier sample for domain denial.
- Never fall back to direct HTTP or provision a new credential through a consumer.

## Enablement and Receipts

- Keep the default-disabled producer inactive until protected policy, native fence and installed authority are ready.
- Merge the actual producer before publishing consumers.
- Require fresh registered scope and live admitted VIN before provider dispatch.
- Preserve accepted controls when confirmation fails.
- Unknown outcomes never permit automatic replay.
- Require post-dispatch state evidence before reporting confirmation.
- Keep runtime vehicle acceptance separate from source tests and publication.
