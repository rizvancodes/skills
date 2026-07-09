---
name: create-task
description: |
  Create a compact durable task brief from clarified requirements, prior conversation, an external
  ticket, issue, support request, bug report, or small standalone work item. Use when the user says
  to create a task, issue, brief, small work item, or standalone implementation artifact
  that does not need an epic workspace. Writes one task brief under docs/work/tasks by default. Do
  NOT use for interviewing from scratch, creating epic docs, implementation planning, coding,
  review, checkpointing-only work, or durable language promotion.
---

# Create Task

Create one compact durable brief for standalone work.

This skill is an artifact writer. It turns clarified requirements into a task brief that can be
planned or executed later. It may ask a focused question if a blocking gap remains, but it should
not restart the requirements interview.

<HARD-GATE>
Do NOT write production code.
Do NOT write implementation plans, execution tasks, file-by-file steps, or commit cadences.
Do NOT create epic docs, scenario folders, acceptance documents, or status files.
Do NOT create checkpoints or finish records.
Do NOT invent requirements, acceptance criteria, IDs, owners, or constraints.
Do NOT invoke `plan-work`, `execute`, or another downstream skill after writing the brief.
Do NOT overwrite an existing task brief unless the user explicitly asked to update it.
</HARD-GATE>

## Required Readiness

Create a task only when the source material is clear enough for a compact brief.

Required minimum:

- the outcome the task should achieve
- the affected user, actor, system, module, or workflow
- why the task matters
- scope boundaries and non-goals
- acceptance direction
- known constraints, assumptions, and unresolved questions

If the work has multiple scenarios, delivery slices, owners, packages, services, apps, or runtimes,
recommend `create-epic` instead. If the minimum is missing, ask one focused question or recommend
`gather-requirements`. If one technical unknown blocks the brief, recommend `spike`.

## Artifact Layout

Use the project-configured ID and task-root convention when present. Read `AGENTS.md` and
`docs/work/config.md`, or the equivalent project-local file named by repo instructions. Keep
workflow artifact names fixed.

Default layout:

```text
docs/work/tasks/<task-slug>/
  brief.md
```

Create only the task root and `brief.md`. Later workflow skills can add plans, reviews,
checkpoints, or finish records when those steps actually happen.

## Workflow

### 1. Load Source Material

Read only the context needed to write an accurate brief:

1. Current conversation and any output from `gather-requirements`.
2. User-provided ticket, issue, support request, bug report, note, transcript, or URL.
3. Project `AGENTS.md` and work config.
4. Related existing task briefs, epic docs, ADRs, `LANGUAGE.md`, `DOMAIN.md`, and nearby docs.
5. Code paths only when repository behavior affects the task shape.

Do not flood context with every rule or nearby file. Load enough to avoid generic briefs.

### 2. Resolve Name, ID, And Path

Choose the task title and slug from the user's language.

For work item IDs:

- Use a user-supplied local ID when it matches project convention.
- Use the project allocation source when config defines one.
- If no convention exists, use a slug-only path.
- Ask before inventing the first project ID prefix.
- Record external tracker IDs as references; do not replace the local ID with them.

Before writing, check whether the target path already exists. If it exists, ask whether to update it
unless the user already asked to update that exact task.

### 3. Write The Task Brief

Path:

```text
docs/work/tasks/<task-slug>/brief.md
```

Structure:

```md
# Task: [Task Name]

**Work kind:** task
**Status:** draft
**Task root:** docs/work/tasks/<task-slug>
**Local ID:** [ID or "not assigned"]
**External references:** [tracker links/IDs or "none"]
**Parent epic:** [epic path/ID or "none"]

## Intent
[What this task should change and why it matters.]

## Scope
### In Scope
- [Included behavior, fix, investigation result, or small capability]

### Out Of Scope
- [Excluded behavior] - [reason]

## Acceptance Criteria
- **AC1:** [Concrete observable condition that must be true]
- **AC2:** [Concrete observable condition that must be true]

## Scenario
- **Actor:** [User/system/stakeholder]
- **Situation:** [Starting context]
- **Trigger:** [What starts the behavior]
- **Expected behaviour:** [Observable outcome]
- **Must not happen:** [Failure, regression, or unacceptable outcome]

## Context
- [Relevant files, docs, modules, previous work, or external source material]

## Constraints
- [Business, product, technical, UX, platform, or timeline constraint]

## Decisions
- [Decision] - [reason]

## Assumptions
### Verified
- [Assumption verified from code, docs, or source material]

### Unverified
- [Assumption that must remain visible]

## Validation Findings
- [Fact checked from repo docs, code, official docs, or source material]

## Open Questions
- [Question] - [why it matters / whether it blocks planning]

## Planning Notes
- [Sequencing or risk note that helps `plan-work`, without writing the plan]
```

Keep sections that have useful content. Omit empty optional sections rather than filling them with
boilerplate.

### 4. Self-Review The Artifact

Before reporting, review the brief and fix issues inline:

- **Placeholder scan:** remove empty headings, generic filler, repeated "TBD", or vague acceptance
  criteria.
- **Internal consistency:** check intent, scope, scenario, acceptance criteria, constraints,
  assumptions, and open questions do not contradict each other.
- **Scope check:** verify the work is compact enough for one task brief; recommend `create-epic`
  instead if it needs multiple scenarios, owners, delivery slices, packages, services, apps,
  runtimes, or modules.
- **Ambiguity check:** make any acceptance criterion or scenario detail that could be read two ways
  more concrete, or record the ambiguity as an open question.
- **Vocabulary check:** use project/domain terms consistently and avoid generic labels where a
  precise term exists.
- **Boundary check:** remove implementation plans, file-by-file steps, commands, phase tasks, and
  architecture details that belong to later workflow steps.

### 5. Report The Result

After writing, report:

- task brief path
- any open questions that block planning
- recommended next step

Recommend `plan-work` only when the brief is coherent enough to plan. Recommend more clarification
or `spike` when it is not.

## Quality Bar

- The brief should be compact enough to read in one pass.
- The brief should still carry enough context for a future agent to plan the work without replaying
  the whole conversation.
- Acceptance criteria are observable and testable.
- Scenario content stays inline; separate scenario files belong to epics.
- Planning notes are hints, not executable tasks.
- Open questions stay visible; they are not silently solved.
- If related docs or code disagree with the source material, surface the conflict before writing or
  record it clearly as an open question.

## Anti-Patterns

### Inflating the task into an epic
- **Detection:** separate requirements, scenarios, acceptance, and spec docs are created.
- **Why:** standalone tasks should stay lightweight.
- **Resolution:** write one `brief.md`, or recommend `create-epic` if the work needs more shape.

### Re-running the interview
- **Detection:** asking broad discovery questions after the user asked to create the task.
- **Why:** `gather-requirements` owns the interview.
- **Resolution:** ask only focused blocking questions, then write or recommend gathering.

### Writing a plan
- **Detection:** file-by-file implementation steps, phase tasks, commit messages, or execution
  checklists appear in the brief.
- **Why:** planning is a separate skill with different context needs.
- **Resolution:** keep sequencing as brief planning notes and leave executable decomposition to
  `plan-work`.

### Placeholder brief
- **Detection:** empty headings, generic filler, or "TBD" repeated through the file.
- **Why:** a compact brief should carry decisions, not ceremony.
- **Resolution:** omit optional sections, ask a focused question, or mark one explicit open
  question with why it matters.
