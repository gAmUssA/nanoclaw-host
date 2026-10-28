---
alwaysApply: false
applyTo: "scripts/deploy.sh, src/deploy-private-state.py, skills/heartbeat/scripts/heartbeat-checks.py — when writing, repairing or reading deployment window markers"
---

# Deployment Window Markers

## Required Read

- Read `skills/host-contracts/references/deployment-marker-windows.md` in full before changing deployment window writers, permissions or suppression readers.
- Its full text binds as rule content. Missing or unreadable content stops the affected action until repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Ownership

- Canonical deployment owns append-only markers through its fixed private-state helper.
- Heartbeat readers never write, repair or migrate marker history.
- Publish compatible readers and this contract before deploying a new writer format.

## Unknown Coverage

- Preserve legacy rows and unknown history without truncation or rewriting.
- Report unsupported, malformed or unreadable history as actionable unknown coverage.
- Unknown coverage never supplies suppression or a healthy result.
