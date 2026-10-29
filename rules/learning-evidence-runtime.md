---
alwaysApply: false
applyTo: "skills/learning-evidence*/**, rules/learning-evidence*.md, src/session-transcript-view*, src/volume-mounts.ts, container/Dockerfile — when implementing or verifying the learning evidence runtime"
---

# Learning Evidence Runtime

## Required Read

- Before implementing, enabling or verifying the learning source view or its scanner dependency, read `skills/host-contracts/references/learning-evidence-runtime.md` in full.
- Its full text and named sections bind as rule content. Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Boundary

- Only registered main receives its own group's read-only audited transcript projection. Credentials, SDK settings and skills remain outside it. Failed or unavailable provenance/scanning supplies no evidence; a mount grants no approval.
