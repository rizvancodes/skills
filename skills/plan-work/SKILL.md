---
name: plan-work
description: |
  Turn an approved task brief or epic artifact into an execution-ready plan. Use
  when the user says "plan this", "write the implementation plan", "ready to plan", "turn this
  brief or artifact into tasks", "break this down", or asks how to implement an approved work
  artifact. Produces right-sized plans with dependency shape, dependency graph, affected owner map,
  per-task Context blocks, exact files, verification, commit cadence, and exit criteria. Do NOT
  use for requirements gathering, artifact creation, architecture design, spike discovery,
  implementation, review, or durable knowledge promotion.
---

# Plan Work

Read an approved work artifact and produce an execution-ready plan.

Planning defines execution order, files, checks, handoff context, and stop conditions. It does not
decide what the work should be, settle architecture, write production code, run tasks, review code,
or promote durable knowledge.

<HARD-GATE>
Do NOT write or modify production code.
Do NOT scaffold implementation files.
Do NOT execute the plan.
Do NOT run spike discovery.
Do NOT plan around a pending spike as if the answer were known.
Do NOT reopen product scope or architecture decisions unless the source artifact is blocked.
Do NOT automatically invoke another skill after writing the plan.
</HARD-GATE>

## Progressive Disclosure Rule

Do not plan from checkpoint/status summaries alone. Use them only to find the approved source
artifact and likely state.

Before turning any claim into an execution task, read the artifact that owns it:
`brief.md`/`requirements.md` for scope, scenarios for behaviour and acceptance, `architecture.md`
for solution shape, `models/*` for exact contracts and state shapes, and source/code/git for actual
implementation state.

If an owning artifact is not read, mark that claim unknown and either load it, gate it, or stop
before planning around it.

## Operating Vocabulary

**Planning:** dependency graph, dependency shape, execution unit, tracer bullet, phase boundary,
reviewable slice, commit cadence, done-when criteria
**Slicing:** vertical slice, pre-agreed seam, contract-first integration, expand-migrate-contract,
prefactoring, risk-first ordering, independently grabbable task
**Context Delivery:** per-task Context block, path-scoped rules, vocabulary priming, validation
gate, source-checkable claim, context dump, point-of-use constraints

## Work Item Plans

Resolve the work kind from the approved artifact path. The path is authoritative:

```text
task
  Input: docs/work/tasks/<slug>/brief.md
  Output: docs/work/tasks/<slug>/plan.md
  Shape: compact standalone plan or reviewable vertical slice with Context blocks.

epic
  Input: docs/epics/<slug>/architecture.md when present, otherwise docs/epics/<slug>/requirements.md
  Output:
    docs/epics/<slug>/plans/index.md
    docs/epics/<slug>/plans/phase-N.md
  Shape: plan index plus phase files. Use owner-scoped subfolders only when independently
  reviewable owner/runtime tracks are real.
```

Do not infer a smaller or larger work kind when the artifact path already identifies it. External
tickets or issues should be represented locally as task briefs unless they need an epic workspace.

Split by actual owner, runtime, package, service, app, module, or dependency direction. Do not
default to backend/frontend.

## Anti-Pattern Watchlist

### Planning before approval
- **Detection:** a plan is written from rough conversation, unresolved requirements, unresolved
  architecture, or a pending spike.
- **Resolution:** stop and recommend `gather-requirements`, `create-task`, `create-epic`,
  `architect-solution`, or `spike`.

### Checkpoint-driven planning
- **Detection:** a plan turns checkpoint/status summary text into tasks without loading the
  owning brief, requirements, scenario, architecture, model, source file, or git evidence.
- **Resolution:** use checkpoint/status only as pointers. Load the owning artifact or mark the
  claim unknown before planning.

### Wrong dependency shape
- **Detection:** every plan is forced into generic phases, or a refactor is treated like a new
  capability, or a bug fix is treated like greenfield work.
- **Resolution:** choose the dependency shape before decomposing tasks.

