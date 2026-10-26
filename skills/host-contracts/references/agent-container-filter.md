# Agent Container Filter

## Authored scope

- `jbaruch/nanoclaw` owns `scripts/exclude-infra-containers.sh` and its co-shipped consumers `scripts/list-agent-containers.sh` and `scripts/deploy.sh`.
- This named stdout contract is a scoped exception to `coding-policy: script-delegation` JSON output.
- This contract grants no runtime authority and waives no authorization or deployment check.

## Input and output

- Input is newline-delimited Docker container names on stdin.
- Success returns matching per-group agent container names, one per line, in input order.
- An empty result succeeds with empty stdout.
- Exclude the orchestrator `nanoclaw` and persistent infrastructure `nanoclaw-visible-browser`.
- Every emitted name starts with `nanoclaw-`.
- A runtime failure exits nonzero with a diagnostic on stderr.
- Consumers check the filter exit code before signaling any returned container.

## Regression coverage

- `scripts/exclude-infra-containers.test.ts` checks infrastructure exclusion, retained agent names and empty input.
- `scripts/test_deploy_agent_grep.py` checks the composed agent-list/deploy boundary and tool failures.
- Changes to the filter or either consumer preserve those tests and this line-oriented contract.
