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

- Narrow exception for `jbaruch/nanoclaw` `scripts/check-env-credentials.py`.
- Applies only to this standalone credential-input validator
- Preconditions (all required):
  1. It reads candidate env and allowlist files as data; it never sources or executes them
  2. File paths come from explicit CLI arguments or from its resolved checkout location
  3. It performs no NAS, SSH, or other network operations
  4. Tests cover default paths, CLI overrides, input non-execution, and deploy failure gating
- Every other script in `scripts/` sources `scripts/common.sh`

## Credentials live in the vault

- Runtime credentials live in the OneCLI vault, never the NAS `.env` or a compose file
- `~/nanoclaw/.env` holds non-secret configuration
- `jbaruch/nanoclaw` `scripts/deploy.sh` step 1b refuses to deploy while `.env` holds a credential-shaped variable outside `scripts/env-credential-allowlist.json` (`scripts/check-env-credentials.py`)
- Narrow exception for the `ONECLI_API_KEY` bootstrap credential that reaches the vault.
- Applies only to host-side OneCLI control-plane authentication
- Preconditions (all required):
  1. It is the allowlist's sole permanent bootstrap entry
  2. The NAS `.env` is mode 600, excluded from git, and never mounted or forwarded into an agent container
  3. Host code reads the key without printing it or placing it in process arguments
- Narrow exception for a credential an open migration issue still moves out of `.env`.
- Applies while that issue is open
- Preconditions (all required):
  1. Its allowlist entry cites the issue as `owner/repo#N`
  2. The entry is removed when the issue closes
- Narrow exception for a credential no vault injection path can carry, such as a request-body OAuth refresh secret.
- Applies to a host script that must read the value itself
- Preconditions (all required):
  1. An open issue tracks moving it into the vault
  2. It lives in its own env file outside the project tree, mode 600, read only by that script
  3. The script never prints it
- Narrow exception for website sessions and saved website passwords in the dedicated NAS browser.
- Preconditions are binding: read `rules/browser-profile-storage.md` before enabling authenticated persistence.
- Narrow exception for native identity/ACME state of the private Tailscale noVNC viewer.
- Preconditions are binding: read `rules/browser-tailnet-viewer.md` before relying on it.
- Every other credential lives in the vault
