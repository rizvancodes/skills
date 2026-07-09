---
name: checkpoint
description: |
  Write durable resume state for the current task or epic. Use when ending a session, saving
  progress, recording context pressure, preserving a resume point, or when the user says
  "checkpoint", "save progress", "wrap up this session", "session close", "session ending",
  "context is getting long", or "save where we are". Performs a light disk/git state scan,
  writes checkpoint.md, and for epics updates status.md as a short dashboard when high-level state
  changed. Do NOT use for starting sessions, requirements gathering, artifact creation, planning,
  implementation, review, functional testing, temporary transfer briefs, or final completion.
---

# Checkpoint

Write durable workflow state at the work item root.

`open-session` reads checkpoints later. A checkpoint must reflect what actually exists on disk and
in git, not just conversation memory.

<HARD-GATE>
When checkpointing is requested or a session/context boundary is reached, write the checkpoint before continuing.
Do NOT continue workflow work after writing a checkpoint.
Do NOT use checkpoint as a design document, plan, review, functional-test report, or temporary transfer brief.
Do NOT promote durable knowledge here. Record candidates for `freeze-knowledge`.
</HARD-GATE>

## Context Pressure Signals

Treat checkpointing as due when a session shows:

- repeated shortcuts such as "same as above", "and so on", or "etc."
- vague steps replacing previously specific steps
- forgotten file paths, artifact names, or constraints that were already established
- repeated failed fix attempts
- self-contradiction or drift from the current plan/artifacts

## Artifact Boundary

Write checkpoints here:

```text
docs/work/tasks/<slug>/checkpoint.md
docs/epics/<slug>/checkpoint.md
```

For epics only, update:

```text
docs/epics/<slug>/status.md
```

`status.md` is the current epic dashboard: current phase, completed phases, blockers, latest
review/functional-test state, next action, and key artifact links.

`checkpoint.md` is the latest session resume record: what changed, what is in progress, decisions,
deviations, blockers, verification, git state, and the exact next skill/input.

Do not create `status.md` for tasks.

Checkpoint and status files are pointers, not proof. For important state claims, prefer artifact
paths, commit SHAs, diff evidence, command names, or run-report paths. If a detail comes only from
conversation, label it as conversation-only context.

Do not write checkpoint content in a way that lets the next agent safely skip the owning artifact.

Read `references/templates.md` before writing `checkpoint.md` or epic `status.md`.

## Workflow

### 1. Resolve Work Item

Determine whether the user supplied a work item path, artifact path, plan path, review path, slug,
or current repo context.

Infer the work item root:

```text
docs/work/tasks/<slug>/... -> task
docs/epics/<slug>/...      -> epic
```

If multiple work items are plausible, ask which one to checkpoint.

If no work item root exists and no workflow artifacts were created, report that there is no durable
workflow state to checkpoint. Do not create a random checkpoint.

### 2. Perform A State Scan

Do a light state scan, not a full context reload.

Always inspect:

- existing `checkpoint.md`, if present
- current branch
- `git status --short`
- recent commits relevant to the session, when identifiable
- diff summary for uncommitted changes

Do not start broad verification solely for checkpointing. Record known command/run results and the
checks still needed.

For tasks, inspect only the relevant current-state files:

- `brief.md`
- `plan.md`
- latest/current `spikes/*.md`
- `review.md`
- latest `functional-tests/runs/*.md`
- `finish.md`, only if the task appears near completion or already complete

For epics, inspect only dashboard and current execution files:

- `status.md`
- `checkpoint.md`
- `plans/index.md`
- active `plans/phase-N.md`, if identifiable
- latest/current `spikes/*.md`
- latest relevant `reviews/*.md`
- latest `functional-tests/runs/*.md`
- `finish.md`, only if the epic appears near completion or already complete

Read source artifacts such as `requirements.md`, scenario files, `architecture.md`, model files, or
`brief.md` only when needed to identify the current resume point or resolve a contradiction.

### 3. Detect Session Activity

Determine what happened from artifacts, git, and the conversation:

- `gather-requirements`
- `create-task`
- `create-epic`
- `architect-solution`
- `spike`
- `plan-work`
- `execute`
- `review-code`
- `functional-test`
- `freeze-knowledge`
- `finish-work`

If multiple activities happened, record them all but keep the resume point singular.

For functional testing, record the cases path, latest run report, evidence paths, and
pass/fail/blocked/not-run counts when present.

### 4. Determine One Next Step

Recommend exactly one next skill and exact input path.

Allowed next skills:

```text
open-session
gather-requirements
create-task
create-epic
architect-solution
spike
plan-work
execute
review-code
functional-test
freeze-knowledge
finish-work
blocked
```

Prefer:

- `execute` when a task plan or epic phase is ready and not implemented
- `review-code` after execution completes and no fresh review exists
- `functional-test` after review when runtime/user-facing verification is still needed
- `freeze-knowledge` when stable language, domain, ADR, or rule candidates were identified
- `finish-work` when implementation, review, functional testing, and knowledge decisions are resolved
- `blocked` only when no workflow skill can progress the work without external input

### 5. Write The Checkpoint

Use the checkpoint template in `references/templates.md`.

Include:

- current state summary
- completed work this session
- in-progress work and exact remaining work
- branch, commits, and uncommitted changes
- decisions, deviations, blockers, and open questions
- verification state
- durable knowledge candidates
- context needed by the next session that is not obvious from artifacts/code/git
- required source reads for the next skill
- artifacts or claims not loaded and therefore unknown
- exact next skill, input path, and reason

Preserve useful prior checkpoint history only when it is still true. If disk/git state contradicts
the prior checkpoint, make the contradiction explicit.

### 6. Update Epic Status When Needed

For epics, update `status.md` only when high-level workflow state changed:

- current phase
- completed/reviewed phases
- active blockers
- latest review verdict
- latest functional-test state
- next recommended action
- key artifact links

Keep session nuance in `checkpoint.md`. Keep `status.md` short enough to act as an index card.

### 7. Report And Stop

Report:

```text
Checkpoint saved to `<path>`.
Next: [skill] with `[input path]`.
```

Then stop. Do not continue workflow work.

## Anti-Patterns

- **Memory-only checkpoint:** writing from conversation memory without checking disk/git state.
- **Full epic reload:** reading every requirement, scenario, architecture, model, plan, review, and
  run report when only the current state is needed.
- **Vague resume point:** saying "continue implementation" without a skill, path, and reason.
- **Design pollution:** adding new product or architecture opinions to checkpoint instead of source
  artifacts.
- **Report/status duplication:** copying full review or functional-test detail into checkpoint or
  `status.md` instead of linking the source artifact.
- **Lost commits:** omitting commit SHAs/messages created during the session.
- **Continuing after checkpoint:** using checkpoint as a pause marker and then resuming work in the
  same flow.

## What This Is Not

- Not `open-session`: it writes state; it does not only load and recommend.
- Not `handover`: it is durable workflow state, not a temporary transfer brief.
- Not requirements gathering, artifact creation, planning, implementation, review, or functional
  testing.
- Not `freeze-knowledge`: it records candidates but does not promote them.
- Not `finish-work`: it preserves resume state but does not mark work complete.