### Horizontal slice by default
- **Detection:** tasks build all schema, then all API, then all UI, with no independently
  verifiable path.
- **Resolution:** prefer vertical slices: thin tracer bullets through the smallest useful path.
  Use horizontal work only when it is a genuine foundation or refactor migration batch.

### Unsafe refactor
- **Detection:** a plan removes or rewrites an old contract before callers have migrated.
- **Resolution:** use expand -> migrate batches -> contract.

### Missing seam
- **Detection:** tasks mention tests or integrations but do not state the stable interface,
  contract, model, port, public API, route, event, or user-visible behaviour being tested.
- **Resolution:** identify the seam or send the work back to `architect-solution`.

### Plan without Context blocks
- **Detection:** tasks list files and steps but lack matched rules, language, constraints,
  validation, and anti-patterns.
- **Resolution:** every executable task gets a Context block. Compact task plans still need it.

### Context dump
- **Detection:** plan headers paste entire artifacts, rules, docs, agent files, or unrelated
  architecture.
- **Resolution:** headers prime; task Context blocks deliver only matched content at point of use.

### Placeholder task
- **Detection:** "add error handling", "wire this up", "similar to Task 2", "TBD", or no exact
  file paths.
- **Resolution:** make the task concrete or split it. Repeat essential interface details rather
  than relying on memory.

### Unresolved assumption pass-through
- **Detection:** unresolved assumptions appear in implementation steps without a verification gate.
- **Resolution:** verify before planning, ask the user, mark a before-implementing gate, or
  recommend `spike`.

## Behavioural Instructions

### 1. Load The Approved Artifact

1. Resolve the source path. If the user specifies a path, use it. Otherwise look for:
   - `docs/work/tasks/*/brief.md`
   - `docs/epics/*/architecture.md`
   - `docs/epics/*/requirements.md`

   If the path came from checkpoint/status, treat it as a pointer and still read the source
   artifact before extracting planning claims.

2. Infer work kind from the source path:
   - `docs/work/tasks/<slug>/brief.md` -> task
   - `docs/epics/<slug>/architecture.md` -> epic
   - `docs/epics/<slug>/requirements.md` -> epic

3. If no approved artifact exists, stop and recommend the smallest upstream step:
   `gather-requirements`, `create-task`, `create-epic`, or `architect-solution`.

4. Read the artifact section by section. Extract:
   - objective and done-when criteria
   - scenario or acceptance criteria
   - scope and out-of-scope boundaries
   - affected owners, runtimes, packages, services, apps, modules, or user flows
   - language and domain invariants
   - architecture, models, seams, contracts, or boundaries already approved
   - validation findings, unresolved assumptions, open questions, spike findings

5. For epics, read relevant scenario/model files only when they affect sequencing, entry paths,
   handoffs, contracts, state, permissions, terminal outcomes, or acceptance criteria.

6. If required sections are empty, blocking questions remain, or spike briefs are pending without
   findings, stop and surface the gap. Do not fill the gap silently.

### 2. Load Project Context

7. Read project `AGENTS.md`. Extract repo shape, ownership boundaries, dependency direction,
   verification commands, and workflow config.

8. Read local `AGENTS.md` files for affected owners when present.

9. Read relevant `LANGUAGE.md`, `DOMAIN.md`, ADRs, architecture docs, model docs, and nearby source
   docs. Extract only terms, invariants, decisions, and constraints that affect execution order or
   task context.

10. Read `.agents/rules/` inventory when present. Prefer path scopes and short summaries first.
    Read full rule text only for rules that match files a task will touch.

11. Read relevant `.agents/agents/` persona files when present. Extract matching vocabulary and
    anti-patterns. Do not paste whole persona files into the plan.

12. Detect stack versions from dependency files. If plan steps rely on version-sensitive
    library/API/framework/runtime behaviour not already verified in the source artifact, verify
    against official docs or add a task-level verification gate.

    If the approved architecture, model, brief, or project rules name an installed technical skill
    whose guidance is required during implementation, carry its exact skill ID into only the
    matching task Context blocks. Do not copy the skill body into the plan and do not attach every
    available technical skill to every task.

