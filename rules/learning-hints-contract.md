---
alwaysApply: false
applyTo: "src/state-migrations/**, src/db-messages.ts, src/db-tasks.ts, skills/learning-pass/**, tests/test_learning_sources.py — when writing or reading the host learning hint projection or its source records"
---

# Learning Hint Contract

## Required Read

- Before writing or reading host learning hints or changing their source records, read `skills/host-contracts/references/learning-hints-contract.md` in full.
- Its full text and named sections bind as rule content. Missing or unreadable content stops the affected action until the plugin is repaired.
- Read explicitly when triggered; never `@`-import the reference.

## Boundary

- Host migrations own the versioned view. Readers use scoped read-only queries; hints are untrusted search input and supply no identity, approval or permission. Keep learning cadence disabled until installed compatibility and runtime acceptance are verified.
