# Verification Planning and Testlist

Read [source-policy.md](source-policy.md) before inspection and
[feature-list.md](feature-list.md) when creating or revising features.
For evidence collection/closure, read [coverage.md](coverage.md).

## Workflow and terminology

Read the main spec -> write a qualitative module overview and feature list ->
aggregate features into testpoints -> compose testpoints into testlist entries.

- Feature list: the DUT's functions, behavior, and contracts, including concrete
  configuration/interface rules and complete application flows where specified.
- Testpoint: reusable verification intent aggregating relevant features,
  applicable configuration rules, stimulus/process requirements, and checks.
- Testlist entry: a concrete verification scenario combining relevant testpoints
  and selecting its parameter space and event conditions.

These are many-to-many mappings, not a fixed parent-child tree. Do not enforce
one feature per testpoint, one testpoint per entry, or one entry per executable
testcase. A testlist need not correspond to a separate physical file. Testplan
may refer to the overall verification-planning document rather than a fourth
mandatory artifact.

## Testpoints: aggregate features into verification intent

A testpoint combines relevant features for a meaningful verification purpose.
For example, a load/store transfer testpoint may aggregate command behavior,
length/address configuration, data mapping, and result/completion checks. A
different copy flow may need different testpoints and testlist organization.
Do not treat a testpoint as a renamed single feature or as a mandatory testcase.

Describe:

- Verification purpose and referenced feature IDs.
- Applicable configuration rules, boundaries, and special cases from those features.
- The necessary reusable stimulus/process intent.
- What is checked, the observable result, the expected-behavior basis, and the
  pass/fail meaning. Reference shared checks rather than repeating obvious rules.

Identify shared checking responsibilities rather than assigning each feature's
check exclusively to its named testcase. Most routine functional expectations
belong in the common reference model and scoreboard and apply to every relevant
test. Scenario-level exceptions are allowed as described in [coding.md](coding.md); planning need not prescribe
their code structure.

Do not duplicate detailed layout, format, or interface rules already defined in
features. "Check data" may be sufficient only when the references unambiguously
establish which data/result and mapping apply. Missing local repetition is not
missing information; unresolved source definitions or broken references are.

Do not specify variable data types, code-level constraints, UVM class/task names,
scoreboard queue structures, or matching implementation here. Those belong to
coding. Preserve verification intent without prematurely designing the code.

## Testlist: concrete scenarios and parameter spaces

Compose testpoints into scenario entries, retaining stable entry IDs and testpoint
references. Select fixed conditions, parameter spaces, applicable combinations,
and event relationships according to the scenario. Keep spec legality distinct
from deliberately narrower verification choices.

Illustrative example, assuming the features define the stated legality:

| Entry | Referenced verification intent | Scenario selection |
| --- | --- | --- |
| General load transfer | Load transfer correctness testpoint | Length 16-512 bytes; applicable legal address rules. |
| Large load transfer | Same testpoint | Length 256-512 bytes, explicitly a test scenario choice. |
| Maximum-length load | Same testpoint | Length fixed at 512 bytes. |

Do not require separate testpoints for these length choices. The coding agent
implements the selected space through suitable data types, constraints, and
sequences. The plan must state the intended random space and targeted conditions;
the particular implementation of that randomization belongs to coding.

Backpressure, concurrency, reset, and error/recovery also belong in scenario
composition when applicable. If a condition leaves the expected behavior unchanged,
reuse the appropriate checks. If it changes the expected behavior, such as reset
cancelling a transfer, associate the relevant cancellation/reset testpoints and
checks; do not merely add a "reset" label while expecting ordinary completion.
Do not invent concurrency, queuing, or recovery guarantees absent from the spec.

### Broad normal random testing and focused random suites

For module verification, include a substantial normal-operation random case or
suite as a primary regression entry, not only separate directed cases per feature.
Jointly vary compatible normal modes, participant counts and identities, roles,
data/configuration choices, ordering, and legal delays as applicable. A collection
of isolated feature tests with a random value inside each does not replace this
combined normal space. Describe the actual space and meaningful interactions;
calling a case "random" or increasing its transaction count is insufficient.

