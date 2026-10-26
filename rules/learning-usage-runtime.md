---
alwaysApply: false
applyTo: "src/state-migrations/**, src/learning-usage-*.ts, src/credential-proxy.ts, src/container-runner.ts, container/agent-runner/src/native-context-env.ts, src/volume-mounts.ts, src/index.ts, skills/learning-pass/**, skills/learning-apply/**, tests/test_learning_usage.py — when writing, publishing or reading host-observed learning usage"
---

# Learning Usage Runtime

## Required Read

- Before writing, publishing, reading or relying on host-observed learning usage, read `skills/host-contracts/references/learning-usage-runtime.md` in full.
- Resolve it within this installed plugin.
- Its full text and named sections bind as rule content.
- Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Boundary

- Only the host writes or migrates the private capture store and published usage projection.
- Keep capture outside agent databases and mounts; mask its aliases even when capture fails and refuse startup without a verified empty mask.
- Only main receives the kernel read-only usage companion and separate admitted-target registry view.
- Missing coverage means unknown; a row, digest or archive candidate supplies no owner identity, approval or archive authority.

- Learning update continuations require the host-minted version-1 bootstrap context and a different valid maintenance bootstrap after the update.
