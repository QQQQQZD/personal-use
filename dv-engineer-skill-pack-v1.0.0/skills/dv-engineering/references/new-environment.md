# New IP Testbench: Architecture Before Coding

Agreed update: 2026-11-06. Apply when creating a new IP/module verification
environment. Read [coding-review.md](coding-review.md) for tracing behavior to
features and reporting source gaps. Implementation follows [coding.md](coding.md).

## Two input cases

1. Detailed verification material is available, but component architecture is not
   supplied. Read the relevant feature list, testpoints, testlist, and authoritative
   specification material together. Use their interface-driving contracts, behavior,
   check requirements, and planned scenarios to propose component responsibilities,
   component logic, connections, and information flow.
2. The user supplies a component plan, including responsibilities, logic, and
   check allocation. Use that plan as the architectural basis. Obtain unspecified
   stimulus and checking behavior from the associated feature list and supporting
   verification material. Report missing responsibilities or contradictions before
   proposing changes; do not silently replace the supplied architecture.

For either case, the absence of detail in one document is not itself a gap. Follow
its references first. If a combined document holds multiple kinds of information,
use the corresponding content without requiring separate physical files.

## Information sufficiency

Before implementation, judge whether the available information supports the
requested environment: interface driving and response contracts, relevant task
behavior/configuration, independent expected-result basis, observable results,
check responsibilities, and applicable completion/reset/cancellation semantics.
Read only the dependencies needed for the target environment. Do not invent
concurrency, cancellation, or recovery behavior that the specification does not define.

For a consequential gap, identify the source location or missing definition,
the specific question, and the affected architecture or check. Follow the existing
gray/red issue conventions. Continue useful unaffected analysis, but do not guess
the missing behavior to claim the environment is ready for coding.

Ordinary implementation choices, such as helper decomposition, queue types,
or local class structure, belong to the AI's design work. Do not turn every such
choice into a question for the user. A plan is ready for review when its behavior
and checking basis are sufficiently defined, not when every line of code is planned.

## Task organization and interface stimulus

Use the following default architecture:

- Test: top-level scenario and task planning. Tasks in the test organize task
  execution, configuration/data preparation, interface sequence invocation,
  concurrency, synchronization, event injection, and overall completion.
- Task-level object: aggregates the information needed for a task. Name it normally
  according to project semantics; no special naming convention is imposed here.
- Interface sequences/sequencers/drivers: implement the corresponding interface
  stimulus and transfers. Sequences communicate with drivers through sequencers.

Preserve task-level aggregation rather than making the entire stimulus model a
collection of tiny interface transfers. At the same time, test tasks should reuse
interface sequences and drivers rather than directly implement cycle-level pin
driving or duplicate their protocol logic.

Do not introduce a separate task-level sequence by default. Keep top-level planning
in the test; virtual sequences are not the default. If a specific need warrants a
virtual sequence, explain that need and include the alternative in architecture
review before coding. Do not introduce one merely to match a generic UVM template.

## Framework diagram presentation

Show component responsibilities and essential information flow rather than a
signal-level wiring diagram:

- Omit separate edges for routine operations whose access path is already clear,
  such as clearing an interrupt through the register-access agent. Expand a path
  only when it explains a meaningful architectural distinction or special behavior.
- Represent interface handshakes with one bidirectional edge or a pair of opposing
  edges, labeled by interface or purpose. Do not expand every request, release,
  and acknowledgement signal into its own edge. Put detailed signal timing in
  interface descriptions or timing diagrams when needed.

These are presentation rules, not changes to component responsibilities. Preserve
the essential input-observation, expected-result generation, and actual-result
comparison paths; keep stimulus intent distinguishable from observed execution.

## Required review deliverable and implementation gate

After resolving information gaps sufficiently for design, deliver:

1. A framework diagram showing test/task orchestration, task information, interface
   components, DUT connections, observation paths, expected-result generation,
   and comparison/check paths as applicable. Label control/stimulus and observed/
   expected data flow clearly enough to avoid confusing their sources.
2. A concise explanation or table of component functions, principal logic,
   inputs/outputs, and check ownership, referencing features where needed.

Scale the diagram to the module. Do not require irrelevant components or a large
architecture report. Avoid duplicate checking responsibilities and ensure that
necessary checks have owners; reuse shared functional checking as specified in
[coding.md](coding.md).

Submit this framework for user review and wait for explicit acceptance before
writing the new environment's implementation code, including scaffolding. A
general request to build an environment does not waive this agreed review gate.
If the user has already explicitly accepted the same current framework, proceed
without requesting acceptance again. Revise a materially changed framework for
review before implementing the changed architecture.
