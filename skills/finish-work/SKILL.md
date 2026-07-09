---
name: finish-work
description: |
  Final close step for a task or epic. Use when implementation and review are complete
  and the user wants to mark the work done, close a task or epic, prepare merge readiness, or
  ask "is this finished?", "close this work", "finish this work", "ready to merge?", or "wrap
  the epic up". Verifies completion evidence, review status, commits, checks, unresolved
  blockers, and durable knowledge decisions; writes a final finish.md record and updates epic
  status/checkpoint. Do NOT use for ordinary session checkpointing, implementation, planning,
  fresh review, or durable knowledge promotion itself.
---

# Finish Work

Close a work item as complete with evidence.

`checkpoint` preserves where a session stopped. `finish-work` proves the task or epic
has reached a terminal state. It should be boring, factual, and hard to mistake for another design,
planning, or implementation pass.

<HARD-GATE>
Do NOT implement missing work.
Do NOT fix review findings.
Do NOT perform a fresh deep review here.
Do NOT close work with unresolved critical or important review findings unless the user explicitly
accepts the risk in writing.
Do NOT hide uncommitted or unrelated changes.
Do NOT promote durable knowledge here; require `freeze-knowledge` first or record an explicit
deferral.
</HARD-GATE>

## Progressive Disclosure And Evidence

Use the smallest context set that can prove finish gates.

Checkpoint and status files are pointers, not proof. Before marking a gate passed, read the owning
artifact or evidence:

- source scope: task `brief.md`, epic `requirements.md`, or `architecture.md`
- plan/execution: `plan.md`, `plans/index.md`, active/completed `plans/phase-N.md`, commits, and
  git diff/status
- review: `review.md` or `reviews/phase-N.md`
- functional behavior: `functional-tests/cases.md` and latest relevant `functional-tests/runs/*.md`
- durable knowledge: changed `LANGUAGE.md`, `DOMAIN.md`, ADRs, rules, or explicit user deferral

If the owning artifact has not been read, mark the gate unproven. If sources conflict, surface the
conflict and stop instead of finishing.

## Work Item Roots

Infer the kind from the root path:

```text
docs/work/tasks/<slug>/     -> task
docs/epics/<slug>/          -> epic
```

Write final evidence to:

```text
docs/work/tasks/<slug>/finish.md
docs/epics/<slug>/finish.md
```

For epics, also update `docs/epics/<slug>/status.md` to `complete` when all finish gates pass.

## Finish Gates

Before writing `finish.md`, verify these gates from disk and git:

1. **Source artifact exists**
   - task: `brief.md`
   - epic: `architecture.md` when present, otherwise `requirements.md`

2. **Plan state is complete or intentionally absent**
   - Small tasks may finish without a formal plan only when the scope is obvious and documented.
   - Epics should have a plan unless the user explicitly chose an ad hoc path.

3. **Execution is complete**
   - Planned tasks/phases are done.
   - There is no active in-progress item in `checkpoint.md`.
   - Acceptance criteria or done-when criteria are met or explicitly waived.

4. **Review gate is satisfied**
   - Epics require `review-code` with verdict `ship`.
   - `ship with fixes` and `no-ship` are not finished states.
   - For task work, review may be marked not required only with a reason.

5. **Verification evidence exists**
   - Required tests/typecheck/build/manual checks ran and passed, or the absence of a check is
     documented with a concrete reason.

6. **Functional-test gate is resolved**
   - User-facing or externally observable behavior has a latest relevant functional-test run, or
   - functional testing is explicitly not applicable with a concrete reason, or
   - the user explicitly accepts finishing without it.
   - Failed or blocked required functional cases are not finished states unless explicitly accepted
     by the user.

7. **Git state is clean enough to finish**
   - Relevant implementation commits are recorded.
   - Uncommitted changes are absent, or only finish/checkpoint/status documentation remains.
   - Unrelated dirty files are listed and excluded from finish claims.

8. **Durable knowledge gate is resolved**
   - Stable language/domain/decision/rule candidates were frozen with `freeze-knowledge`, or
   - no candidates exist, or
   - the user explicitly deferred them.

9. **No open blockers remain**
   - Open questions, contract gaps, and known blockers are closed, deferred, or converted into
     follow-up work.

If a gate fails, stop and report the smallest next action: `plan-work`, `execute`, `review-code`,
`functional-test`, `freeze-knowledge`, `checkpoint`, `finish-work`, or user decision.

## Behavioural Instructions

### Step 1: Resolve Scope

1. Determine the work root from the user request, a supplied artifact path, or the current
   checkpoint.

2. If multiple work items are plausible, ask which one to finish.

3. Read existing:
   - source artifact
   - plan files
   - review artifacts
   - latest relevant functional-test run, when behavior needs runtime evidence
   - checkpoint
   - epic `status.md`, scenarios, architecture, and models only when needed to prove a finish gate
   - existing `finish.md` if present

4. Check git status, current branch, uncommitted files, and recent relevant commits.

### Step 2: Evaluate Finish Gates

5. Build a gate table:

```md
## Finish Gate Check

| Gate | Status | Evidence |
| --- | --- | --- |
| Source artifact | pass/fail | path |
| Plan complete | pass/fail/deferred | path |
| Execution complete | pass/fail | checkpoint/plan evidence |
| Review | pass/fail/not required | review path/verdict |
| Verification | pass/fail/partial | commands |
| Functional test | pass/fail/not required/deferred | run path or reason |
| Git state | pass/fail/partial | commits/uncommitted files |
| Durable knowledge | pass/fail/deferred | files or deferral |
| Blockers | pass/fail | blocker summary |
```

6. If any required gate fails, do not write a completion record. Present:
   - failed gate
   - evidence
   - smallest next action
   - exact input path for that action

### Step 3: Write Finish Record

7. When gates pass, read `references/templates.md` and write `finish.md`.

8. Update `checkpoint.md` to terminal state:

```md
## Next Step
Skill: none
Input: none
Reason: work item finished; see finish.md.
```

9. For epics, update `status.md`:
   - workflow state: `complete`
   - blockers: `none` or deferred follow-up
   - next recommended action: `none`
   - finish record: `finish.md`

### Step 4: Report And Stop

10. Report:
    - finish record path
    - status/checkpoint files updated
    - verification evidence
    - git state
    - any deferred follow-up

11. Stop. Do not merge, deploy, or start new work unless the user explicitly asks.

## What This Is Not

- **Not checkpoint** - does not merely preserve resume state
- **Not execute** - does not write production code
- **Not review-code** - does not produce fresh code findings
- **Not freeze-knowledge** - does not promote durable docs directly
- **Not shipping/launch** - does not deploy production or manage rollout

## Key Principles

- **Terminal means terminal** - no hidden active phase or unresolved review
- **Evidence over confidence** - completion needs artifacts, checks, commits, review state, and
  functional evidence when applicable
- **Smallest next action** - if not done, identify exactly what remains
- **Knowledge gate matters** - runtime discoveries should not stay buried by accident
- **Clean close** - leave the next session with no ambiguity about whether the work is done

## Tasks This Skill Handles

- "finish this work"
- "close this task"
- "close this epic"
- "is this finished?"
- "ready to merge?"
- "wrap the epic up"
- "mark this complete"
- "final close"
