---
name: execute
description: |
  Implement one clear, bounded request directly or execute one approved plan unit from plan-work.
  Use when the user says "do that" after agreeing a concrete code change, "implement this", "fix
  this", "build this", "execute the plan", "start implementation", "continue implementation",
  "run the task plan", or "run phase N". Treat a selected plan as the execution contract when one
  exists. Without a plan, establish a lightweight direct execution contract from the user's
  request, relevant owning artifacts, project rules, code, and tests; recommend plan-work only when
  accepted work needs durable decomposition or sequencing. Work in small verified increments, apply
  scope discipline, classify failures, and perform a slim self-review. Do NOT use for requirements
  gathering, artifact creation, planning, spike discovery, fresh code review, or durable knowledge
  promotion.
---

# Execute

Implement one bounded execution unit.

Use plan-backed mode when the user selects or clearly continues an approved plan. Use direct mode
when the user has requested a clear, coherent change that does not need a separate planning
artifact. Both modes keep scope tight, verify claims with fresh command output, and stop after one
execution unit.

<HARD-GATE>
Do NOT implement until the execution goal, scope, constraints, and verification path are clear.
Do NOT use direct mode to bypass unresolved product, architecture, contract, migration, or
sequencing decisions.
Do NOT start a second execution unit in the same session.
Do NOT silently deviate from the selected plan or direct execution contract.
Do NOT broaden scope to adjacent fixes or refactors.
Do NOT claim success without fresh verification.
Do NOT automatically invoke another skill after self-review.
</HARD-GATE>

## Progressive Disclosure Rule

When a plan is selected, it is the execution contract. In direct mode, record a lightweight
execution contract in live progress tracking before editing: goal, scope, constraints, significant
steps, and verification. Do not create a plan file merely to satisfy this skill.

Checkpoint/status files are resume hints only.

When resuming, use checkpoint/status to find the plan, current task, commits, or drift signals, then
verify the resume point against the selected plan or direct request, git state, and task-local files
before editing. Do not implement from checkpoint summaries.

If the plan's Context block is missing a source-critical requirement, contract, rule, or model
needed for safe execution, stop and recommend repairing the plan or loading the owning artifact.
In direct mode, load the owning source or ask one focused question when the same information is
missing. Do not silently infer source-critical context from memory.

## Execution Units

Select one execution mode and one execution unit.

```text
plan-backed task
  Plan: docs/work/tasks/<slug>/plan.md
  Unit: the task plan.

plan-backed epic
  Plan: docs/epics/<slug>/plans/phase-N.md
  Unit: one phase file.

direct
  Contract: the clear user request plus relevant owning sources, project rules, code, and tests.
  Unit: one coherent, reviewable change.
```

For epic work, run exactly one phase file. Do not run the epic plan index as an execution
unit. Use the index only to choose the next phase when the user has not specified one.

Use direct mode only when all of these are true:

- the user explicitly asked to implement, change, fix, or build something
- the intended outcome and important boundaries are clear from the request, immediate conversation,
  owning artifact, or existing behavior
- the work fits one coherent, reviewable execution unit
- relevant project rules and a meaningful verification path can be discovered
- no unresolved decision would cause materially different implementations

Recommend `plan-work` when the outcome and solution shape are accepted but execution needs durable
decomposition or sequencing: multiple independently reviewable units, cross-owner or cross-runtime
coordination, or broad migrations. If product behavior, architecture, contracts, or migration
semantics remain unresolved, stop and recommend the owning requirements, architecture, or spike
workflow instead. Do not require `plan-work` solely because no plan file exists.

## Anti-Pattern Watchlist

### No bounded contract
- **Detection:** the request leaves outcome, scope, constraints, or verification materially unclear,
  or contains multiple independently reviewable units
- **Why:** unresolved execution shape turns ambiguity into code drift
- **Resolution:** load the owning source, ask one focused question, or recommend the workflow that
  owns the unresolved decision; use `plan-work` only for accepted work needing decomposition or
  sequencing

