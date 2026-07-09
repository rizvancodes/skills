---
name: execute
description: |
  Execute one approved plan unit from plan-work. Use when the user says "run this", "execute the
  plan", "start implementation", "continue implementation", "run the task plan", "run phase N",
  "implement this plan", or "build this". Reads task or epic phase plans with
  per-task Context blocks, implements only the selected execution unit in small verified
  increments, tracks progress, applies scope discipline, verifies before claiming completion,
  classifies failures, and performs a slim self-review. Do NOT use for requirements gathering,
  artifact creation, planning, spike discovery, fresh code review, or durable knowledge promotion.
---

# Execute

Execute one approved plan unit.

Execute implements the plan. It trusts the plan's embedded Context blocks, follows the task steps,
keeps scope tight, verifies claims with fresh command output, and stops after one execution unit.

<HARD-GATE>
Do NOT implement without an approved plan.
Do NOT start a second execution unit in the same session.
Do NOT silently deviate from the plan.
Do NOT broaden scope to adjacent fixes or refactors.
Do NOT claim success without fresh verification.
Do NOT automatically invoke another skill after self-review.
</HARD-GATE>

## Progressive Disclosure Rule

The selected plan is the execution contract. Checkpoint/status files are resume hints only.

When resuming, use checkpoint/status to find the plan, current task, commits, or drift signals, then
verify the resume point against the plan, git state, and task-local files before editing. Do not
implement from checkpoint summaries.

If the plan's Context block is missing a source-critical requirement, contract, rule, or model
needed for safe execution, stop and recommend repairing the plan or loading the owning artifact.
Do not silently infer the missing context from memory.

## Operating Vocabulary

**Execution Discipline:** execution unit, plan file, per-task Context block, step-level tracking,
incremental loop, verified increment, working state, verified commit, one-unit-per-session, commit
cadence
**Decay Prevention:** compression tell, context pressure, attention budget, hard stop,
task-counter backstop, checkpoint boundary
**Failure Handling:** failure classification, autonomous fix, escalate, out-of-scope blocker,
two-attempt cap, architectural reassessment
**Quality Gates:** identify/run/read/verify/claim, deterministic checks, slim self-review, exit
criteria verification, evidence before claims
**Scope Control:** plan drift, noticed-but-not-touching, rollback-friendly change, tech debt entry,
YAGNI, touched-file discipline

## Execution Units

Resolve the execution unit from the plan path. The path is authoritative.

```text
task
  Plan: docs/work/tasks/<slug>/plan.md
  Unit: the task plan.

epic
  Plan: docs/epics/<slug>/plans/phase-N.md
  Unit: one phase file.
```

For epic work, run exactly one phase file. Do not run the epic plan index as an execution
unit. Use the index only to choose the next phase when the user has not specified one.

## Anti-Pattern Watchlist

### No plan
- **Detection:** user asks to implement from notes, brief, spec, conversation, or memory
- **Why:** execution without a plan turns design ambiguity into code drift
- **Resolution:** stop and recommend `plan-work`

### Second unit
- **Detection:** after completing one task plan or epic phase, the agent starts another unit
- **Why:** context pressure compounds across execution units
- **Resolution:** stop after slim self-review and recommend review or checkpoint

### Plan drift
- **Detection:** implementing something different because it seems cleaner or more convenient
- **Why:** the approved plan is the contract for this execution session
- **Resolution:** stop, explain the issue, recommend a plan change, and wait for user approval

### Context reconstruction
- **Detection:** re-reading all rules, docs, specs, and agents despite the plan having Context
  blocks
- **Why:** it burns context before implementation and can create conflicts with the approved plan
- **Resolution:** trust the plan. Read only task-local files and stale/missing context needed to
  implement safely

### Checkpoint-driven execution
- **Detection:** code changes are based on checkpoint/status wording instead of the approved plan,
  task Context block, source file, or git evidence.
- **Why:** checkpoint/status can be stale and intentionally incomplete.
- **Resolution:** use checkpoint/status only to locate the resume point. Verify against the plan,
  git, and task-local files before editing.

### False completion
- **Detection:** "should pass", "looks good", or claiming a check passed from memory
- **Why:** only a fresh command result proves the claim
- **Resolution:** use the gate function: identify, run, read, verify, claim

