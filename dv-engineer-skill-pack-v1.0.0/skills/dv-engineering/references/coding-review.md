# Coding Review and Document Feedback

Apply this during coding/component review, after first-edition planning.
Read [source-policy.md](source-policy.md) for provenance/issue handling and
[coding.md](coding.md) for implementation requirements.

## Coding feedback: feature-list adequacy for implementation

During later coding work, review whether the relevant feature definitions support
the actual component being implemented. This supplements source-based document
review and covers only the current coding scope; successful coding cannot establish
that the whole feature list has no omissions.

Follow testlist -> testpoints -> features and consult the authoritative spec as
needed. DUT behavior and expected-result rules used for implementation must be
represented or explicitly referenced in the feature list. Do not silently retrieve
an omitted rule from the spec, implement it, and leave the document gap unreported.
Standalone component work can start from its applicable features rather than
inventing an unrelated testlist entry.

Examples of required behavior knowledge:

- Driver: acceptance, holding, timing, and legal cancellation rules.
- Downstream responder: legal response delay, backpressure, ordering, and errors.
- Predictor/checker: data mapping, state-dependent effects, completion/cancellation,
  and the independent expected-result basis.
- Sequence: configuration prerequisites, task flow, and permitted event combinations.

Classify discovered issues before proposing changes:

| Observation | Response |
| --- | --- |
| Spec defines a rule but feature list omits or misstates it | Propose a source-linked document correction; record affected implementation/checks. |
| Feature contains the rule but references/grouping obscure it | Correct the association rather than duplicating the definition. |
| Spec is missing or contradictory | Use gray/red issues and do not invent the expected behavior. |
| Choice concerns UVM classes, queues, types, constraints, or helper structure | Make an appropriate coding decision; do not manufacture a design feature. |

Do not demand one independent code block per feature: a checker can enforce
multiple features and a feature can depend on multiple components. Code can
provide implementation evidence, not redefine the design contract. Inspecting
existing TB belongs to this later review stage and must not contaminate the
main-spec-only first-edition author. Keep RTL observations separately sourced
under the established integration rules.