### Second unit
- **Detection:** after completing one task plan or epic phase, the agent starts another unit
- **Why:** context pressure compounds across execution units
- **Resolution:** stop after slim self-review and recommend review or checkpoint

### Contract drift
- **Detection:** implementing something different because it seems cleaner or more convenient
- **Why:** the selected plan or direct execution contract is the authority for this session
- **Resolution:** stop, explain the issue, and get user approval before changing the contract

### Context reconstruction
- **Detection:** re-reading all rules, docs, specs, and agents despite a selected plan having
  sufficient Context blocks, or loading broad unrelated context in direct mode
- **Why:** it burns context before implementation and can introduce irrelevant constraints
- **Resolution:** in plan-backed mode, trust the plan; in direct mode, load only owning and
  task-local sources needed to establish and execute the contract safely

### Checkpoint-driven execution
- **Detection:** code changes are based on checkpoint/status wording instead of the selected plan,
  direct request, task Context block, owning source, or git evidence.
- **Why:** checkpoint/status can be stale and intentionally incomplete.
- **Resolution:** use checkpoint/status only to locate the resume point. Verify against the selected
  execution contract, git, and task-local files before editing.

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
  required by the current execution contract
- **Why:** abstractions written before proof of need increase review and maintenance cost
- **Resolution:** use the simplest implementation that satisfies the contract. Extract only when the
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

1. Select the execution mode.
   - Use plan-backed mode when the user names a plan, asks to run a plan or phase, or clearly
     continues work governed by an approved plan.
   - Otherwise assess direct mode. Do not select an unrelated plan merely because one exists.

2. For plan-backed mode:
   - resolve the specified plan path, or locate the plan that clearly governs the requested work
   - infer `docs/work/tasks/<slug>/plan.md` as a task unit
   - infer `docs/epics/<slug>/plans/phase-N.md` as an epic phase unit
   - if only an epic index is supplied, select one requested or next phase file
   - read the selected plan and extract its source artifact, scope, task order, Context blocks,
     files, verification, commit cadence, done-when criteria, and unresolved gates

3. For direct mode:
   - resolve the requested outcome from the user directive and immediate conversation
   - read project and path-scoped `AGENTS.md` instructions that govern likely touched files
   - load relevant owning artifacts, source files, tests, and git state needed to verify the request
   - establish a lightweight execution contract containing goal, scope, non-goals, constraints,
     significant steps, likely files, verification, and commit expectation
   - if the work cannot be bounded safely as one coherent unit, ask one focused question or
     recommend the workflow that owns the unresolved decision

   Do not write a plan artifact for direct mode. The live contract is temporary execution state,
   not a competing durable source of truth.

4. Read `references/scope-discipline.md` from this skill. It applies to the whole execution unit.

5. Read checkpoint/state only when useful:
   - if resuming mid-unit
   - if the user asks for a refresh
   - if git state or files suggest drift
   - if the selected plan or direct request references a prior checkpoint

   Treat checkpoint/state as hints. Verify completed/in-progress claims against the selected
   execution contract, git state, commits, and task-local files before continuing.

6. Check git branch and worktree state.
   If on `main` or `master`, warn and get explicit user consent before editing.
   If the worktree has unrelated changes, do not overwrite them. Work around them or ask if they
   block the current execution contract.

   Establish commit cadence:
   - Default: commit after each verified executable task.
   - If the selected plan gives exact commit instructions, follow them.
   - If the user explicitly says not to commit, leave changes uncommitted and report that deviation.
   - If unrelated dirty files make a clean task commit unsafe, stop and ask before continuing.

7. Create live progress tracking with one item per task or per significant direct-mode step.
   If resuming, mark completed tasks/steps as complete and start at the resume point.

Output: execution mode and unit selected, contract loaded, branch/worktree checked, progress
tracking ready.

### Phase 2: Execute Tasks

