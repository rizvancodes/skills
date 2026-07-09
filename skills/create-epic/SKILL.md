---
name: create-epic
description: |
  Create durable epic documentation from clarified requirements, prior conversation, an external
  ticket, or an approved requirements-gathering discussion. Use when the user says to create an
  epic, capability, roadmap item, or durable design artifact for larger work with multiple
  scenarios, delivery slices, owners, packages, services, apps, runtimes, or modules. Writes the
  epic workspace and core docs: requirements.md and scenarios with scenario-local acceptance
  criteria. Do NOT use for interviewing from scratch, creating compact standalone task briefs,
  implementation planning, architecture or solution design, coding, review, checkpointing-only work,
  or durable language promotion.
---

# Create Epic

Create the durable docs for a larger epic, capability, or roadmap item.

This skill is an artifact writer. It turns clarified requirements into files. It may ask a focused
question if a blocking gap remains, but it should not restart the requirements interview.

<HARD-GATE>
Do NOT write production code.
Do NOT write implementation plans, phase plans, execution tasks, or commit cadences.
Do NOT write architecture docs, solution designs, package boundaries, API designs, state ownership
models, data flows, or chosen technical shapes.
Do NOT create compact task briefs; that belongs to `create-task`.
Do NOT invent requirements, acceptance criteria, scenarios, IDs, owners, or constraints.
Do NOT invoke `plan-work`, `execute`, or another downstream skill after writing docs.
Do NOT overwrite an existing epic workspace unless the user explicitly asked to update it.
</HARD-GATE>

## Required Readiness

Create an epic only when the source material is clear enough to write durable docs.

Required minimum:

- the outcome the epic should achieve
- the primary actor, user, system, or stakeholder
- why this matters now
- scope boundaries and non-goals
- at least one concrete scenario
- acceptance direction, even if some criteria remain draft
- known constraints, assumptions, and unresolved questions

If the minimum is missing, ask one focused question or recommend `gather-requirements`. If one
technical unknown blocks the docs, recommend `spike`.

## Artifact Layout

Use the project-configured ID and epic-root convention when present. Read `AGENTS.md` and
`docs/work/config.md`, or the equivalent project-local file named by repo instructions. Keep
workflow artifact names fixed.

Default layout:

```text
docs/epics/<epic-slug>/
  requirements.md
  scenarios/
    <scenario-slug>.md
```

Use `docs/epics/` as the baseline root. If a project config defines a different epic root, follow
that only when the user confirms it for the project.

Create only meaningful scenario files. Do not create empty placeholder scenarios.

## Workflow

### 1. Load Source Material

Read only the context needed to write accurate docs:

1. Current conversation and any output from `gather-requirements`.
2. User-provided ticket, issue, PRD, note, transcript, or URL.
3. Project `AGENTS.md` and work config.
4. Related existing epic docs, task briefs, ADRs, `LANGUAGE.md`, `DOMAIN.md`, and nearby docs.
5. Code paths only when repository behavior affects the epic shape.

Do not flood context with every rule or existing epic. Load enough to avoid generic docs.

### 2. Content Boundary

Classify each detail before writing:

- If it defines the work item, scope, requirement, imposed constraint, assumption, or accepted
  tradeoff, put it in `requirements.md`.
- If it describes a concrete user, system, or domain situation, put it in a scenario.
- If it defines how that scenario will be accepted, put it in that scenario's acceptance criteria.
- If it defines architecture, solution shape, package boundaries, APIs, contracts, data flow, state
  ownership, or runtime ownership, leave it out for a later architecture or solution-design step.
- If it defines execution order, files, commands, tests, phases, or commits, leave it for
  `plan-work`.

### 3. Resolve Name, ID, And Path

Choose the epic title and slug from the user's language.

For work item IDs:

- Use a user-supplied local ID when it matches project convention.
- Use the project allocation source when config defines one.
- If no convention exists, use a slug-only path.
- Ask before inventing the first project ID prefix.
- Record external tracker IDs as references; do not replace the local ID with them.

Before writing, check whether the target path already exists. If it exists, ask whether to update it
unless the user already asked to update that exact epic.

### 4. Write Requirements

Path:

```text
docs/epics/<epic-slug>/requirements.md
```

Structure:

```md
# Requirements: [Epic Name]

**Work kind:** epic
**Status:** draft
**Epic root:** docs/epics/<epic-slug>
**Local ID:** [ID or "not assigned"]
**External references:** [tracker links/IDs or "none"]

## Intent
[What outcome this epic should create and why it matters.]

## Background
[The current situation, pain, opportunity, or context that explains why this work exists now.]

## Actors
- [Actor] - [need, responsibility, or stake]

## Scope
### In Scope
- [Included behavior or capability]

### Out Of Scope
- [Excluded behavior] - [reason]

## Requirements
- [Stable thing that must be true when this epic is done]

## Constraints
- [Imposed business, product, technical, regulatory, UX, timeline, platform, compatibility, or
  existing-system limit]

## Domain Language
- **Use:** [canonical term] - [meaning]
- **Avoid:** [term] - [why]
- **Ambiguous:** [term] - [what must be clarified]

## Decisions
- [Work-item decision about scope, terminology, priority, non-goals, target actors, or accepted
  tradeoff] - [reason]

## Assumptions
### Verified
- [Assumption verified from code, docs, or source material]

### Unverified
- [Assumption that must remain visible]

## Anti-Patterns
- [Thing to avoid] - [why]

## Validation Findings
- [Short fact from repo docs, code, official docs, or source material that affects the
  requirements]

## Open Questions
- [Question] - [why it matters / whether it blocks planning]
```