### 3. Validate Before Decomposition

13. Resolve open questions that affect task shape or ordering before writing tasks. Ask one
    focused question when the answer cannot be discovered from artifacts or code.

14. For each unresolved assumption:
    - verify from code, official docs, project docs, or source artifacts when possible
    - ask the user if it cannot be verified
    - add a task-level before-implementing gate only when the plan can safely proceed without the
      answer

15. If spike findings exist, fold them into the dependency graph. If findings conflict with the
    source artifact, stop and recommend updating the source artifact before planning.

### 4. Map The Work Surface

Before decomposing tasks, map what the plan will touch.

Include:

- files, folders, packages, apps, services, modules, routes, components, workers, or adapters
- models, contracts, events, interfaces, ports, schemas, or state shapes
- seams where tests, integrations, or callers should attach
- existing patterns to follow
- risky or version-sensitive APIs to verify

Do not over-map unrelated areas. The map should explain task boundaries, not document the whole
codebase.

### 5. Choose Dependency Shape

Classify the work before writing tasks. Pick one primary shape and combine shapes only when the
work genuinely needs it.

```text
new capability
  foundation/seam -> vertical slice 1 -> vertical slice 2 -> hardening

integration
  contract/seam -> producer -> consumer -> end-to-end verification

refactor
  expand -> migrate batch A/B/C -> contract

bug fix
  reproduce -> localize -> fix -> regression guard

technical cleanup
  safety net -> mechanical batches -> remove old path

unknown
  spike -> update source artifact -> plan again
```

Rules:

- Prefer vertical slices / tracer bullets for new behaviour: each slice should prove one useful
  path end to end.
- Use foundation work only when later slices genuinely depend on it and it is independently
  reviewable.
- Use expand -> migrate -> contract for refactors that affect existing callers.
- Use prefactoring when a small preparatory change makes the actual change safer.
- Put high-risk or source-checkable assumptions early.
- Mark what can run independently and what must be sequential.

For plans spanning more than one phase, owner, or migration batch, present the proposed dependency
shape and breakdown to the user before writing detailed tasks.

### 6. Decompose Into Execution Units

Choose the output shape:

- Task: one compact plan for a focused standalone execution unit.
- Epic: plan index plus phase files.

Each execution unit should be independently understandable, testable, and reviewable. If it cannot
be reviewed as a coherent diff, split it.

Task sizing:

- Prefer tasks touching 1-5 files.
- Split tasks that touch unrelated owners, mix refactor with behaviour, or naturally need separate
  reviews.
- For wide refactors, split by migration batch rather than by technical layer.
- For independent slices, record independence explicitly so a future handover/subagent flow can use
  it.

### 7. Write Execution Tasks

Before writing executable tasks, read `references/templates.md` and use its execution task block.

Task writing rules:

- Use exact paths.
- Keep each step concrete and small.
- Prefer test-first steps for behaviour changes when a real test seam exists.
- If tests are not appropriate, state the reason and provide another verification path.
- Include commands with expected outcomes.
- Include commit instructions after task verification.
- Stage only files changed for the current task.
- Include code snippets only for tests, model shapes, interfaces, schemas, command examples, or
  ambiguous seams. Do not paste full production implementations by default.
- If a later task depends on a name, signature, model, or contract produced here, put it in
  `Interfaces`.

### 8. Write Plan Files

Before writing plan files, read `references/templates.md` and choose the matching template:

- Task brief -> task plan.
- Epic artifact -> epic plan index plus epic phase files.

Preserve the template headings unless a section is explicitly irrelevant. If a section is
irrelevant, write `None` or a short reason rather than silently deleting it.

### 9. Self-Review And Exit

Self-review the plan and fix issues inline:

- **Artifact coverage:** every acceptance/success criterion maps to at least one task.
- **Dependency shape:** selected shape fits the work; refactors use expand/migrate/contract when
  needed.
