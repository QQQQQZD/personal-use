---
name: dv-engineering
description: Build and review hardware feature lists, testpoints, testlists and verification plans from specifications; develop and review SystemVerilog/UVM testbenches; plan coverage, debug simulation failures, and write evidence-linked engineering reports for IP and SoC verification.
---

# DV Engineering

Release: 1.0.0 (2026-10-23).

Local update: 2026-11-06 — new-IP-environment architecture and review rules.

Read the references required by the task completely before acting. All paths below
are relative to this skill. This package includes the agreed planning methodology;
no draft directory or external skill is needed for these workflows.

## Task routing

| Task | Required references |
| --- | --- |
| Read a spec to produce a module overview / feature list | [Source policy](references/source-policy.md), [Feature list](references/feature-list.md) |
| Produce testpoints, testlist, or a verification plan | Source policy, Feature list, [Planning](references/planning.md), [Coverage](references/coverage.md) |
| Review feature lists / testpoints / testlists / planning completeness | [Document review](references/document-review.md) and its linked applicable contracts |
| Understand RTL/TB or review existing verification adequacy | [TB review](references/tb-review.md) |
| Create a new IP/module verification environment | [New environment](references/new-environment.md), Coding, Coding review and feedback; obtain framework acceptance before implementation |
| Write or modify sequence, driver, monitor, predictor, scoreboard | [Coding](references/coding.md), [Coding review and feedback](references/coding-review.md) |
| Plan, implement, or assess functional coverage and closure | [Coverage](references/coverage.md); Coding when editing code |
| Diagnose setup, compile, elaboration, simulation, mismatch, hang, regression | [Debug](references/debug.md) |
| Write a design description, verification spec, or engineering report | [Documents](references/documents.md) and its task-specific routes |

## Source and execution boundaries

For first-edition planning, main-spec-only source policy takes precedence over
generic code-reading workflows. Do not read RTL/TB merely because this is a DV
task. Use clean context when prior implementation knowledge would contaminate
the first-edition author. External definitions and RTL-derived proposals follow
the staged integration policy. A deliberately requested as-built description
is a different deliverable and must be labeled accordingly.

Coding reviews assess implementation and document adequacy after planning.
DUT behavior comes from confirmed specification sources; code is evidence of
implementation. Follow testlist -> testpoints -> features rather than coding
from one file or a case title.

Distinguish specified, inferred, proposed, implemented, unresolved, planned,
executed, and checked results. Cite relevant sources and baselines. Do not invent
DUT guarantees, project commands, tool availability, tests, or signoff evidence.
Use actual project flow/tool instructions for execution and initialization.
This skill contains no connection-management or simulator command recipes.

Scope work to the requested deliverable and IP/SoC path. A review/diagnosis request
does not by itself authorize implementation changes. Preserve unrelated work.
Missing definition/check evidence should remain visible rather than be guessed.

Use an available specialist methodology for formal, CDC/RDC, analog, or other
specialized signoff tasks; this package does not establish those signoff results.
Report what was actually reviewed, changed, and validated and what remains open.
