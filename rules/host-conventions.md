---
alwaysApply: true
---

# Host Agent Conventions

Deployment mechanics for the NanoClaw host agent (Claude Code on Mac). Other host-tier rules cover collaboration, ownership, and infrastructure patterns — see this plugin's `README.md` rules table.

## Always deploy with deploy.sh

Never run `docker compose up -d --build` directly. Always use `./scripts/deploy.sh` — it pulls code, rebuilds, runs `tessl update` to fetch the latest plugins from the registry, clears overrides, kills stale containers, clears sessions, and restarts. Skipping this means the orchestrator runs without the latest published plugins.

## Registry is the delivery artifact

Tessl registry plugins are what gets delivered to containers. Git is the source, not the delivery mechanism. Never skip publishing — always run the full promote pipeline.

## Scripts use common.sh

All scripts in `scripts/` source `scripts/common.sh` for shared config (`NAS_HOST`, `NAS_PROJECT_DIR`, `nas()` helper). No hardcoded IPs or paths.

## Credentials live in the vault

- Runtime credentials live in the OneCLI vault, never the NAS `.env`
- `jbaruch/nanoclaw` `scripts/deploy.sh` step 1b refuses to deploy while `.env` holds a credential-shaped variable outside `scripts/env-credential-allowlist.json` (`scripts/check-env-credentials.py`)
- The allowlist holds `ONECLI_API_KEY`, the key that reaches the vault, plus each variable an open migration issue still covers, citing that issue
- Remove an allowlist entry when its issue closes
- A credential a host script needs outside the vault gets its own mode-600 file outside the project tree, read only by that script (`~/.config/nanoclaw/trakt.env`, jbaruch/nanoclaw#1045)