- **Dependency order:** no task depends on work that appears later without a clear gate.
- **Independence:** tasks marked independent can actually run independently.
- **Seam clarity:** tests/integrations target a stated seam or behaviour.
- **Context coverage:** every task has a non-empty Context block.
- **Interface continuity:** produced names/signatures/models match later consumers.
- **Rule matching:** tasks touching scoped paths include applicable rule text or exact constraint.
- **Validation coverage:** source-checkable implementation assumptions are verified or gated.
- **Source ownership:** no task relies on checkpoint/status-only claims.
- **Verification:** every task and plan has exact commands or an explicit reason none apply.
- **Commit cadence:** every executable task has a commit step after verification.
- **Placeholder scan:** no TBD, TODO, "wire up", "handle errors", "similar to", or vague steps.
- **Scope check:** no task or phase is too large for one execution session.
- **Implementation check:** the plan did not edit or smuggle in production code.

Then report:

```text
Plan written to `<path>`. [N] execution units, [M] tasks. Dependency shape: [shape].
Please review and tell me if you want changes before execution.
```

If planning surfaced durable language, model facts, decisions, or rules, list them as
knowledge-freezing candidates only. Do not promote them unless the user asks.

Recommend the next action: `execute`, review the plan, `freeze-knowledge`, or stop. Do not invoke
another skill automatically.

## Error Handling

- **No approved artifact:** stop and recommend `gather-requirements`, `create-task`, or
  `create-epic`.
- **Architecture required:** stop and recommend `architect-solution`.
- **Pending spike:** stop and recommend running the spike first.
- **Artifact and spike findings conflict:** stop and recommend updating the source artifact.
- **No seam for tests/integration:** ask one focused question or recommend `architect-solution`.
- **No rules or agent files:** valid state. Use artifact language, project instructions, and code.
- **Rules lack path scopes:** match by content and note content-based matching.
- **Dependency files missing:** proceed with artifact/project context and mark versions unconfirmed.
- **Plan too large:** split into smaller execution units. Do not compress Context blocks.
- **User asks to implement while planning:** finish or pause planning first, then use `execute`.

## What This Is Not

- **Not requirements gathering or artifact creation** - does not decide what the work should be.
- **Not architecture design** - does not settle ownership, models, or seams from scratch.
- **Not spike discovery** - does not answer blocking unknowns through exploration.
- **Not implementation** - does not edit production code.
- **Not review** - does not assess completed code.
- **Not a context dump** - embeds matched context at point of use.

## Key Principles

- **Approved artifact first** - planning starts after source artifacts are approved.
- **Shape before tasks** - choose the dependency shape before decomposing.
- **Vertical slices by default** - new behaviour should prove useful paths end to end through thin
  tracer bullets.
- **Expand before migration** - refactors keep old callers working until migration is complete.
- **Every task carries context** - rules, language, anti-patterns, validation, and constraints at
  point of use.
- **Interfaces travel between tasks** - record exact consumed and produced shapes when tasks depend
  on each other.
- **Small execution units** - one coherent outcome per session.
- **Narrow checks first** - run focused verification before broadening.
- **No hidden uncertainty** - unresolved assumptions are verified, gated, or sent upstream.

## Rationalisation Table

| Rationalisation | Reality |
|-----------------|---------|
| "The brief is clear enough to implement directly" | The artifact defines what. The plan defines order, context, files, and checks. |
| "Every plan can use the same phase template" | New work, refactors, bug fixes, and cleanups need different dependency shapes. |
| "We can refactor by replacing the old thing" | Wide refactors need expand -> migrate -> contract to keep the system green. |
| "Context blocks are redundant" | Rules and language decay mid-session. Context blocks reinforce them at the exact task. |
| "One plan file is simpler" | Simpler to write, harder to execute. Large epic work needs phase-sized files. |
| "The executor can figure out the seam" | If the test/integration seam is unclear in the plan, execution will drift. |
| "Verification can wait until the end" | Late verification makes failure expensive. Put checks beside the tasks that need them. |
