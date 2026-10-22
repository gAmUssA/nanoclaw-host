---
alwaysApply: true
---

# Learning Evidence Runtime

## Authority

- The host owns the runtime contract in `skills/learning-evidence-runtime/runtime.json` for `jbaruch/nanoclaw-admin` learning-evidence consumers.
- The manifest declares the scanner dependency, its image-installation pin, renewal owner and the bounded source-view protocol.
- A consumer rejects unsupported or incomplete contracts before enabling the view.

## Source view

- Only the registered main group receives the read-only source view.
- The host selects that group's SDK project-transcript directories and projects them into the declared namespace by group folder and session name.
- SDK settings, skills and credential directories stay outside the view.
- The original SDK session trees stay separate for writes.
- The host masks the original session-tree alias under the administrative project view while the projection is enabled.
- Native skill processes receive their chat and session context from host input.
- Consumers resolve the current chat through `NANOCLAW_CHAT_JID` and the registered `groups` array in `/workspace/ipc/available_groups.json` before selecting a source.
- Source identifiers cannot select another group, a link or a path outside the projected session directory.
- Read source bytes only through the published provenance and credential auditor; a mount supplies no approval authority.

## Scanner dependency

- `jbaruch/nanoclaw` installs the declared Gitleaks binary from the named image stage in `container/Dockerfile`.
- That stage pins its upstream image by tag and SHA-256 digest.
- The committed `/container` Docker Dependabot job proposes reviewed renewal of both image references.
- The minimum compatible scanner version in the runtime manifest is an interface bound, not a second resolved image pin.
- Runtime callers never download a scanner or accept session-supplied scanner configuration, baselines or allow comments.
- Missing, incompatible or failed scanners yield no learning evidence; supplemental checks cannot replace the detector.
