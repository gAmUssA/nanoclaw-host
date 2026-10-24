---
alwaysApply: false
applyTo: "container/visible-browser/**, container/skills/visible-browser/**, src/visible-browser*, docker-compose.yml, scripts/deploy.sh, skills/watchers/** — when implementing, operating or using authenticated persistence for the dedicated browser"
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
- OneCLI or another secrets manager provisions the profile encryption key.
- Runtime setup may materialize that managed key into a dedicated mode-600 host cache outside the project.
- Mount the managed key cache read-only only into the non-agent browser service.
- Never put that key in `.env`, command arguments, an agent container or model context.
- Provision and unlock storage unattended after the initial setup.
- Missing keys, failed authentication and corrupt archives fail closed without replacing the previous ciphertext.

## Access and retention

- Only the literal main-tier agent may control browser sessions through the private CDP bridge.
- Private operator viewing uses the existing approved device/identity path.
- Never expose CDP or enable Funnel.
- Browser autofill may reuse saved website credentials for an approved account and matching origin.
- Agents must not reveal, export or log passwords, cookies, storage tokens or profile encryption keys.
- CAPTCHA solving is operator-approved.
- MFA requires operator handoff.
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

## Native visible-browser lookup

- Host `jbaruch/nanoclaw` installs `container/visible-browser/validate-agent-cdp.mts` as `/usr/local/lib/nanoclaw/validate-visible-browser.mts` in the agent image.
- Invoke only `node /usr/local/lib/nanoclaw/validate-visible-browser.mts`; it takes no stdin or task-supplied endpoint.
- The host injects non-secret `VISIBLE_BROWSER_CDP_URL` only for main's private browser attachment; operators never set this capability variable in `.env`.
- Exit 0 emits only JSON `{"available":true}` and a newline; unavailable, substituted, malformed or headless results exit nonzero with empty stdout and an actionable redacted diagnostic.
- Preserve every access and retention constraint after this lookup; availability grants no owner identity or website action authority.
- Missing target or incompatible output requires repairing the installed image, never local-browser fallback or credential export.
- Regression coverage: `jbaruch/nanoclaw` `src/visible-browser-guard.test.ts`; native browser coverage also runs `container/visible-browser/runtime-canary.mts`.
