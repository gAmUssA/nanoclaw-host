---
alwaysApply: false
applyTo: "src/channels/whatsapp*, src/whatsapp-*, src/ipc-handlers/whatsapp*, container/agent-runner/src/whatsapp-tools*, docs/WHATSAPP.md — when implementing or verifying WhatsApp channel relinking, private owner pairing, or live phone acceptance"
---

# WhatsApp Channel Control

## Required Read

- Before implementing, invoking or verifying WhatsApp relinking, private pairing or live-phone acceptance, read `skills/host-contracts/references/whatsapp-channel-control.md` in full.
- Its full text and named sections bind as rule content. Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Boundary

- Use the fixed host account and verified owner-only main Telegram control. Keep credentials/keys host-private and pairing codes out of model output/history/logs. Phone approval belongs to the owner; preserve working registration and require saved open-connection proof.
