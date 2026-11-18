# Implement and Validate Verification Code

Read [coding-review.md](coding-review.md) for cross-document traceability and
feedback. Read [coverage.md](coverage.md) when implementing coverage.

For a new IP/module environment, first read
[new-environment.md](new-environment.md). It defines the two input cases,
test/task-based orchestration, and framework-review gate before implementation.

## Before editing

Identify the requirement, current failure or missing detection capability, active
configuration, and minimal files needing change. Inspect local interfaces, base
classes, ownership conventions, and established API usage. Preserve unrelated
user work. Do not change RTL unless the requested task authorizes RTL changes.

Write down what must fail before the change and what should pass after it. When
possible, add a focused regression for a confirmed bug. Do not fabricate APIs,
signals, hierarchy paths, register definitions, or supported simulator options.

## Design the verification path

- Stimulus: drive legal traffic by default, check randomization success, make
  negative stimulus deliberate, and retain seed/config reproducibility.
- Drivers/monitors: use the actual acceptance handshake and sampling clock/edge;
  respect stability under stalls, clocking-block conventions, and reset semantics.
  Avoid races and monitor assumptions derived only from intended stimulus.
- Prediction: derive expectations from the applicable contract and accepted
  inputs/configuration. Account for masks, widths, signedness, byte order,
  truncation/rounding/saturation and numerical tolerances where applicable.
- Scoreboards: define matching keys, allowed reordering, duplicate/missing/spurious
  detection, bounded pending state, reset flushing rules, and end-of-test checks.
  Do not erase pending state merely to make shutdown pass.
- Object ownership: clone/copy transactions where later mutation would change
  recorded expectations or observations; follow the project's ownership model.
- Completion: distinguish stimulus finished, DUT drained, checker queues empty,
  and timeout. Ensure objections and forked activity have a deliberate lifecycle.
- Coverage: sample meaningful observed/accepted events; map bins/crosses to
  requirements. Justify exclusions and avoid sampling only randomized intent.

Choose the smallest appropriate mechanism. Not every check requires a UVM class
or a full reference model. A targeted invariant can be the right oracle when it
fully expresses the requirement being checked.

## Validation ladder

Use available project facilities, in proportion to the change:

1. Static inspection: syntax risks, widths/types, reset behavior, hookups and
   configuration/factory selection.
2. Compile/elaborate the affected configuration; resolve first causal diagnostics.
3. Focused positive test for expected behavior and relevant boundary/reset cases.
4. Where practical and authorized, a controlled negative test or reversible local
   fault injection proving the new checker detects the intended error. Do not
   inject faults into shared production runs or retain accidental mutations.
5. Relevant regression for interactions and affected configurations.

Use a trusted flow or explicit success criteria, not absence of console errors.
If a stage cannot run, report exactly what is unvalidated. Include test/config,
seed, result, and artifact locations for executed tests. Never silently weaken
the checker, tolerance, timeout, or traffic mix to eliminate a failure.

## Coding guidance: shared checking and scenario-level exceptions

Most routine functional checking
should reside in the common reference model and scoreboard: derive expectations
from accepted inputs, applicable configuration, and task state, and compare actual
outputs and completion. Interface monitors/checkers may handle local protocol
conditions. Applicability follows the active mode, enables, and state, not the
testcase name. A feature check must run when that feature is exercised by any
applicable directed or random test, not only by its dedicated feature test.

Tests primarily choose and produce stimulus and coordinate the scenario. A
focused test makes a condition easier to reach; it does not need a private copy
of the normal functional checker. For example, the common early-release check
applies both when a test deliberately delays the last participant and when that
ordering arises in a broad random run.

Do not force every check into the scoreboard when scenario-level organization is
clearer. An error-injection test may inject a fault, wait for an interrupt, read
the interrupt information registers, compare the specified error type and source,
and perform the specified clear/recovery operations. Waiting for an interrupt
alone is only a progress check. Where reused, factor the register inspection and
comparison into a shared checking helper; do not require a framework for a simple
one-off check.

Keep the responsibilities explicit. Test-local interrupt/diagnostic checks do not
replace checking the fault's specified effects on data, synchronization, release,
or task state. Update the affected task's expected behavior when the specification
defines it, while continuing applicable checks on unaffected tasks. Do not disable
the entire scoreboard to accommodate error injection, and do not invent recovery
semantics for unresolved cases.

Random tests must retain correctness checks, not merely traffic generation and a
watchdog. When completion is required, check each relevant task rather than using
global activity as evidence that all tasks progress. A recovery completion must
not masquerade as ordinary successful completion in a normal-operation test.

The required observation granularity remains a project scope decision. For
example, checking externally visible completion under contention does not prove
an exact RR arbitration order. Record that distinction if chosen; do not turn one
project's decision to omit exact arbitration-order prediction into a general rule
or remove the design requirement from the feature list.