For each task in plan-backed mode, or once for the direct execution unit:

1. Load point-of-use context.
   - Plan-backed mode: read and trust the task's Context block.
   - Direct mode: use the execution contract plus the smallest relevant owning sources, rules,
     code, and tests.
   - If that context names an installed technical skill dependency, load the exact skill at this
     point before implementing. Do not preload unrelated skills or substitute a similarly named
     skill.

2. Read only task-local files needed for implementation.
   Prefer `rg` for search. Do not reload broad project context unless the selected contract is stale
   or missing critical information.

3. Run before-implementing verification gates first.
   If a gate cannot be resolved, stop and escalate.

4. If this is the first task with test files, test steps, or bug-fix work, read
   `references/tdd.md` from this skill before editing.

5. Execute the selected contract using small verified increments.
   - Keep changes inside the agreed goal and scope.
   - In plan-backed mode, stop and explain before changing an unlisted file.
   - In direct mode, add a newly discovered necessary file to live tracking when it remains within
     scope; stop if it changes the contract.
   - Choose the smallest complete increment that advances the task.
   - Prefer the simplest implementation that can satisfy the current step.
   - Prefer tests before behavior changes when the contract or codebase has a test seam.
   - After each increment, run the narrowest useful verification affected by the change.
   - Do not rerun the same passing command unless relevant code, config, dependency, or
     environment state changed.
   - Keep the project in a working state between increments and between tasks.
   - Keep increments rollback-friendly. Do not mix unrelated refactors, deletes, replacements, and
     behaviour changes unless the execution contract explicitly requires it.
   - If an incomplete increment would expose broken user-visible behaviour, use an existing feature
     flag/configuration guard when appropriate, or stop and escalate if the contract did not
     account for visibility.

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
   - The selected contract or owning source is unclear or contradictory.
   - Fix requires changing design, contract, or scope.
   - Failure is in code not touched by this task.
   - External API/framework behavior is uncertain and not already verified.

   **Out of scope**
   - Missing dependency, environment problem, service unavailable, wrong runtime version, database
     unavailable, or unmanaged dev process state.

   After two autonomous attempts for the same failure, stop and ask the user whether to reassess
   the execution contract. After three total attempts including user-directed retries, treat it as
   a planning problem until proven otherwise.

8. After each plan-backed task, or after the direct execution unit:
   - Run the required verification.
   - Update progress tracking.
   - Commit the verified task or direct unit by default:
     - inspect the diff
     - stage only files changed for this task or direct unit
     - use the selected plan's commit message when provided, otherwise write a descriptive message
     - do not include unrelated worktree changes
     - if a clean task commit is not possible, stop and ask
     - if the unit naturally produced independent verified increments and the contract allows it,
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
- If the execution contract feels too large to execute carefully, stop and recommend splitting it.

### Phase 4: Slim Self-Review

After the selected execution unit is complete, run only objective self-review:

1. Deterministic checks:
   - run the contract's typecheck command, if present
   - run the contract's test command, if present
   - run build/lint only if required by the contract or affected owner, runtime, package, service,
     or app
   - read output and confirm success

2. Exit criteria:
   - check every done-when criterion from the selected plan or direct execution contract
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
   - list approved deviations from the execution contract
   - list unapproved deviations as blockers

Self-review summary format:

