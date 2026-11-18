# Build the Feature List

Read [source-policy.md](source-policy.md) before source inspection.

## Purpose and organization

Organize the output by abstraction layer FIRST, then by the DUT's principal
functions WITHIN each layer. Use it to identify functional scope and connect
related contracts, behaviors, and end-to-end tasks. It is not merely a list of
test names. Do not invert this into function-first/layer-second organization.

Begin with a short qualitative module overview: its architectural role, what
triggers its work, the principal operation, and its outcome. Do not include
detailed configuration in this overview. For example, a read-task module may
accept a command, read the required data, and report task completion, if supported
by the main spec. Describe multiple principal functions separately when useful.

The output MUST visibly separate the following layers in the listed order; using
them only as internal analysis categories is insufficient. These are presentation
levels, not strict parent-child feature dependencies. Do not force identical
functional subgroups into every layer or invent content for inapplicable layers.

| Level | Content |
| --- | --- |
| Configuration / interface contracts | Configuration fields and legal domains, applicability and effective timing; interface handshakes, timing, stability, and obligations of both the DUT and its peers. |
| Behavioral features | Required DUT behavior in a particular state or after accepting a command, signal, or event, including conditional effects and required non-effects. |
| Tasks / behavioral flows | The task the DUT must accomplish within the overall architecture, and the end-to-end relationship between the constituent behaviors. |

Use layer sections with function subsections, or tables visibly grouped/sorted
by Layer -> Function. Do not use an unlabeled mixed list or an ungrouped table
whose only distinction is a layer column. Shared items belong in a common group
within their relevant layer and are referenced by stable IDs across functions
and layers. For example, each layer may have Common, Load, Store, or Copy groups
as applicable; these names are illustrative, not a universal template.

Keep concrete definitions, source links, unresolved markers, and cross-feature
references intact when reorganizing. Check both detailed and concise review
outputs for explicit layers, functional grouping within layers, and shared-rule
deduplication. This presentation requirement creates no priority ranking or RTL
mapping requirement and does not relax first-edition source isolation.

The distinction between a behavioral feature and a flow is the verification
viewpoint, not merely complexity: a feature asks whether a particular behavior
is correct under its conditions; a flow asks whether connected behaviors actually
achieve the required overall outcome. Locally legal operations alone do not prove
that an end-to-end task is correct.

Requirement extraction is the work of building the feature list; do not require
a duplicate requirement list or a second requirement-ID hierarchy. Behavior
analysis is an internal method for understanding related features, not a mandatory
separate phase or behavior-model document. Put meaningful relationships in the
appropriate behavioral/flow entries. If already clear, do not expand them again.
Use diagrams or relationship tables only when they materially clarify the behavior.

### Finding principal functions and modes

Identify principal functions from the DUT's responsibility, trigger, data/control
destinations, and externally observable outcome. Use architecture descriptions,
mode summaries, detailed flows, and configuration scenarios together; do not
equate chapter headings or individual fields with independent functions.

For each principal function, identify conditions that change its behavior and
connect the mode to its selection condition, affected paths, outcome, and local
exceptions. Factor genuinely shared contracts into references. A mode may affect
several paths; listing those paths independently does not preserve the mode's
overall task unless their relationship is visible.

Reconcile terminology across sections using stated behavior and context. Record
source names when they help readers recognize the function. Similar names alone
do not establish equivalence, and application background alone does not establish
a DUT capability. Preserve the DUT's specified contribution to an application
without assigning the surrounding algorithm to it.

After organizing by function, reverse-check substantive source sections, tables,
and figures for responsibilities, mode conditions, legal domains, and special
behaviors that lack a semantic home. Check relationships as well as individual
clauses. Distinguish an omitted behavior, a relationship lost during organization,
an existing shared definition, and a source gap needing a concrete question.
Do not use entry counts or link presence as proof of successful extraction.

This analysis need not become a duplicate requirement hierarchy or a separate
mandatory document. Put useful mode relationships into the feature entries or
a compact shared table; retain internal extraction notes only when they aid review.

### Review versions and summary preservation

When a concise review version is requested, preserve principal functions, mode
relationships, behavior-selecting conditions, known legal limits, and unresolved
decisions needed for that review. Detailed formulas and beat mappings may be
linked. Use a compact mode or combination table when prose becomes long; do not
replace the reviewable support set with only its count.

Evaluate brevity against the review decision, not a character limit. A shortened
entry must still let a reviewer identify what behavior applies under which
conditions. Explicitly identify material content that requires opening the linked
detail. During review, distinguish detailed extraction omissions, summary losses,
and insufficiently exposed source gaps instead of reporting all as missing features.

### Granularity and concrete definitions

Organize by coherent behavior rather than spec sentence count or individual
parameter values. Preserve conditions and relationships when splitting entries.
The feature list MUST retain applicable spec-defined values, ranges, units,
field definitions, timing limits, and special cases; it is not an abstract summary
that leaves all concrete definitions to another document. Shared definitions
may be referenced rather than copied.

