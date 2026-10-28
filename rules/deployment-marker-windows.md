---
alwaysApply: false
applyTo: "scripts/deploy.sh, src/deploy-private-state.py, skills/heartbeat/scripts/heartbeat-checks.py — when writing, repairing or reading deployment window markers"
---

# Deployment Window Markers

## Required Read

- Before every deployment marker write, repair or read, load `skills/host-contracts/references/deployment-marker-windows.md` in full from installed `jbaruch/nanoclaw-host` or this repository when maintaining it.
- Its full text binds as rule content.
- Missing or unreadable content stops the affected action until repaired.
- Read explicitly when triggered.
- Never `@`-import the reference.

## Ownership

- Canonical deployment owns markers through its fixed private-state helper.
- Heartbeat readers never write, repair or migrate marker history.
- Publish compatible readers and this contract before deploying a new writer format.

## Unknown Coverage

- Preserve exact legacy originals as inactive archives before owner migration.
- Preserve unknown history unchanged.
- Report unsupported, malformed or unreadable history as actionable unknown coverage.
- Unknown coverage never supplies suppression or a healthy result.
