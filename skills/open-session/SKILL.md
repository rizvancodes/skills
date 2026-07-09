---
name: open-session
description: |
  Read-only session start for the task/epic workflow. Loads checkpoint/status pointers, project
  context, minimal source-of-truth artifacts, and git state; presents current operational state;
  and recommends one next action. Use when starting or resuming work, asking "where are we?",
  "what's the status?", "pick up where we left off", "load context", "catch me up", or "resume
  work". Do NOT use for checkpointing, ending sessions, requirements gathering, artifact creation,
  architecture, planning, implementation, review, functional testing, or completion.
---

# Open Session

Load enough current state for the user to decide what to do next.

This skill is read-only. It orients; it does not act.

<HARD-GATE>
Do NOT modify files.
Do NOT invoke another skill after presenting context.
Do NOT treat checkpoint/status summaries as source-of-truth evidence.
Do NOT continue into requirements, artifact creation, planning, implementation, review, testing, or finish work.
Stop after presenting the session context and one recommended next action.
</HARD-GATE>

## Progressive Disclosure Rule

Use the smallest context set that can safely classify the workflow state and recommend the next
step.

Checkpoint and status files are resume/index artifacts. Read them first to identify the work item,
likely state, relevant paths, and likely next action. Before presenting a claim as true, read the
artifact that owns that claim.

If the owning artifact has not been read, mark the claim as unknown or unconfirmed. If
checkpoint/status conflicts with an owning artifact, source file, review artifact, functional-test
run, or git state, surface the drift and prefer the owning source.

## Work Item Contract

Supported local work item kinds:

```text
task = standalone local work packet, including imported external tickets/issues
epic = larger product or architecture capability
```

Default roots:

```text
docs/work/tasks/<task-slug>/
docs/epics/<epic-slug>/
```

Expected task artifacts, when present:

```text
docs/work/tasks/<task-slug>/
  checkpoint.md
  brief.md
  plan.md
  review.md
  finish.md
  spikes/
  functional-tests/
    cases.md
    runs/
```

Expected epic artifacts, when present:

```text
docs/epics/<epic-slug>/
  status.md
  checkpoint.md
  finish.md
  requirements.md
  scenarios/
  architecture.md
  models/
  plans/
    index.md
    phase-N.md
  reviews/
  spikes/
  functional-tests/
    cases.md
    runs/
```

Tasks do not have `status.md`. Epics may have `status.md` as a short dashboard.

Treat local IDs and external tracker IDs differently. A local ID is the repository's durable work
handle, such as `ABC-48`. External IDs are references from Jira, Linear, GitHub, support systems, or
other trackers. Use project instructions from `AGENTS.md`, `docs/work/config.md`, or equivalent
files to interpret them.

Only read what exists. Do not create missing directories or files in this skill.

## Anti-Patterns

### Context Dump
- **Detection:** loading every work artifact, project rule, source file, review, and test report
  regardless of the next action.
- **Resolution:** inventory broad sets; read full contents only when needed to classify state or
  verify a recommendation.

### Stale State
- **Detection:** reporting checkpoint/status state without checking disk and git.
- **Resolution:** validate checkpoint/status against referenced artifacts, git status, and recent
  commits.

### Missing Vocabulary
- **Detection:** summary has paths and state but no relevant domain/project terms.
- **Resolution:** extract only vocabulary needed for the current work item or next step.

### Phase Confusion
- **Detection:** summary does not identify current workflow state, completed work, active artifact,
  and next action.
- **Resolution:** classify state from artifacts and recommend exactly one next skill/input.

### Silent Conflict
- **Detection:** checkpoint/status, artifact, review, functional-test, code, and git evidence
  disagree but the summary hides it.
- **Resolution:** list the conflict. Do not silently choose the convenient source.

### Action Creep
- **Detection:** after loading context, the agent starts the recommended action.
- **Resolution:** stop. The user chooses the next skill.

