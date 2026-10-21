---
alwaysApply: true
---

# Browser Profile Storage

## Scope

- `jbaruch/nanoclaw` may persist website cookies, session storage and saved website passwords for its dedicated `visible-browser` service.
- Reuse approved website access autonomously across turns and restarts.
- API credentials remain in OneCLI.
- Website access grants no authority for unrelated actions on that website.

## Storage preconditions

- Persist authenticated profile data only as authenticated encrypted archives or on an encrypted filesystem.
- Keep the live decrypted profile in the dedicated non-agent browser service, on private memory-backed storage or the encrypted filesystem.
- Use owner-only profile directories and ciphertext files.
- Keep the profile encryption key outside the project tree, group mounts, images, logs and ordinary backups.
- Permit a dedicated mode-600 host key file when the storage adapter requires local key bytes; mount it read-only only into the non-agent browser service.
- Never put that key in `.env`, command arguments, an agent container or model context.
- Provision and unlock storage unattended after the initial setup.
- Missing keys, failed authentication and corrupt archives fail closed without replacing the previous ciphertext.

## Access and retention

- Only the literal main-tier agent may control browser sessions through the private CDP bridge.
- Private operator viewing uses the existing approved device/identity path; never expose CDP or enable Funnel.
- Browser autofill may reuse saved website credentials for an approved account and matching origin.
- Agents must not reveal, export or log passwords, cookies, storage tokens or profile encryption keys.
- CAPTCHA solving is operator-approved; MFA requires operator handoff.
- Retain approved sessions and saved passwords until the operator removes them or the website expires or revokes them.
- Never clear authentication on a recurring schedule or routine deployment.

## Checkpoint and recovery

- Checkpoint after initial authentication and before controlled browser shutdown or recreation.
- Create consistent encrypted checkpoints after the browser flushes its profile.
- Replace the previous ciphertext only after the new checkpoint succeeds.
- Exclude decrypted profiles and keys from the ordinary group/git backup.
- Restore checkpoints only into the same dedicated browser identity and storage boundary.
- Document the last successful checkpoint and crash-recovery limits.
- Verify saved-login reuse, encrypted storage, restart, recreation and failure recovery with synthetic credentials before production use.
