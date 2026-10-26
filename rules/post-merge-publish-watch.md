---
alwaysApply: true
---

# Post-Merge Publish Watch

## Required Read

- Before shipping or merging a plugin repository, read `skills/host-contracts/references/post-merge-publish-watch.md` in full.
- Its full text and named sections bind as rule content. Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Boundary

- Use the coding-policy release skill before merge; publication is a separate gate. Resolve publish.yml by filename and never report an unresolved run as shipped.