### Scope creep
- **Detection:** touching files or behavior outside the current task because an improvement is
  visible
- **Why:** unrelated changes muddy review and create unplanned risk
- **Resolution:** log the observation as noticed-but-not-touching. Do not fix it

### Oversized increment
- **Detection:** many files, multiple concerns, or a large diff accumulate before focused
  verification
- **Why:** failures become expensive when the causal change is buried
- **Resolution:** implement the smallest complete increment, verify it, then continue

### Premature abstraction
- **Detection:** adding generic utilities, factories, configuration layers, or future-proofing not
  required by the current planned task
- **Why:** abstractions written before proof of need increase review and maintenance cost
- **Resolution:** use the simplest implementation that satisfies the plan. Extract only when the
  duplication or complexity is real in the current work

### Verification theatre
- **Detection:** rerunning the same passing command without any relevant code, config, dependency,
  or environment change
- **Why:** repeated unchanged checks add no evidence and burn context
- **Resolution:** run checks after changes that could affect them. Reuse the previous result only
  as historical context, not as fresh proof

### Compression tell
- **Detection:** "similar to above", "same pattern", "as before", "repeat", "likewise", or
  shortcutting steps because they feel repetitive
- **Why:** these phrases signal context degradation
- **Resolution:** hard stop. Write a checkpoint recommendation and do not continue implementation
  in the same session

### Retry spiral
- **Detection:** repeated fixes without clearer understanding
- **Why:** confidence can rise while correctness falls
- **Resolution:** two autonomous attempts for own-code failures, then escalate

### Uncommitted task drift
- **Detection:** verified task changes are left uncommitted while execution continues to the next
  task
- **Why:** commits are execution checkpoints. Without them, review, rollback, and resume all get
  harder
- **Resolution:** commit after each verified task by default. Stage only the current task's files
  and never include unrelated worktree changes

## Behavioural Instructions

### Phase 1: Setup

1. Resolve the plan path.
   If the user specifies a path, use it. Otherwise look for:
   - `docs/work/tasks/*/plan.md`
   - `docs/epics/*/plans/phase-*.md`

2. Infer the execution unit kind from the path:
   - `docs/work/tasks/<slug>/plan.md` -> task
   - `docs/epics/<slug>/plans/phase-N.md` -> epic phase

3. If only an epic plan index is specified, read it to identify the requested or next phase, then
   execute the selected phase file only.

4. If no plan exists, stop and recommend `plan-work`.

5. Read the selected plan file.
   Extract:
   - source artifact path
   - execution unit scope
   - task list and task order
   - per-task Context blocks
   - files to read/create/modify/test
   - verification commands
   - commit instructions or expected commit cadence
   - done-when and checkpoint criteria
   - unresolved gates or before-implementing checks

6. Read `references/scope-discipline.md` from this skill. It applies to the whole execution
   unit.

7. Read checkpoint/state only when useful:
   - if resuming mid-unit
   - if the user asks for a refresh
   - if git state or files suggest drift
   - if the plan references a prior checkpoint

   Treat checkpoint/state as hints. Verify completed/in-progress claims against the plan, git
   state, commits, and task-local files before continuing.

8. Check git branch and worktree state.
   If on `main` or `master`, warn and get explicit user consent before editing.
   If the worktree has unrelated changes, do not overwrite them. Work around them or ask if they
   block the current plan.

   Establish commit cadence:
   - Default: commit after each verified executable task.
   - If the plan gives exact commit instructions, follow them.
   - If the user explicitly says not to commit, leave changes uncommitted and report that deviation.
   - If unrelated dirty files make a clean task commit unsafe, stop and ask before continuing.

9. Create live progress tracking with one item per task or per significant task step.
   If resuming, mark completed tasks/steps as complete and start at the resume point.

Output: plan loaded, execution unit selected, branch/worktree checked, progress tracking ready.

### Phase 2: Execute Tasks

For each task in the selected plan unit:

1. Read the task's Context block from the plan.
   Trust it. It should contain matched rules, language, domain constraints, anti-patterns, and
   validation findings.

2. Read only task-local files needed for implementation.
   Prefer `rg` for search. Do not reload broad project context unless the plan is stale or missing
   critical information.

3. Run before-implementing verification gates first.
   If a gate cannot be resolved, stop and escalate.

