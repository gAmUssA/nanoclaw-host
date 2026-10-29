---
alwaysApply: true
---

# Persona Persist Direct-Push

## Required Read

- Before implementing or relying on persona persistence or its direct-push exception, read `skills/host-contracts/references/persona-persist-direct-push.md` in full.
- Its full text and named sections bind as rule content. Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Boundary

- Only groups/global/SOUL.md and groups/global/SOUL-untrusted.md may direct-push, through the approved host flow and deterministic whole-push allowlist. All other changes require a reviewed PR.
