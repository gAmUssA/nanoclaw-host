---
alwaysApply: false
applyTo: "src/memory-diet*.ts, container/shared/memory-budget-*, container/agent-runner/src/memory-diet-tools.ts, skills/memory-diet/**, skills/memory-hygiene/**, tests/test_memory_hygiene.py — when producing or reading canonical memory budget metadata"
---

# Memory Budget Policy

## Required Read

- Before producing, invoking or reading the shared memory-budget analyzer, read `skills/host-contracts/references/memory-budget-policy.md` in full.
- Resolve it within this installed plugin.
- Its full text and named sections bind as rule content.
- Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Boundary

- Only the host/image owns the canonical policy and parser; consumers use the fixed immutable image target.
- Missing or unsupported artifacts mean unknown budget, never a separate exemption list or parser.
- Keep active-rule sections live and report their growth without turning exemptions into alarms or extraction targets.
- Install dual-version readers before enabling the new host writer; metadata and lookup grant no memory-write authority.
