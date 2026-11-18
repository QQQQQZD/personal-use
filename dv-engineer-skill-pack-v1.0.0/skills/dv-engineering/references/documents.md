# Produce Technical Documents

## Choose the document's authority

Before writing, distinguish these deliverables from context; ask if the distinction
would materially change the requested result:

- Design specification: intended externally observable requirements. Label new
  behavior as proposed until approved; do not invent normative requirements.
- As-built design description: behavior reconstructed from a specific code/config
  baseline. Explicitly state that implementation evidence is not independent
  confirmation of design intent.
- Verification specification/test plan: how requirements will be stimulated,
  predicted, observed, checked, covered and closed.
- Review/debug report: evidence, conclusions, limitations and recommended actions.

Use the user's template when supplied. Otherwise select relevant sections below;
do not manufacture irrelevant sections to fill a template.

## Design specification or as-built description

Include purpose/scope, source baseline and authority, block context, interfaces,
parameters/configuration, transaction semantics, state/ordering/timing behavior,
register behavior if applicable, reset/recovery, errors, assumptions, and open
issues. Specify units, bit/byte ordering, signedness, valid conditions, and
acceptance/completion meanings where important.

Use stable requirement IDs and source links. Keep separate columns or annotations
for specified, code-observed, inferred, proposed, and unresolved statements.
For a code-derived document, trace consequential behavior through actual logic,
not comments alone. Do not infer an architectural guarantee from one simulation.

## Feature lists and verification plans

For feature lists, testpoints, testlists, and verification specifications, read
[source-policy.md](source-policy.md), [feature-list.md](feature-list.md), and
[planning.md](planning.md). Apply [document-review.md](document-review.md) for review.
Do not reconstruct the planning procedure from a generic document template.

## Engineering reports

Identify the question, source/code/configuration baseline, scope, evidence actually
reviewed, findings, validation performed, unresolved issues, and next decisions.
Trace findings to features, testpoints, scenarios, checks, and runs as applicable.
Keep planned, implemented, executed, observed, and checked-successfully distinct.
Use [coverage.md](coverage.md) for coverage closure claims and report sampled
coverage separately from scenario/check evidence. Explain exclusions and waivers.
Prioritize substantive detection gaps over optional style remarks.

## Final consistency check

Check source revisions, internal cross-references, IDs, units, timing and register
definitions for consistency. Ensure each conclusion has evidence and each unknown
remains visible. Verify generated tables do not imply unperformed runs or pretend
that planned coverage already exists. Deliver an editable artifact with concise
limitations and owner decisions needed, rather than an unsupported signoff claim.
