# Source Policy and Document Baselines

## Source boundary: first edition MUST be main-spec-only

The first edition MUST derive DUT requirements and expected behavior solely from
the designated main spec. Preserve its name and revision at document level.
Missing definitions stay visible as questions; completeness means covering the
specified information and exposing gaps, not inventing answers to every question.

General DV knowledge may help organize information, identify questions, and
derive verification scenarios. It MUST NOT introduce unsupported DUT behavior,
protocol guarantees, limits, or expected results.

### RTL isolation

The first-edition author MUST NOT consume RTL, RTL analysis summaries, or earlier
implementation-derived documents. Separating RTL statements in the final output
is insufficient if RTL has already influenced the author's interpretation.

Use a clean-context author/subagent with only the main spec, user instructions,
and writing methodology. Do not fork history containing RTL knowledge. If the
coordinator has read RTL, it must not inject that knowledge into the first edition.
Prefer a spec-only workspace/input allowlist where available. Prompt-only access
restrictions are not hard isolation; disclose limitations rather than claiming
enforced separation. If a clean context cannot be provided, obtain one before
claiming a strictly isolated first edition.

Save the first edition before subsequent RTL analysis. RTL may then be reviewed
as needed, even when the spec is sufficient, to produce a separate adjustment
proposal. Label every such observation as RTL-derived, with code baseline/location,
related feature, discrepancy or suggestion, and impact. Do not mix it with
spec-extracted statements. Merge only changes explicitly directed by the user,
retain source/confirmation records, and preserve the original first edition.
RTL observation alone does not establish a design requirement.

### External documents

In the first edition, record external document references present in the main
spec but do not automatically read and expand their contents. Keep main-spec
statements; mark details that require the external document as pending. Identify
the document, version/section if provided, and the content needing expansion.

After the first edition, read and integrate external content as directed by the
user. Confirm version and applicability, particularly for protocols such as AXI;
do not import an entire protocol or assume rules from memory. Supplementary
definitions, such as NPU DMA format-specific DDR layouts, may be expanded into
the current document instead of remaining only a link. Preserve provenance.

External normative specs and RTL share a staged integration process, not equal
authority: an applicable confirmed specification can define requirements; RTL
remains implementation evidence pending explicit design confirmation. Merely
referencing an external document does not make every referring feature unresolved.