Preserve dependencies between choices. For example, a synchronization task may
select its participants and then choose an aggregation point from those
participants; independently randomizing each participant's role could create an
inconsistent task. Honor configuration effective timing: values that may change
only during initialization vary across initialized runs, not arbitrarily between
transactions. Keep unknown legality or completion behavior visible as pending,
with the implemented subset and remaining target space distinguished.

Software-assisted operation, error injection, CRC testing, or other substantially
different setup/expectation families may have separate focused random cases when
applicable. They still reuse the common checks. Do not impose these categories on
designs without the corresponding functions or silently claim that one family
exercises another family's specialized conditions.

Directed cases remain useful for explicit feature stimulation, difficult events,
debugging, and reproducibility. Generating them cheaply is not a reason to omit
the broad normal random case. Ordinary parameter variation can be covered there;
it need not be enumerated again in every directed entry. A distinct boundary or
resource event may still justify a focused case.

### Random variation within directed scenarios

A directed scenario fixes the condition needed to exercise its purpose, not
necessarily all its parameters. Where practical, randomize the remaining legal
participants, sizes, identities, data, order, and delays while preserving the
target condition. Fully fixed settings are appropriate when they materially help
smoke testing, boundary reproduction, or debugging; they are not the default for
every focused scenario.

For example: "Leave one core to report later; check that no core is released
before it reports, then complete synchronization normally." The late core,
participant count, main-core selection, barrier ID, other arrival order, and
legal delays can vary. Preserve the normal-completion premise rather than
accidentally triggering a timeout. This example does not create a requirement
to test every participant count in that directed case.

### Readable scenario descriptions

State the distinguishing action and expected result in plain language. Put shared
legal ranges and ordinary random dimensions in a referenced parameter-space
definition instead of compressing every value into each scenario sentence.
Retain an exact value locally when that value is the point of the test. Avoid
symbol strings that require the reviewer to reconstruct the intended sequence.

Random-space inclusion alone is not execution evidence. Apply the [functional coverage closure rules](coverage.md) to actual selected conditions, observed events,
exercised checks, and task results; adjust stimulus or use focused cases for
meaningful conditions that remain unreached.

### Cross-layer reading contract for coding

The coding agent MUST start from the selected testlist entry, read its referenced
testpoints, and follow the relevant feature references until the scenario, legal
space, behavior, and check basis are understood. Do not implement from only one
file or a short case title. Read the needed dependency set, not necessarily every
document in full. Report broken links, unresolved source entries, or contradictions;
do not guess information that was omitted locally to avoid duplication.

### Priority and emphasis

AI may propose testlist priorities and yellow highlights with brief reasons for
human review. Distinguish AI-proposed from human-confirmed status. Do not infer
human confirmation from a color. First-edition suggestions must not use RTL-derived
risk information. Features themselves have no priorities; reusable testpoints need
not inherit a fixed priority across different scenarios.

Priority can guide scheduling when appropriate, not implementation quality.
Every case the user requests must faithfully implement its scenario, stimulus,
and checking. Never weaken, stub, or skip a case because it has lower priority.

## Provisional completeness review and stopping criteria

Use bidirectional mapping review as an initial method, not a completeness proof:

- Forward: determine how each in-scope feature is addressed through testpoints
  and applicable testlist scenarios. Shared checking may carry common interface
  rules without a dedicated testcase; record the planned association.
- Backward: trace each testlist scenario's behavior/check assumptions to its
  testpoints and features. Do not introduce unsupported DUT requirements.
- Evaluate actual scenario applicability, not merely link presence. If a reusable
  testpoint supports load/store but every entry selects load, store remains a gap.
- Separate planned mapping from implemented checking and observed run evidence.
  Links alone never establish successful verification or measured coverage.

Add an entry when it brings a distinct verification purpose: an unaddressed
behavior/flow, a defined special or boundary condition, or a meaningful interaction.
Do not enumerate the Cartesian product of every parameter and event. Ordinary
value variation without a new purpose belongs in an existing scenario's space.

Stop expanding when in-scope features have applicable verification arrangements,
specified special cases and relevant interactions have been considered, and
unresolved items are visible. Further subdivisions that only repeat an existing
purpose are unnecessary. Keep potentially valuable but insufficiently supported
scenario suggestions separate for review instead of silently making them required
cases. This stopping rule bounds work; it does not guarantee absence of omissions.
