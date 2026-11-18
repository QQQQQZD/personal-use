# Review Planning Documents

Read [source-policy.md](source-policy.md), [feature-list.md](feature-list.md),
and [planning.md](planning.md) for the candidate's applicable contract. Read
[coverage.md](coverage.md) when reviewing coverage/closure claims.

## Independent review of generated documents

Freeze the document version under review. Prefer a clean-context reviewer given
the raw authoritative input, the review methodology, and the candidate documents,
without the author's reasoning or favorable assessment. Read the source spec
before judging the candidate. Author self-checks remain useful but do not replace
independent review. Do not require a quota of findings or equate reviewer agreement
with proof of completeness. Produce evidence-linked findings before editing the
candidate; verify each finding and apply corrections within the user's scope.

### Feature-list fidelity and completeness

For first-edition review, use only the main spec and candidate feature list, with
the same RTL/external-content boundary as generation. Recheck the relevant source
content when later external definitions or confirmed adjustments are incorporated.

Work in both directions:

- Source -> feature: independently examine substantive passages, tables, and
  figures for requirements, peer/DUT obligations, conditions, modes, exceptions,
  ranges, and flow relationships. Identify omitted or weakened content.
- Feature -> source: confirm each requirement's basis and meaning. Check for
  invented restrictions, example-to-rule promotion, reversed obligations, and
  incorrect units, timing, legal domains, or applicability.
- Function/flow: check that the DUT's specified principal tasks and mode
  relationships survive organization; mapping isolated clauses is insufficient.
- Presentation: confirm layer-first/function-second organization, shared-rule
  consistency, source traceability, visible issues, and faithful concise summaries.

Maintain a project-specific source-disposition table as review evidence. This is
not a second requirement hierarchy and does not assign a new ID to every sentence.
Use source locations precise enough to show what was checked, including multiple
obligations within one passage when needed:

| Spec source and substantive content | Destination | Disposition and semantic review |
| --- | --- | --- |
| Section/passage/table/figure and relevant obligation(s) | One or more feature IDs | Captured accurately, partially captured, omitted, or unresolved; explain conditions/relationships checked. |
| Repeated source rule | Canonical shared feature ID | Verify equivalence and preserve local exceptions. |
| Missing/conflicting definition or unexpanded external reference | Issue ID and affected features | Record question, source boundary, and impact. |
| Background, illustrative material, or agreed out-of-scope content | Exclusion or context use | State why it does not define an in-scope DUT requirement. |

Construct this evidence from raw spec inspection, not by backfilling links from
the existing feature list alone. A destination link is not evidence of semantic
completeness: a passage's normal case may be captured while its mode exception
is omitted. Do not turn this source sweep into a requirement to create a feature
for every register field or document sentence.

Report completeness relative to a named spec baseline and agreed scope, with
reviewed source areas, unresolved items, exclusions, and review limits. Do not
claim absolute absence of omissions or completeness of unwritten design intent.

### Testpoint and testlist effectiveness

Read the spec, feature list, testpoints, and testlist jointly. Apply the provisional
mapping check in [planning.md](planning.md) and test whether the planned stimulus, observation, and
comparison can expose plausible violations of the applicable contract.

Use specific counterexamples as review probes: which scenario would trigger this
wrong behavior, which observation would reveal it, and which planned check would
distinguish it from legal behavior? Do not add the hypothetical bug as a new DUT
requirement or expand generic fault ideas into mandatory cases without a reason.

Inspect the actual selected spaces and event conditions. An entry referencing
length variation but selecting only one endpoint, an unspecified backpressure
phase, or reset using an incompatible normal-completion expectation can hide gaps
even with valid links. Check independence of planned expectations and distinguish
progress-only checks from functional checks. Referenced shared rules/checks are
valid; do not demand their verbatim repetition in every testpoint.

Also check that the plan includes combined normal random operation, retains
useful random variation in focused scenarios, and makes routine feature checks
available to all applicable tests. Verify that scenario-level checking exceptions
have a clear purpose and do not leave the common functional checks inactive.

This is a review of planned detection capability, not evidence that code implements
it or that a simulation has detected the fault. Separate confirmed planning gaps,
unresolved definitions, and optional scenario suggestions.

### Structural checks and finding closure

Where useful, automate duplicate-ID, broken-reference, document-baseline, and
issue-resolution consistency checks. Such checks support traceability; they do
not establish semantic correctness or coverage.

Each material finding identifies document/entry location, spec evidence, the
specific omission or misinterpretation, verification impact, and proposed action.
Use finding IDs for tracking. After revision, recheck the finding and affected
references/scenarios. Keep design questions in the agreed gray/red format until
resolved. Do not label every improvement suggestion as a confirmed defect.

Store review findings and the source-disposition table alongside the project's
reviewed documents, recording spec and candidate versions. Keep project review evidence with project artifacts rather than in this skill.