Keep sections that have useful content. Omit empty optional sections rather than filling them with
boilerplate.

Section guardrails:

- `Constraints` only include limits already imposed by the user, product, repo rules, platform,
  compatibility, accessibility, security, or existing system behaviour. Do not include proposed
  design choices.
- `Decisions` only include work-item decisions: scope, terminology, priority, non-goals, target
  actors, and accepted tradeoffs. Do not include architecture or implementation decisions.
- `Validation Findings` only include facts that affect requirements. Keep this short. Do not include
  investigation notes, file summaries, or proposed implementation.

### 5. Write Scenarios

Path:

```text
docs/epics/<epic-slug>/scenarios/<scenario-slug>.md
```

Write one file per meaningful user, system, or domain scenario.

Structure:

```md
# Scenario: [Scenario Name]

**Actor:** [User/system/stakeholder]
**Situation:** [Starting context]
**Trigger:** [What starts the scenario]

## Expected Behaviour
- [Observable outcome]

## Edge Cases
- [Edge case] - [expected handling]

## Must Not Happen
- [Failure, regression, or unacceptable outcome]

## Acceptance Criteria
- **AC1:** [Observable condition that proves this scenario is satisfied]
- **AC2:** [Observable condition that proves this scenario is satisfied]
```

Use scenario language by default. Use formal Given/When/Then only when the project already uses BDD
or the user asks for it.

Acceptance criteria define what must be true for the scenario to be accepted. They are not
implementation tasks, test plans, commands, file names, or phase sequencing.

### 6. Self-Review The Artifact

Before reporting, review the written docs and fix issues inline:

- **Placeholder scan:** remove empty headings, generic filler, repeated "TBD", or vague
  requirements.
- **Internal consistency:** check requirements, scenarios, scope, assumptions, decisions, and open
  questions do not contradict each other.
- **Scope check:** verify the epic is large enough to need an epic workspace, but not inflated with
  unrelated work.
- **Ambiguity check:** make any requirement or acceptance criterion that could be read two ways more
  concrete, or record the ambiguity as an open question.
- **Vocabulary check:** use project/domain terms consistently and avoid generic labels where a
  precise term exists.
- **Boundary check:** remove architecture, model, planning, file, command, or test-task details that
  belong to later workflow steps.

### 7. Report The Result

After writing, report:

- epic root path
- files created or updated
- any open questions that block planning
- recommended next step

Recommend an architecture or solution-design step when the solution shape, technical boundaries, or
cross-owner/runtime responsibilities need to be settled before planning. Recommend `plan-work` only
when the requirements and scenarios are coherent enough to plan without that step. Recommend more
clarification or `spike` when they are not.

## Quality Bar

- The docs should read like project-native engineering/product notes, not generic PRD boilerplate.
- Requirements explain why, who, scope, language, assumptions, and boundaries.
- Scenarios make behaviour concrete and own their acceptance criteria.
- Acceptance criteria are observable and testable, without becoming implementation tasks.
- Open questions stay visible; they are not silently solved.
- If related docs or code disagree with the source material, surface the conflict before writing or
  record it clearly as an open question.

## Anti-Patterns

### Re-running the interview
- **Detection:** asking broad discovery questions after the user asked to create the epic.
- **Why:** `gather-requirements` owns the interview.
- **Resolution:** ask only focused blocking questions, then write or recommend gathering.

### Writing a plan
- **Detection:** phase tasks, file-by-file implementation steps, commit messages, or execution
  checklists appear in the requirements or scenario docs.
- **Why:** planning is a separate skill with different context needs.
- **Resolution:** leave executable decomposition to `plan-work`.

### Architecture leakage
- **Detection:** requirements or scenarios mention new interfaces, package splits, internal APIs,
  state ownership, data flow, runtime ownership, chosen libraries, or technical solution shape.
- **Why:** `create-epic` defines the work item and behaviour; architecture or solution design shapes
  the solution later when needed.
- **Resolution:** keep only the requirement, scenario, or imposed constraint. Leave the design
  detail for a later architecture or solution-design step.

### Placeholder docs
- **Detection:** empty headings, generic filler, or "TBD" repeated across files.
- **Why:** durable docs become noise if they carry no decisions.
- **Resolution:** omit optional sections, ask a focused question, or mark one explicit open
  question with why it matters.

### Path-first design
- **Detection:** creating folders before the epic is named, scoped, or confirmed.
- **Why:** the filesystem shape can anchor the wrong concept.
- **Resolution:** resolve name, scope, and existing work first.
