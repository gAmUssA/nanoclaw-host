---
alwaysApply: false
applyTo: "src/host-plugins/tessie/**, src/volume-mounts.ts, skills/tessie-*/**, tests/test_native.py, docs/tessie-host-authority.md — when implementing or using native Tessie vehicle authorization, routing, receipts or location projection"
---

# Tessie Host Authority

## Required Read

- Before producing or consuming native Tessie requests, changing protected vehicle policy or gateway routing, or claiming installed readiness, read `skills/host-contracts/references/tessie-host-authority.md` in full.
- Resolve the reference within this installed plugin.
- Its full text and named sections bind as rule content.
- Missing or unreadable content stops the affected action.
- Read explicitly when triggered.
- Never `@`-import the reference.

## Native Ownership

- Host `jbaruch/nanoclaw` owns schema-1 `tessie_request`/journals and schema-2 protected VIN/chat policy.
- Honor protected `non_archived` selection through fresh live inventory, including authorized active shared vehicles; never substitute a frozen VIN snapshot.
- Agent scratch files, roles, payload chat IDs and tier labels grant no vehicle or location authority.
- Use the existing normal OneCLI route and vaulted Tessie API key.
- Keep provider key values out of host request bodies, agent containers, logs and chat.
- Never fall back to direct HTTP or provision a new credential through a consumer.

## Enablement and Receipts

- Keep the default-disabled producer inactive until protected policy, existing gateway and installed authority are ready.
- Merge the actual producer before publishing consumers.
- Require fresh registered scope and live admitted VIN before provider dispatch.
- Preserve accepted controls when confirmation fails.
- Unknown outcomes never permit automatic replay.
- Require post-dispatch state evidence before reporting confirmation.
- Keep runtime vehicle acceptance separate from source tests and publication.
