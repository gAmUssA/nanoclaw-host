---
alwaysApply: true
---

# Agent File Permissions

## Identity

- Apply this contract to host operations that create, replace or restore agent-managed files, including SSH, privileged container commands, deploys, checkpoints, downloads and skill scripts.
- Resolve the agent's numeric UID/GID from current runtime configuration and the container's effective user before writing.
- Use the runtime's normalized container identity; root, the SSH user and an assumed UID are not substitutes.
- Run agent-state writers as that identity, including host-invoked skill scripts.
- Keep a privileged orchestrator privileged only where required; create its agent-facing output with the agent's ownership before exposing the file.

## Private Writes

- Create private documents and state at `0600`, and their private directories at `0700`.
- Preserve a different mode only when the owning runtime contract requires shared access.
- Preserve ownership and restrictive modes through temporary files, atomic replacements, rotations and restores.
- For required privileged repairs, restore the intended owner and mode before declaring success.
- Do not use world-readable defaults, `chmod 777` or blanket recursive ownership changes.

## Boundaries

- Limit repairs to verified agent-managed paths; preserve host-managed paths and unrelated users' files.
- Refuse symlinks, multiply linked files, special files and unexpected mount crossings during privileged repairs.
- Anchor privileged filesystem operations to verified directory and file descriptors; do not rely on a prior path check alone.
- Keep document contents, credential values and private filenames out of shared diagnostic output.
- Verify restored state before moving recovery originals into a private archive outside active group state.
- Preserve the recovery copy until an authorized retention decision permits deletion.

## Verification and Delivery

- Verify final numeric UID/GID and modes after writes or repairs.
- Verify read/write access as the actual agent identity; a root-level success is insufficient.
- Check existing-file updates, new files, atomic replacement and privileged restore when changing a writer.
- Keep permission health checks read-only and report unresolved access failures without exposing private contents.
- Fix recurring writers; a later ownership sweep does not satisfy prevention.
- Publish this rule through the release pipeline and verify the installed rule in each host checkout used for maintenance.