```md
## Execute Self-Review

**Execution contract:** [plan path or "direct: concise goal"]
**Execution unit:** [task/epic phase/direct change]

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

- report the selected plan path or direct execution goal and unit
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

- **No plan found:** assess direct mode. Proceed when one safe bounded unit can be established;
  recommend `plan-work` only when accepted work needs decomposition or sequencing, and route other
  unresolved decisions to their owning workflow.
- **Epic index supplied:** select one phase file; do not execute the index.
- **Plan missing Context blocks:** stop and recommend re-planning or repairing the plan.
- **Pending spike or unresolved gate:** stop and recommend running the spike or resolving the gate.
- **Plan and code conflict:** stop, describe the conflict, and ask whether to update the plan.
- **Main/master branch:** stop for explicit consent before editing.
- **Unrelated dirty files:** do not overwrite. Ask only if they block the execution contract.
- **Command unavailable:** report the missing tool and follow the contract's fallback if present.

## What This Is Not

- **Not planning** - does not create or rewrite a durable plan; a direct execution contract is
  lightweight live state for one bounded change
- **Not requirements gathering or artifact creation** - does not change product scope or design
- **Not spike discovery** - does not answer blocking unknowns outside the execution contract
- **Not fresh review** - only performs objective self-review; judgement belongs to a fresh review
- **Not multi-unit execution** - one task plan, epic phase, or bounded direct change per session

## Key Decisions

### Select the smallest safe execution contract
- **Context:** some implementation requests are already clear and bounded; others need explicit
  decomposition, sequencing, or design decisions.
- **Decision:** use direct mode for one safe coherent change and plan-backed mode when a plan governs
  the work.
- **Consequence:** absence of a plan file is not a blocker; unresolved execution shape is.

### Trust the plan Context blocks
- **Context:** `plan-work` embeds task-local rules, language, domain constraints, anti-patterns,
  and validation findings.
- **Decision:** execution trusts those blocks instead of rebuilding context from scratch.
- **Consequence:** if the plan is stale, stop and re-plan instead of patching context ad hoc.

### One execution unit per session
- **Context:** implementation consumes attention quickly, especially with verification failures.
- **Decision:** run exactly one task plan, epic phase, or bounded direct change.
- **Consequence:** continuation happens through checkpoint/review, not by rolling into the next
  unit.

### Objective self-review only
- **Context:** implementing agents are poor judges of their own broader correctness.
- **Decision:** self-review checks commands, exit criteria, scope, and deviations only.
- **Consequence:** a fresh review skill should handle deeper judgement.

## Key Principles

- **Right-sized contract first** - use an approved plan when it governs the work; otherwise define
  one bounded direct execution contract
- **Context at point of use** - read the task Context block or direct-mode owning sources before
  touching files
- **Evidence before claims** - run fresh checks and read output
- **Incremental implementation** - build the smallest complete change, verify it, then continue
- **Scope discipline** - implement the selected unit, not adjacent improvements
- **Failure classification** - know whether to fix, escalate, or stop
- **Stop cleanly** - one unit, self-review, handoff

## Rationalisation Table

| Rationalisation | Reality |
|-----------------|---------|
| "There is no plan, so I must stop" | A clear bounded request can use direct mode. Recommend planning only when it reduces real uncertainty or coordinates multiple units. |
| "The user said 'do that,' but there is no artifact" | Immediate conversation can authorize direct execution when the referent, outcome, and boundaries are clear. Ask only when they are materially ambiguous. |
| "The next phase is related, I can keep going" | Related work still needs a fresh execution context. Stop after one unit. |
| "This adjacent fix is obvious" | Obvious unplanned fixes still create review risk. Log it, do not touch it. |
| "The test should pass" | Should is not evidence. Run it fresh. |
| "I'll test it all at the end" | Late feedback makes the cause of failure harder to find. Verify each meaningful increment. |
| "I'll rerun the same check just to be sure" | Repeating an unchanged passing command adds no evidence. Run it after relevant changes. |
| "This abstraction will be useful later" | Later is not the current contract. Keep the implementation simple until need is proven. |
| "The plan is mostly right" | Mostly right means partially wrong. Stop and resolve deviations before coding. |
| "I'll keep trying one more fix" | Two autonomous attempts, then escalate. Repeated retries hide uncertainty. |

## Tasks This Skill Handles

- "execute the plan"
- "run this plan"
- "run phase 1"
- "start implementation"
- "continue implementation"
- "implement this task"
- "do that"
- "fix this bug"
- "make that change"
- "build this"
- "execute"
- "pick up implementation"
