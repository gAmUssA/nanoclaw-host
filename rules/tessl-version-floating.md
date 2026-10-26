---
alwaysApply: true
---

# Tessl Version Floating

## Required Read

- Before changing a Tessl dependency manifest or relying on the fleet floating-version exception, read `skills/host-contracts/references/tessl-version-floating.md` in full.
- Its full text and named sections bind as rule content. Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Boundary

- Only explicitly covered manifests may float. Covered jbaruch dependencies use managed/latest; every exception retains its named deterministic gate.
