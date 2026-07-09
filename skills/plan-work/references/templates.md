# Plan Work Templates

Use these templates only after `plan-work` has loaded the approved artifact, project context,
dependency shape, affected owners/runtimes, and execution-unit breakdown.

## Execution Task Block

Every executable task uses this structure:

```md
### Task N: [Title]

**Context:**
- **Objective:** [what this task accomplishes]
- **Source sections:** [source artifact sections this task implements]
- **Rules:** [matched rule text or exact constraints]
- **Language:** [relevant canonical terms]
- **Domain:** [ownership, authority, lifecycle, invariants]
- **Anti-patterns:** [matched Detection/Why/Resolution entries]
- **Validation:** [verified facts or before-implementing checks]

**Interfaces:**
- **Consumes:** [models/contracts/functions/routes/events/state this task relies on, or "none"]
- **Produces:** [exact names/signatures/contracts later tasks rely on, or "none"]

**Files:**
- Read: `path`
- Create: `path`
- Modify: `path`
- Test: `path`

**Steps:**
- [ ] [small concrete action]
- [ ] [small concrete action]

**Verification:**
- [ ] Run: `command`
      Expected: [specific pass/fail/output]

**Commit:**
- [ ] Stage only this task's files: `git add path path`
- [ ] Commit: `git commit -m "[type]: [descriptive task message]"`

**Done When:**
- [ ] [specific task exit criterion]
```

## Task Plan

Use for `docs/work/tasks/<slug>/brief.md`.

```md
# [Task Name] Plan

## Source
- Brief: [path]
- Spike findings: [paths, if any]

## Dependency Shape
[chosen shape and why]

## Dependency Graph
[ordered dependencies, blockers, independent work]

## Affected Owners And Seams
[compact file/interface/seam/owner map]

## Execution Strategy
[vertical slice, refactor migration, bug-fix chain, or other selected shape]

## Done When
[task-level exit criteria]

## Tasks
[executable task blocks]

## Verification
[narrowest useful commands first]

## Review Notes
[what a fresh reviewer should focus on]
```

## Epic Plan Index

Use for `docs/epics/<slug>/plans/index.md`.

```md
# [Epic Name] Implementation Plan

## Index
- **Read this first for:** [one-line plan summary]
- **Use this plan when:** [what planning/execution this governs]
- **Read by concern:**
  - Sequencing / dependencies -> `## Dependency Graph`, `## Phase Summary`
  - Which phase to run next -> `## Phase Summary`
  - Completion bar -> `## Overall Done When`

**Source artifacts:** [requirements, architecture, model paths]
**Scenarios:** [paths, if relevant]
**Spike findings:** [paths, if relevant]

## Dependency Shape
[chosen shape and why]

## Dependency Graph
[phase dependencies, blockers, independent phases/batches]

## Phase Summary
1. [Phase name] - [one-line scope] -> `phase-1.md`

## Owner / Runtime Map
[which owner, runtime, package, service, app, or module owns each phase]

## Overall Done When
[success criteria reframed as verification where possible]
```

## Epic Phase File

Use for `docs/epics/<slug>/plans/phase-N.md`.

```md
# [Epic Name] Phase N: [Phase Name]

## Index
- **Phase scope:** [one-line outcome]
- **Read this phase when:** [condition that makes this next]
- **Skip this phase if:** [what must already be true]
- **Primary concerns:** [main seams]

## Dependency Context
- **Shape:** [overall dependency shape]
- **Depends on:** [prior phases/tasks]
- **Blocks:** [later phases/tasks]
- **Can run independently from:** [other phase/batch, or "none"]

## Context Header
- **Language:** [compact terms]
- **Constraints:** [constraint names and one-line descriptions]
- **Validation:** [phase-level verified facts]
- **Unresolved Gates:** [task numbers that require before-implementing verification]

## Work Surface
[phase-local file/interface/seam map]

## Tasks
[executable task blocks]

## Phase Checkpoint
- [ ] Acceptance criteria met
- [ ] Tests pass: `command`
- [ ] Typecheck passes: `command`
- [ ] Build/lint passes if relevant: `command`
- [ ] Done when: [phase-specific exit criteria]
- [ ] Ready for review before next phase
```
