# Functional Coverage

Read [source-policy.md](source-policy.md) for planning-source restrictions and
[planning.md](planning.md) for Feature/TP/testlist associations.

## Functional coverage: objectives, collection, and closure

Coverage planning identifies which specified configurations, behaviors, and tasks
need evidence. A documented coverage objective does not necessarily require a
covergroup, cover property, or an independently sampled runtime event. Select the
evidence mechanism for each objective instead of requiring one coverage bin per
feature. Coverage records occurrence; reference models and checkers establish
correctness under the applicable specification.

### Scope and useful combinations

Organize objectives by configuration/interface contracts, behavioral features,
and tasks/flows, with functional groups within each layer. Consider applicable
mode/format selections, legal boundaries and branch conditions, interface/resource
events, and relationships between participating paths in an end-to-end task.
These are planning views, not three mandatory independent covergroups or duplicate
coverage models for the same requirement.

Choose crosses for a distinct verification purpose, such as mode/format with a
relevant tail boundary, or a resource-exhaustion condition with recovery. Do not
enumerate every parameter/event Cartesian product. State applicability, legal
domains, exclusions, and unresolved definitions; unknown legality is not grounds
to silently classify combinations as illegal or ignore them.

### Choose the evidence mechanism per objective

| Mechanism | What it establishes | Closure evidence |
| --- | --- | --- |
| Sampled functional coverage | A defined configuration, event, transition, or combination was observed at a specified sampling point. | Applicable bins hit, with the sampling semantics and source identified. |
| Scenario and checking evidence, without dedicated runtime coverage | The relevant scenario executed and the required behavior was checked successfully. | A matching execution record, actual selected conditions, enabled and exercised reference-model/checker paths, task completion where required, and case PASS. |

Both mechanisms may support one objective. Avoid redundant behavioral sampling
when accepted-command/configuration sampling already identifies the scenario and
an adequate reference model plus scoreboard checks its execution. Conversely,
configuration coverage alone cannot establish that a temporal interaction such
as actual resource exhaustion occurred; use observable event evidence or a
scenario whose execution records establish that condition.

For example, a Gather command and its relevant index selection can establish
which discrete-row scenario was selected, while a reference model and scoreboard
compare the resulting addresses/data. Keep "Gather reads the indexed rows
correctly" as a documented coverage objective, but it need not have a separate
runtime coverage collector reconstructing address discontinuities. Close it with
the applicable selection evidence and checked case PASS. Noncontiguous addresses
alone do not identify Gather: other address-generation behaviors may also produce
them. Additional index sampling is needed only if the objective distinguishes
index properties that the existing command/scenario evidence does not establish.

A case name or generic PASS is insufficient when the case can select multiple
modes or bypass the relevant path. Retain enough actual configuration/event and
run evidence to show that this objective was exercised, its checks were active
and applicable, and no skip or vacuous completion replaced the intended work.
Do not count objectives closed by scenario evidence as hit covergroup bins or
blend them into a simulator's covergroup percentage. Report them separately.

### Collection location and sampling semantics

Choose the observation point according to the information and event needed:

- Accepted-command monitoring: actual accepted modes, formats, sizes, enables,
  and applicable shared configuration. Generated stimulus distributions may be
  useful diagnostics but do not prove command acceptance or execution.
- Interface monitors: actual requests, transfers, releases, stalls, and responses
  under that interface's defined contract. Do not infer an acceptance condition,
  holding rule, or ready/valid behavior from signal names alone.
- Stateful collectors: only when required for cross-interface or cross-time
  objectives, using associated transactions and confirmed state-update rules.
  An externally reconstructed credit value is a reference state, not evidence
  that the RTL's internal counter took that value.
- Task association/completion or checking records: selected path participation,
  end-to-end scenarios, and scenario-based closure. Reuse existing association
  information rather than duplicating the scoreboard without a distinct purpose.

Internal observation may be appropriate when an objective cannot be distinguished
externally and internal evidence is actually required; it is not the default for
every behavioral feature. Keep occurrence coverage independent from checking
results. If reporting "completed and checked successfully" coverage, label it
separately rather than suppressing occurrence evidence on a failing comparison.

For each objective record its Feature/TP/scenario associations, applicability,
selected mechanism, sampling event or execution evidence, checking basis, and
closure rule. Mark unresolved sampling or completion contracts as pending. When
reviewing holes, distinguish stimulus not reached, sampling/association defects,
unreachable or inapplicable conditions, and missing specification definitions.
Neither bin hits nor case counts alone establish verification completeness.
