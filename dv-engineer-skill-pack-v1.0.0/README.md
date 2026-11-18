# DV Engineer Skill Pack

Version 1.0.0 — 2026-10-23

Local update 2026-11-06: new-IP-environment design now requires an information
sufficiency check, test/task orchestration, and an accepted framework diagram
before implementation. The entry point routes to the new reference.

Share this entire directory. The reusable skill is `skills/dv-engineering/`;
its entry point is [SKILL.md](skills/dv-engineering/SKILL.md).
The optional [agent charter](DV_ENGINEER_AGENT.md) sets the DV role and routing.

## Use

Put the complete `dv-engineering` folder in the target harness's discoverable
skills directory and include the charter in the agent/preset instructions as
supported by that harness. Keep `references/` with `SKILL.md`. Package loading
configuration is host-specific; this is a portable Markdown skill, not a DSH
plugin or executable installer.

For an explicit-path experiment, instruct the agent to read SKILL.md fully and
load the required references before starting. Record this release version and the
project spec/output baselines. No original draft or author-specific absolute
path is required. Share without project data or credentials.

## Included

Spec-only feature extraction; explicit layer-first/function-second organization;
testpoint aggregation and testlist scenarios; random-testing guidance; independent
document review; coding fundamentals and document feedback; functional coverage
and evidence closure; TB review; simulation debug; technical reports.

Planning and review incorporate the current agreed draft, including later random
and coverage additions. Coding/debug/report guidance provides the methods currently
written; it is not an exhaustive UVM or protocol handbook. No model weights are
trained by installing this package. Use real tasks to evaluate and refine behavior.

The source drafts and existing kit are preserved separately. This release has
structural/frontmatter and local-link validation; it has not been certified by a
full simulator or all supported harnesses.

## Review feedback

Attach the task, input/output baseline, loaded reference names, concrete expected
versus observed behavior, and evidence for each proposed improvement. Distinguish
source ambiguity from skill omission and execution failure. Preserve the source
boundary and first-edition isolation when reproducing planning issues.