For example, a legal length range of 16-512 bytes belongs in a feature. Choosing
the full range, 256-512 bytes, or exactly 512 bytes for a verification scenario
belongs in a testlist entry. These are examples, not universal DMA requirements.

Follow functions and verification needs to encounter relevant register fields
naturally. Do not independently traverse the register table to manufacture feature
entries or require a list of all/core registers. A requested register-specific
verification task can have a different scope.

Do not inflate the feature list or testplan with obvious semantic restatements.
Retain an original rule such as "exactly one response per accepted request";
do not create redundant features/cases merely saying "no loss" and "no duplicate."
Their checking belongs in reference models/checkers/interface agents. A later
code review should flag missing enforcement. Explicit exceptions or distinct
scenarios with a new verification purpose still require appropriate treatment.

### Traceability

Give concrete features stable IDs and spec section/table/figure references,
adding page numbers where useful. List all relevant locations when a feature
combines multiple clauses. Keep a shared definition's source at its canonical
entry and reference that entry elsewhere. Do not copy large source passages
unnecessarily or force the feature list into spec chapter order.

Feature entries have no verification priority. They describe the DUT, not a work
schedule. Do not attach priority rankings or AI risk-based importance to features.

## Shared definitions and references

- Define a shared contract or behavior once, with a stable identifier. Reference
  it explicitly from each applicable function or flow rather than copying it.
- A flow may reference several interfaces, configurations, and behaviors; one
  shared behavior may support several functions. Preserve these many-to-many
  relationships instead of forcing exclusive ownership in a tree.
- For a function-specific variation, state the applicability and the difference
  from the shared definition. Avoid duplicating unchanged requirements or leaving
  the precedence of a local exception ambiguous.
- Similar names do not establish equivalent semantics. Do not merge two
  ready/valid interfaces if their holding, cancellation, timing, or other contract
  rules differ. Factor out only the genuinely common part when useful.
- Deduplicating feature definitions does not deduplicate verification contexts.
  Different implementation paths or interactions may require separate tests of
  the same shared behavior. Conversely, references alone do not mandate duplicate
  tests; decide this during testpoint decomposition.

## Bidirectional interface contracts

For an interface, distinguish:

1. Peer obligations: upstream/downstream behavior that defines the TB's legal
   driving and response space.
2. DUT obligations: behavior the DUT must guarantee under the applicable premises,
   providing the basis for DUT checks.
3. Dependencies and unknowns: conditions under which guarantees hold, and missing
   or ambiguous rules that must not be filled in by assumption.

Do not infer a protocol solely from signal names. For example, request/payload
holding until acceptance is a peer obligation only when the applicable contract
requires it. A maximum downstream ready-response delay must come from the
contract; absence of a specified bound does not authorize inventing one.

Keep protocol constraints, deliberately narrower test settings, and intentional
protocol violations distinct. A simulation watchdog is not automatically a DUT
latency requirement. DUT behavior under illegal stimulus has a pass/fail basis
only to the extent that the applicable specification defines it.

## Missing definitions, conflicts, and review annotations

Use DV experience to identify necessary unanswered questions, not to supply their
answers. Check applicable trigger/acceptance, result/completion, in-flight behavior,
legal conditions, and relevant interruption/recovery semantics. Do not impose
irrelevant questions on a simple module or ask for behavior the spec excludes.

Treat text, tables, diagrams, and explicit requirements together. Apply precedence
only when a documented priority or supersession rule exists. Otherwise preserve
conflicting statements for design review. Distinguish complementary detail from
a genuine contradiction; an illustrative timing diagram does not automatically
prohibit event timings that it simply does not depict.

Mark issues at the affected location, not solely in an end-of-document list:

| Presentation | Meaning |
| --- | --- |
| Gray block: Pending definition | Missing/ambiguous definition or external detail awaiting incorporation. |
| Red block: Definition conflict | Incompatible requirements with no established resolution. |
| Red block: Affected by Q-xxx | Another entry cannot be determined because of a linked missing definition or conflict; this does not assert a separate local contradiction. |
| Yellow block: Highlight | Reserved for important items; it is not the missing-definition color. Testlist AI suggestions and human-confirmed highlights must be distinguished. Feature highlights, if explicitly requested, do not establish a priority ranking. |

In formats supporting styles, use the specified background blocks. Plain Markdown
must use labeled blockquotes if colors cannot be reliably rendered. Always include
explicit status and IDs; never encode meaning by color alone.

Each issue block contains an issue ID, exact source location(s), the missing or
conflicting definition, a concrete question for the designer, and verification
impact. Include both sides for conflicts. A dependent entry references the source
issue and states its own impact rather than duplicating the whole issue.

Continue unaffected work. Leave affected behavior and expected results unresolved
instead of choosing an answer or abandoning the whole document. An optional
consolidated issue index supplements, not replaces, local marking. After review,
record the confirmed answer and its source, update related entries, and retain
the issue's resolution history.
