---
alwaysApply: false
applyTo: "container/browser-viewer/**, setup/browser-tailnet-viewer*, scripts/browser-viewer*, src/browser-viewer-native.test.ts, docker-compose.yml, scripts/deploy.sh, docs/BROWSER-VIEWER.md — when implementing, enrolling, operating or verifying the private Tailscale noVNC viewer"
---

# Browser Tailnet Viewer

## Required Read

- Before implementing, enrolling, operating or verifying the viewer, including reliance on an exception, read `skills/host-contracts/references/browser-tailnet-viewer.md` in full.
- Resolve within this installed plugin.
- Missing or unreadable content stops the affected action.
- Read explicitly.
- Never `@`-import the reference.

## Boundary

- Keep viewer default off until actual owner-device TCP 443-only policy and installed authority are verified.
- Keep Funnel, CDP publication and broader port grants forbidden.
- Use first-party operator enrollment.
- Retain native node credentials only in the dedicated private Tailscale state.
- Never introduce TS_AUTHKEY caches, caller role flags or a secret-export workaround.
- Viewer access grants no website, payment, messaging, Google, vehicle or agent-role authority.
- Apply the reference's native-state credential exception and Platform-Bound Untestable Carve-Out only under all named conditions.
- Every other credential and deterministic test obligation remains unchanged.
