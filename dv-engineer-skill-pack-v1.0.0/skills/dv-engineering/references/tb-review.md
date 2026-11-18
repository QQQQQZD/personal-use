# Understand Code and Review Existing Verification

Use this reference for code/TB review, not main-spec-only planning. For document
review use [document-review.md](document-review.md); for planning use [planning.md](planning.md).

## 1. Establish the contract

Read the relevant design spec first when available. Identify interfaces, legal
configurations, state transitions, transaction acceptance/completion, ordering,
latency constraints, reset, and error behavior. Reuse feature IDs where available. If local traceability labels are needed,
identify them as local rather than official spec IDs; do not create a duplicate
requirement hierarchy for feature-list generation.

Record contradictions and unresolved behavior. Continue work unaffected by them;
request clarification before implementing a consequential interpretation.

## 2. Trace active code, not directory names

Start at the selected top/configuration and follow instantiations, defines,
parameters, factory overrides, config settings, sequence inheritance, and port
connections relevant to the feature. Read effective sequence `body()` methods,
including inherited implementations; an empty derived class can be valid.

Map source stimulus, driver acceptance semantics, monitor sampling, prediction,
scoreboard matching, coverage sampling, and test completion. Check that they are
connected and enabled in the actual configuration.

For SoC, bound the trace by the target feature's end-to-end dependencies. Establish
contracts of intervening fabric, memories, reset domains, and software setup as
needed. Do not read all SoC RTL as a prerequisite for a narrow question.

## 3. Audit the correctness oracle

Identify where expected outcomes come from: behavioral model, architectural
invariant, transaction-level predictor, independent computation, or protocol
contract. A component named scoreboard does not establish that a useful oracle
exists. Flag absent or inadequate prediction/comparison as a leading gap.

Check whether expected data is copied from DUT results or shares implementation
errors with the DUT. A count can check a count requirement but cannot, by itself,
prove payload, identity, ordering, or timing correctness. A timeout establishes
bounded progress, not complete functional correctness.

For a checker review, present:

| Checker and source | What it actually checks | What it misses | Requirement / impact |
| --- | --- | --- | --- |

Inspect transaction identity, duplicates, unsolicited outputs, missing outputs,
ordering/reordering rules, variable latency, reset cancellation, and leftover
transactions at end of test. Determine how a deliberate relevant error would
reach an actionable failure; do not assume coverage sampling is checking.

## 4. Audit scenarios and constraints

For each applicable requirement, examine normal operation, minimum/maximum and
off-by-one boundaries, backpressure, concurrency, contention/resource exhaustion,
back-to-back traffic, reset during activity, recovery, and legal error scenarios.
Select based on the design contract; do not demand unsupported behaviors.

Check whether constraints, defaults, small entity counts, disabled features, or
initialization assumptions make intended scenarios unreachable. Distinguish
illegal stimulus robustness tests from legal protocol operation.

Use a requirement-to-test matrix with separate stimulus and checking status:

| Requirement | Stimulus/test evidence | Expected result / checker | Coverage evidence | Gap |
| --- | --- | --- | --- | --- |

Describe entries as adequately checked, exercised with weak checking, not exercised,
or unknown. Distinguish planned, implemented, and observed-in-run evidence.
Do not claim measured coverage from reading source code alone.

## 5. Report bounded findings

For each material finding provide location, requirement basis, specific escape
scenario, impact, and proposed verification improvement. Separate confirmed
defects, verification gaps, and questions. Explain severity; avoid unsupported
claims of chip failure from a TB gap alone.

Order completeness findings: oracle gaps -> checker adequacy -> feature/scenario
gaps -> relevant edge cases -> architecture -> optional maintainability remarks.
Do not recommend refactoring merely because code differs from a preferred style.