4. If this is the first task with test files, test steps, or bug-fix work, read
   `references/tdd.md` from this skill before editing.

5. Execute the task steps exactly, using small verified increments inside the task.
   - Keep changes scoped to listed files unless the plan is wrong.
   - If an unlisted file must change, stop and explain why.
   - Choose the smallest complete increment that advances the task.
   - Prefer the simplest implementation that can satisfy the current plan step.
   - Prefer tests before behavior changes when the plan or codebase has a test seam.
   - After each increment, run the narrowest useful verification affected by the change.
   - Do not rerun the same passing command unless relevant code, config, dependency, or
     environment state changed.
   - Keep the project in a working state between increments and between tasks.
   - Keep increments rollback-friendly. Do not mix unrelated refactors, deletes, replacements, and
     behaviour changes unless the plan explicitly requires it.
   - If an incomplete increment would expose broken user-visible behaviour, use an existing feature
     flag/configuration guard when appropriate, or stop and escalate if the plan did not account
     for visibility.

6. For each verification command, use the gate function:
   - **Identify:** what claim does this command prove?
   - **Run:** execute the command fresh.
   - **Read:** inspect output and exit code.
   - **Verify:** confirm the output proves the claim.
   - **Claim:** only then say the step is done.

7. If a failure occurs, classify it before acting:

   **Autonomous, max two attempts**
   - Cause is in code just written.
   - Test/typecheck/build failure is local to the current task.
   - Read `references/debugging.md` before the first autonomous fix attempt.
   - Stop adding new task steps until the failure is understood.
   - Fix root cause, add/adjust guard where appropriate, re-run the failing command.

   **Escalate**
   - Plan instruction is unclear or contradictory.
   - Fix requires changing design, contract, or scope.
   - Failure is in code not touched by this task.
   - External API/framework behavior is uncertain and not already verified.

   **Out of scope**
   - Missing dependency, environment problem, service unavailable, wrong runtime version, database
     unavailable, or unmanaged dev process state.

   After two autonomous attempts for the same failure, stop and ask the user whether to reassess
   the plan. After three total attempts including user-directed retries, treat it as a planning
   problem until proven otherwise.

8. After each task:
   - Run the task's required verification.
   - Update progress tracking.
   - Commit the verified task by default:
     - inspect the diff
     - stage only files changed for this task
     - use the plan's commit message when provided, otherwise write a descriptive message
     - do not include unrelated worktree changes
     - if a clean task commit is not possible, stop and ask
     - if the task naturally produced independent verified increments and the plan allows it,
       prefer separate rollback-friendly commits
   - Record noticed-but-not-touching observations for later, without fixing them.

### Phase 3: Context Pressure Checks

Continuously watch for compression tells:

```text
similar to above
same pattern
as before
repeat the same
analogous
likewise
```

If a compression tell appears in your own implementation reasoning or output, stop. Do not ask to
continue in the same session.

Task-counter backstop:

- After the fifth task in one execution unit, pause and self-check for context pressure.
- If the unit has more than seven tasks, recommend checkpointing after the current task and
  continuing in a fresh session.
- If the plan feels too large to execute carefully, stop and recommend splitting it.

### Phase 4: Slim Self-Review

After all tasks in the selected unit are complete, run only objective self-review:

1. Deterministic checks:
   - run the plan's typecheck command, if present
   - run the plan's test command, if present
   - run build/lint only if required by the plan or affected owner, runtime, package, service, or app
   - read output and confirm success

2. Exit criteria:
   - check every done-when criterion from the plan
   - if any criterion is unmet, fix or escalate

3. Diff sanity:
   - inspect changed files
   - confirm changes are limited to the selected execution unit
   - confirm unrelated user changes were not reverted
   - confirm verified task changes were committed, or record why they remain uncommitted

4. Tech debt and deviations:
   - read `references/technical-debt.md` if there are noticed-but-not-touching items, deferred
     shortcuts, or possible debt entries
   - list noticed-but-not-touching items
   - list approved deviations from the plan
   - list unapproved deviations as blockers

Self-review summary format:

