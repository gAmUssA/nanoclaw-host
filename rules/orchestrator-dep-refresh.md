---
alwaysApply: true
---

# Orchestrator Dep Refresh

## Required Read

- Before deploying a changed GitHub-sourced npm dependency in Dockerfile.orchestrator, read `skills/host-contracts/references/orchestrator-dep-refresh.md` in full.
- Its full text and named sections bind as rule content. Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Boundary

- Use ./scripts/deploy.sh --no-cache for that refresh and verify the dependency in the running orchestrator. Do not compose an alternate deploy sequence.