## Workflow

### 1. Resolve Work Item

1. Determine whether the user supplied a work item path, local ID, external tracker ID, slug, title,
   or kind.

2. Read work item configuration when available:
   - project `AGENTS.md` for pointers
   - `docs/work/config.md` or equivalent when it exists

   Extract only local ID format, artifact roots, folder naming, and external tracker handling.

3. Inventory possible roots:
   - `docs/work/tasks/*/`
   - `docs/epics/*/`

4. Resolve the root:
   - explicit path -> use that root
   - local ID -> match artifact metadata first, then folder prefix
   - external tracker ID -> match explicit metadata/links in task or epic artifacts
   - slug/title -> match folder names and artifact titles
   - no selector and multiple roots/checkpoints -> list candidates and ask which to resume
   - no roots/checkpoints -> continue in first-session mode

5. Infer kind from root:
   - `docs/work/tasks/<slug>/` -> task
   - `docs/epics/<slug>/` -> epic

### 2. Load Resume Pointers

1. Read `checkpoint.md` when present.
   Extract resume hints only:
   - source artifact path
   - plan path or active phase
   - completed work
   - in-progress work
   - decisions/deviations
   - verification state
   - required source reads
   - unknown/not loaded notes
   - recommended next skill/input

2. For epics, read `status.md` when present.
   Extract dashboard state only:
   - current phase/state
   - blockers
   - latest review state
   - latest functional-test state
   - key artifact links
   - next action

3. Do not treat any extracted hint as proven until its owning artifact or git state is read.

### 3. Load Project Context

1. Read project `AGENTS.md` when it exists.
   Extract repo shape, ownership boundaries, verification commands, and project-level constraints.

2. If the resolved work item clearly belongs to a package/module with local `AGENTS.md`, read that
   local file too.

3. Inventory project rules when present.
   Prefer file names, path scopes, and summaries. Read full rule text only when it directly affects
   the likely next action.

4. Load `LANGUAGE.md`, `DOMAIN.md`, ADRs, local docs, and package docs selectively.
   Extract only terms, model facts, invariants, ownership boundaries, and anti-patterns relevant to
   the current work item or likely next step.

### 4. Determine Workflow State

Classify from artifacts on disk. Read the minimum owning artifacts needed for the classification.

For tasks:

1. `finish.md` exists -> complete or ready to inspect finish state.
2. `review.md` exists with blocking findings -> review pending/fixes needed.
3. `functional-tests/runs/*.md` exists -> functional-test evidence exists; read latest run only if
   it affects next action.
4. `plan.md` exists -> ready to execute, mid-work, review pending, or ready for functional test.
5. `brief.md` exists but no plan -> ready to plan or execute directly if intentionally compact.
6. `spikes/*.md` exists -> read latest/current spike only if it affects the next action.
7. No task artifact -> ready to shape or proceed ad hoc.

For epics:

1. `finish.md` exists -> complete or ready to inspect finish state.
2. `reviews/*.md` exists -> read latest relevant review only if it affects next action.
3. `functional-tests/runs/*.md` exists -> read latest run only if it affects next action.
4. `plans/index.md` exists -> read it to identify phase order and active/next phase.
5. Active `plans/phase-N.md` exists -> read only the active/next phase when needed.
6. `architecture.md` exists but no plan -> ready to plan.
7. `requirements.md` exists but no architecture -> ready to architect or plan from requirements if
   no architecture is needed.
8. `scenarios/` and `models/` exist -> inventory names; read full files only when behaviour,
   acceptance, contracts, state, or handoffs affect the recommendation.
9. `spikes/*.md` exists -> read latest/current spike only if it affects the next action.
10. No epic source artifact -> ready to shape.

State labels:

```text
ad hoc
ready to shape
ready to architect
ready to plan
ready to run
mid-work
review pending
functional-test pending
ready to freeze knowledge
ready to finish
complete
blocked
unclear
```

