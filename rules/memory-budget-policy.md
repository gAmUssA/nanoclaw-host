---
alwaysApply: false
applyTo: "src/memory-budget*, src/memory-diet*, container/shared/memory-budget-*, container/agent-runner/src/memory-diet-tools*, tests/fixtures/memory-diet-unicode.json, skills/memory-diet/**, skills/memory-hygiene/**, tests/test_memory_*.py — when analyzing canonical memory budgets or operating memory-diet plans, results or journals"
---

# Memory Budget Policy

## Required Read

- Before producing, invoking or reading the shared analyzer, reading or writing memory-diet plans/results, or migrating their private journals, read `skills/host-contracts/references/memory-budget-policy.md` in full.
- Resolve it within this installed plugin.
- Its full text and named sections bind as rule content.
- Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered.
- Never `@`-import the reference.

## Boundary

- Only the host/image owns the canonical policy and parser.
- Consumers use the fixed immutable image target.
- Missing or unsupported artifacts mean unknown budget.
- Never substitute a separate exemption list or parser.
- Keep active-rule sections live and report their growth without turning exemptions into alarms or extraction targets.
- Install dual-version readers before enabling the new host writer.
- Metadata and lookup grant no memory-write authority.
