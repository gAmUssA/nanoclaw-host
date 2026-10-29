---
alwaysApply: false
applyTo: "scripts/exclude-infra-containers.sh, scripts/list-agent-containers.sh, scripts/deploy.sh — when invoking or modifying agent-container enumeration"
---

# Agent Container Filter

## Required Read

- Before invoking or modifying agent-container enumeration or a consumer that signals containers, read `skills/host-contracts/references/agent-container-filter.md` in full.
- Its full text and named sections bind as rule content. Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Boundary

- Preserve infrastructure exclusions and check the filter exit code before signaling any returned container. This lookup grants no runtime authority.