### 5. Validate Drift

Check git state:

- current branch
- `git status --short`
- recent commits relevant to the work item
- work-item-relevant changed files

Compare checkpoint/status against:

- referenced source artifacts
- referenced plans/phases
- review artifacts
- functional-test run reports
- git changes after checkpoint timestamp, when timestamp exists
- artifact modification times when practical

If discrepancies exist, list them in the summary. Do not silently trust stale state.

### 6. Present Session Context

Output this structure:

```markdown
## Session Context

**Project:** [project name]
**Current branch:** [branch]

### Workflow State
**Status:** [state label]
**Work kind:** [task / epic / none selected]
**Work item ID:** [local ID and external reference, or "not recorded"]
**Work item:** [name and path, or "not selected"]
**Source artifact:** [brief/requirements/architecture path, or "not written"]
**Plan:** [plan path, phase path, or "not written"]
**Current phase:** [phase number/name, or N/A]
**Completed:** [completed phases/steps/evidence, or "none recorded"]

### Resume Point
[If checkpoint exists: exact resume point, in-progress work, decisions/deviations.]
[If no checkpoint exists: omit this section.]

### Drift / Conflicts
[Stale checkpoint/status, artifact/git conflicts, or "none detected".]

### Domain Vocabulary
[Only vocabulary relevant to this work item or likely next step.]

### Active Constraints
[Relevant project rules, ownership boundaries, anti-patterns, and verification requirements.]

### Loaded Source Of Truth
[Owning artifacts, source files, git state, review artifacts, or functional-test reports read.]

### Unknown / Not Loaded
[Claims or artifacts intentionally not loaded, with when to load them.]

### Verification Commands
[typecheck/test/build commands from project or local instructions.]
[Last known verification state only when sourced from checkpoint/review/run report.]

### Recommended Next Action
Skill: [gather-requirements/create-task/create-epic/architect-solution/spike/plan-work/execute/review-code/functional-test/freeze-knowledge/finish-work/checkpoint/blocked]
Input: [exact path or none]
Reason: [why this is the next step]
```

Recommended next actions:

- `gather-requirements` when the work shape is unclear.
- `create-task` or `create-epic` when requirements are clear but no source artifact exists.
- `architect-solution` when an approved epic needs technical shape before planning.
- `spike` when one uncertainty blocks requirements, architecture, planning, or execution.
- `plan-work` when an approved source artifact has no executable plan.
- `execute` when a task plan or epic phase is ready.
- `review-code` when implementation exists without fresh review.
- `functional-test` when reviewed user-facing or externally observable behavior needs runtime
  evidence.
- `freeze-knowledge` when stable language, domain, decision, or rule candidates survived evidence.
- `finish-work` when review, functional evidence, and knowledge decisions are resolved.
- `checkpoint` when the session needs durable resume state.
- `blocked` only when no workflow skill can progress without external input.

Stop after presenting the summary.

## First-Session Mode

When no checkpoint exists:

1. If no task or epic roots exist, present minimal project context and recommend the lightest
   suitable next action. Do not force an epic workflow for narrow local work.
2. If roots exist, inventory tasks and epics by path, title, and apparent state.
3. Recommend the logical next action based on missing artifacts and git state.

## Rationalisation Table

| Rationalisation | Reality |
|-----------------|---------|
| "I already know the project, skip context loading" | Disk state is authoritative. Read it. |
| "The checkpoint is probably accurate" | Checkpoints go stale. Compare with git and owning artifacts. |
| "I'll load everything to be safe" | Context flooding hides what matters. Load the smallest sufficient set. |
| "The user knows what to do next" | The recommendation is orientation, not a command. |

## Tasks This Skill Handles

- "where are we"
- "what's the status"
- "load context"
- "start session"
- "pick up where we left off"
- "resume"
- "catch me up"
- "what phase are we on"
- "resume work"
