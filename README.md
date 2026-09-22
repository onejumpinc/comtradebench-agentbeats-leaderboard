# ComtradeBench AgentBeats Leaderboard

This repository contains the **AgentBeats leaderboard and submission infrastructure** for the ComtradeBench evaluation suite.

> **Main project:** [ComtradeBench / OpenEnv](https://github.com/yonghongzhang-io/comtrade-openenv)

ComtradeBench evaluates AI-agent reliability under realistic API failure modes such as pagination errors, rate limits, server faults, duplicates, schema drift, and totals-row traps. This repository is a supporting component of that broader research project.

## Benchmark scope

The current evaluation covers seven task families:

| Task | What it tests |
|---|---|
| Single page | Basic API interaction and parsing |
| Multi-page | Pagination correctness |
| Duplicates | De-duplication |
| Rate limit | Retry/backoff under HTTP 429 |
| Server error | Recovery from HTTP 500 |
| Page drift | Stable retrieval under ordering drift |
| Totals trap | Detection and removal of totals rows |

## Scoring

Each task is evaluated on three dimensions:

- **Completeness** — required outputs are present and valid
- **Correctness** — retrieved and processed data match expected results
- **Robustness** — failures, retries, and edge cases are handled correctly

The authoritative benchmark logic is implemented in the Green agent / judge repository.

## Submission workflow

This fork stages the independently verified One Jump participant for a public
run. Its exact image earned `700/700` in each of two fresh full-stack suites in
[release run 35687874930](https://github.com/onejumpinc/comtrade-deterministic-agent/actions/runs/35687874930).

The workflow is deliberately manual-only and currently inert:
`COMTRADE_AGENT_ID` is a placeholder in the workflow, `scenario.toml`, and the
compatibility copy `scenario.ci.toml`. After the digest-pinned manifest has
been registered on AgentBeats, replace all three occurrences with the resulting
lowercase UUID and dispatch the workflow from `main`.

Before creating a submission branch, the workflow verifies:

- the AgentBeats registration owner, color, category, repository, and immutable
  image or manifest locator;
- the pinned manifest checksum and exact green, mock, participant, and client
  image digests;
- all seven task identities, exact row/request counts, every score component,
  empty judge errors, and an aggregate score of `700/700`;
- real HTTP 429/500 behavior, seven green-owned mock configurations, no
  participant `/configure` call, and complete GitHub Actions provenance.

No registry or model secret is required because every runtime image is public
and the participant is model-free. A branch suitable for an upstream pull
request is created only after all gates pass.

## Related repositories

- **[ComtradeBench / OpenEnv](https://github.com/yonghongzhang-io/comtrade-openenv)** — main research repository and execution environment
- **[Green Comtrade Bench v2](https://github.com/yonghongzhang-io/green-comtrade-bench-v2)** — deterministic benchmark / judge implementation
- **[Purple Comtrade Baseline v2](https://github.com/yonghongzhang-io/purple-comtrade-baseline-v2)** — reference baseline agent

## Research context

The benchmark is motivated by real-world tool-use settings in which an agent must do more than produce a plausible answer: it must retrieve data correctly, recover from failures, produce auditable outputs, and remain reliable under adversarial or unstable API conditions.

For the research framing, evaluation results, and broader benchmark design, see the main **ComtradeBench / OpenEnv** repository.
