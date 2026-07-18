# AGENTS.md

Orientation for agents working in this skills repository. Read this before editing skills or using
the workflow rules here.

## Source Of Truth Discipline

Each workflow artifact owns one kind of truth. Do not duplicate details across artifacts.

- Requirements own why, scope, constraints, assumptions, and work-item decisions.
- Scenarios own concrete behaviour and scenario-local acceptance criteria.
- Architecture owns solution shape, ownership, seams, flows, and design rationale.
- Models own exact shapes: schemas, contracts, interfaces, state models, events, props, ports, and
  invariants.
- Plans own execution order, files, commands, verification, and commits.

If a detail would need to be updated in two places when it changes, move it to the artifact that
owns it and reference it elsewhere.

## Progressive Disclosure Contract

Use the smallest context set that can safely answer the current question or perform the current
workflow step.

Checkpoint and status files are resume/index artifacts. They may identify likely state, next
action, and relevant paths, but they do not authorize assumptions about requirements,
architecture, plans, models, code behavior, review findings, or functional-test evidence.

Before acting on a claim, load the artifact that owns that claim:

- Requirements, scope, constraints, assumptions, and work-item decisions: `requirements.md` or
  task `brief.md`.
- Concrete behavior and acceptance: scenario files.
- Solution shape, ownership, seams, flows, and design rationale: `architecture.md`.
- Exact schemas, contracts, interfaces, state models, events, props, ports, and invariants:
  `models/*`.
- Execution order, files, commands, verification, and commits: `plan.md`, `plans/index.md`, or
  `plans/phase-N.md`.
- Code state: source files plus git diff/status.
- Review findings: review artifact.
- Functional evidence: functional-test cases and run report.

If the owning artifact has not been read, treat the claim as unknown, not true. If
checkpoint/status conflicts with the owning artifact, source code, or git state, surface the drift
and prefer the owning artifact/code/git state.

## Technical Guidance Discipline

Workflow skills should define process and artifact boundaries. Detailed technical guidance belongs
in focused project rules, skills, references, ADRs, source docs, or official docs.

When a workflow step needs API, database, interface, state, security, performance, migration, or
framework guidance, load the relevant source instead of encoding generic rules in the workflow
skill.

## References

- Skill-local `references/` directories contain workflow-specific templates and adapted contracts.
- Generic technical guidance comes from source-qualified skill references named at point of use,
  using `owner/repo@skill`. Load an installed skill when available; otherwise retrieve it for the
  current task with `npx skills use <owner/repo@skill>`. Do not copy upstream skill bodies into this
  repository.
- Third-party attribution and license notices for retained adaptations live in
  `references/ATTRIBUTIONS.md`.

## Skill Editing

- Keep skills concise and procedural.
- Put workflow-wide rules in this file.
- Put only point-of-use instructions inside individual skills.
- Do not add auxiliary docs unless they are directly needed by a skill.
