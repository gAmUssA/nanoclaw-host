---
alwaysApply: false
applyTo: "src/channels/whatsapp*, src/whatsapp-*, src/ipc-handlers/whatsapp*, container/agent-runner/src/whatsapp-tools*, docs/WHATSAPP.md — when implementing or verifying WhatsApp channel relinking, private owner pairing, or live phone acceptance"
---

# WhatsApp Channel Control

## Authority

- `jbaruch/nanoclaw` owns the fixed-account channel, host control and private pairing delivery.
- Keep Baileys credentials and Signal keys in host-private storage outside agent mounts and backups.
- Reuse configured account and phone; accept no caller-supplied account, path, command or delivery target.

## Owner Control

- Authorize from the verified literal main IPC directory and current registered owner-only Telegram surface.
- Keep pairing codes out of agent tool results, history and ordinary logs.
- Deliver a fresh code with its remaining validity and phone steps through sensitive owner-only delivery.
- Retire displayed codes on rotation, expiry or connection; retain phone approval with the owner.
- Preserve valid registered credentials and recovery originals when replacing unusable attempts.
- Serialize requests and bound pairing attempts; allow a later explicit owner retry after exhaustion.
- Confirm success only after the connection opens and registered credentials are saved.

## Live Interaction Testing

- Apply `coding-policy: testing-standards` Platform-Bound Untestable Carve-Out only to the smallest invocation layer for these external interaction classes:
  1. Native Baileys WhatsApp Web pairing transport through `src/channels/whatsapp.ts` `defaultSocketFactory` and the real socket's `requestPairingCode` call.
  2. Owner phone Linked devices approval and readback of the real account's open connection and persisted registered credentials.
- Keep deterministic authorization, lifecycle, timers, filesystem operations, code privacy, request serialization and failure handling covered by CI-executable tests.
- Follow `jbaruch/nanoclaw` `docs/WHATSAPP.md`, “Live relink acceptance after deployment”, for manual validation of those two interaction classes.
- Use an explicitly authorized pairing session; do not revoke a working account for a test.
- Verify fresh private code delivery, owner phone approval, saved registration and one self-chat intake.
- Verify Telegram, a preexisting agent session and the dedicated browser remain running during relinking.
- Exercise expiry, exhaustion, concurrent requests, rejected callers and credential-save failure without exposing live codes or auth data in evidence.
- Treat synthetic sockets as deterministic evidence only; require actual installed acceptance before claiming live relinking complete.

## Rollout

- Publish and install this authority before relying on the live-interaction testing carve-out.
- Ship agent tools and host IPC/controller together through the reviewed NanoClaw deployment.
- Keep live acceptance open until the owner completes phone approval and installed behavior is verified.
