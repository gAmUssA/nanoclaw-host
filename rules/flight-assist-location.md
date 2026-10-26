---
alwaysApply: false
applyTo: "src/host-plugins/flight-assist-location*, src/location-sinks.ts, skills/flight-assist/** — when writing or consuming current-location.json"
description: Host-owned per-group location snapshot schema, owner filtering, non-owner readers and freshness
---

# Flight-Assist Location Snapshot

## Required Read

- Before writing or consuming the host current-location.json snapshot, read `skills/host-contracts/references/flight-assist-location.md` in full.
- Resolve it within this installed plugin.
- Its full text and named sections bind as rule content.
- Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Boundary

- Only the host publishes location records from the registered owner; preserve agent ownership and private modes.
- Readers remain read-only and never migrate the host artifact.
- Missing, invalid or stale data proves neither airport presence nor absence.
- Keep precise location values out of shared diagnostics.