```md
## Execute Self-Review

**Plan:** [path]
**Execution unit:** [task/epic phase]

**Checks:**
- Typecheck: [pass/fail/not run - reason]
- Tests: [pass/fail/not run - reason]
- Build/lint: [pass/fail/not run - reason]

**Exit criteria:**
- [criterion]: [met/not met]

**Scope:**
- Changed files: [list]
- Commits: [list or "not committed - reason"]
- Deviations: [none/list]
- Noticed but not touching: [none/list]

**Verdict:** [pass/fail]
```

### Phase 5: Handoff

If self-review passes:

- report the executed plan path and unit
- report checks run and results
- report commits created or why changes remain uncommitted
- report changed files
- recommend fresh review before merging or continuing
- recommend checkpointing if this was a long session or epic phase
- stop

If self-review fails and cannot be fixed within failure limits:

- report the failure and current state
- report exact command output summary
- report changed files
- report commits created before the failure, if any
- recommend checkpointing before further work
- stop

Do not start another execution unit.

## Error Handling

- **No plan found:** stop and recommend `plan-work`.
- **Epic index supplied:** select one phase file; do not execute the index.
- **Plan missing Context blocks:** stop and recommend re-planning or repairing the plan.
- **Pending spike or unresolved gate:** stop and recommend running the spike or resolving the gate.
- **Plan and code conflict:** stop, describe the conflict, and ask whether to update the plan.
- **Main/master branch:** stop for explicit consent before editing.
- **Unrelated dirty files:** do not overwrite. Ask only if they block the plan.
- **Command unavailable:** report the missing tool and follow the plan's fallback if present.

## What This Is Not

- **Not planning** - does not create or rewrite the plan
- **Not requirements gathering or artifact creation** - does not change product scope or design
- **Not spike discovery** - does not answer blocking unknowns outside the plan
- **Not fresh review** - only performs objective self-review; judgement belongs to a fresh review
- **Not multi-unit execution** - one task plan or epic phase per session

## Key Decisions

### Trust the plan Context blocks
- **Context:** `plan-work` embeds task-local rules, language, domain constraints, anti-patterns,
  and validation findings.
- **Decision:** execution trusts those blocks instead of rebuilding context from scratch.
- **Consequence:** if the plan is stale, stop and re-plan instead of patching context ad hoc.

### One execution unit per session
- **Context:** implementation consumes attention quickly, especially with verification failures.
- **Decision:** run exactly one task plan or epic phase.
- **Consequence:** continuation happens through checkpoint/review, not by rolling into the next
  unit.

### Objective self-review only
- **Context:** implementing agents are poor judges of their own broader correctness.
- **Decision:** self-review checks commands, exit criteria, scope, and deviations only.
- **Consequence:** a fresh review skill should handle deeper judgement.

## Key Principles

- **Plan first** - execute approved plans only
- **Context at point of use** - read the task Context block before touching files
- **Evidence before claims** - run fresh checks and read output
- **Incremental implementation** - build the smallest complete change, verify it, then continue
- **Scope discipline** - implement the selected unit, not adjacent improvements
- **Failure classification** - know whether to fix, escalate, or stop
- **Stop cleanly** - one unit, self-review, handoff

## Rationalisation Table

| Rationalisation | Reality |
|-----------------|---------|
| "This is small enough to skip the plan" | Small work can have a small plan. Execution still needs an approved path. |
| "The next phase is related, I can keep going" | Related work still needs a fresh execution context. Stop after one unit. |
| "This adjacent fix is obvious" | Obvious unplanned fixes still create review risk. Log it, do not touch it. |
| "The test should pass" | Should is not evidence. Run it fresh. |
| "I'll test it all at the end" | Late feedback makes the cause of failure harder to find. Verify each meaningful increment. |
| "I'll rerun the same check just to be sure" | Repeating an unchanged passing command adds no evidence. Run it after relevant changes. |
| "This abstraction will be useful later" | Later is not the current plan. Keep the implementation simple until need is proven. |
| "The plan is mostly right" | Mostly right means partially wrong. Stop and resolve deviations before coding. |
| "I'll keep trying one more fix" | Two autonomous attempts, then escalate. Repeated retries hide uncertainty. |

## Tasks This Skill Handles

- "execute the plan"
- "run this plan"
- "run phase 1"
- "start implementation"
- "continue implementation"
- "implement this task"
- "build this"
- "execute"
- "pick up implementation"
