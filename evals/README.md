# Evals

- `<case>/`: 8 behavior test cases in the `claude plugin eval` layout (`prompt.md`, `case.yaml`, `graders/`).
- `trigger-queries.json`: 20 description-trigger queries, 10 that should load the skill and 10 near misses that should not.

To run the cases with Claude Code, copy this folder into one skill folder and run `claude plugin eval .` there. To optimize the description, use skill-creator's `run_loop` with `trigger-queries.json`. Both need the `claude` CLI.

## Results for 3.0.0 (2026-11-01)

### Behavior cases: v3 vs v2

Each case was run once per version by a separate subagent with no browser or search tools, then graded against its `graders/` statements.

| Case | v3 | v2 |
|---|---|---|
| breakeven-cash-refund | 5/5 | 3/5 (uses 156 / 501 = 31% and picks the wrong fare) |
| quick-domestic-stays-short | 6/6 | 3/6 (474 words, 4 questions, wide table) |
| yvr-sea-pickup-time | 4/4 | 4/4 |
| yyz-preclearance-connection | 5/5 | 5/5 |
| no-browser-pasted-results | 5/5 | 5/5 |
| family-bags-true-total | 5/5 | 5/5 |
| audit-other-agent-errors | 5/5 | 5/5 |
| post-booking-cancellation-out-of-scope | 3/3 | 3/3 |

The first v3 run of the Quick case failed (509 words, 3 questions). The Quick mode output budget in SKILL.md was added after that run, and two reruns passed (270 and 338 words). Six cases do not separate the versions yet.

### Description triggering

**Method.** skill-creator's trigger eval runs `claude -p` and was not available (no `claude` CLI on the test machine). Substitute: a fresh subagent per run is shown only a list of skills (name and description) and the user messages, and must say which skill, if any, Claude would load. Two set-ups:

- **Crowded**: this skill plus 8 distractors (hotel booking, trip itineraries, airline claims after a disruption, web search, fare tracker, points and miles, Python automation, world clock). 3 runs per description, each with the skill list and the queries in a different random order. Each run judged all 20 queries, one after another, told to treat each as a fresh conversation.
- **Sparse**: this skill plus web search, Python automation and hotel booking only, so that the near misses have fewer other skills to go to. 2 runs per description.
- **Single-query check**: 6 of the hardest queries (q1, q7, q12, q14, q18, q20) each in a fresh subagent on their own, sparse set-up, v3 English description.

A query counts as triggered in a run when the chosen skill is this one. "Majority" uses a 0.5 threshold, as `run_eval` does.

**Results (triggered runs / runs).** Queries not listed triggered in every run for every description (should-trigger) or in no run (near misses).

| Query | Should trigger | v2 en | v2 zh | v3 en | v3 zh |
|---|---|---|---|---|---|
| q1 Shanghai to Vancouver via Seattle, bag pick-up | yes | 1/3, sparse 2/2 | 0/3 | 3/3, 2/2 | 3/3, 2/2 |
| q5 Chinese: Vancouver to Toronto next Friday, which is better value | yes | 2/3, sparse 1/2 | 3/3 | 3/3, 2/2 | 3/3, 2/2 |
| q7 SFO to JFK Thursday morning, something reasonable | yes | 0/3, sparse 0/2 | 1/3 | 3/3, 2/2 | 3/3, 2/2 |
| q11 to q20, all 10 near misses | no | 0 of 50 runs | 0 of 30 | 0 of 50 | 0 of 50 |

| Description | Recall (majority) | Precision (majority) | Should-trigger runs | Near-miss runs that triggered |
|---|---|---|---|---|
| v2 en, crowded | 0.80 | 1.00 | 24/30 | 0/30 |
| v2 zh, crowded | 0.80 | 1.00 | 25/30 | 0/30 |
| v3 en, crowded | 1.00 | 1.00 | 30/30 | 0/30 |
| v3 zh, crowded | 1.00 | 1.00 | 30/30 | 0/30 |
| v2 en, sparse | 0.90 | 1.00 | 17/20 | 0/20 |
| v3 en, sparse | 1.00 | 1.00 | 20/20 | 0/20 |
| v3 zh, sparse | 1.00 | 1.00 | 20/20 | 0/20 |

The single-query checks agreed with the batched runs: q1 and q7 loaded this skill; q12, q14 and q18 went to web search; q20 used no skill.

v2 missed the plain "find me a flight" request (q7) and the bag re-check question (q1). The v3 description names both cases.

**Limits.** This is a simulation of the skill-selection step, not Claude Code's real loader: the model knows it is being asked to choose a skill, and in the crowded and sparse runs it saw 20 queries in one context. The near misses may be too easy when a matching distractor exists; the sparse set-up removes most of those distractors and still shows no false triggers. Rerun `run_loop` with the `claude` CLI before trusting small differences.
