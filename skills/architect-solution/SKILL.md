---
name: architect-solution
description: |
  Create an architecture document for an approved epic before implementation planning. Use when the
  user asks to architect a solution, design the technical shape, settle cross-package or cross-app
  ownership, define seams, models, contracts, data/state flow, runtime responsibilities, migration
  approach, or choose between competing technical approaches. Consumes epic requirements and
  scenario docs plus project rules, AGENTS.md, existing architecture docs, ADRs, relevant code, and
  relevant technical guidance skills or references. Writes architecture.md and any needed
  models/*.md files, and may record durable knowledge candidates for later freeze-knowledge
  promotion. Do NOT use for requirements gathering, creating epic/task docs, implementation
  planning, coding, review, or durable knowledge promotion.
---

# Architect Solution

Create the technical solution artifact that sits between an approved epic and `plan-work`.

This skill is an orchestrator. It defines the architecture workflow, artifact boundaries, and source
of truth rules. It does not encode detailed API, database, UI, security, or framework design rules;
load those from project rules, ADRs, relevant skills, references, nearby code, or official docs.

<HARD-GATE>
Do NOT write production code.
Do NOT write implementation plans, phase plans, task lists, file-by-file steps, commands, or commit
cadences.
Do NOT create or modify `requirements.md` or scenario docs unless the user explicitly asks.
Do NOT create ADRs, LANGUAGE.md, DOMAIN.md, or other durable knowledge files unless explicitly asked.
Do NOT present LANGUAGE.md, DOMAIN.md, or ADR candidates as durable truth; they are proposed
knowledge for `freeze-knowledge` to verify after implementation.
Do NOT duplicate model details in `architecture.md`; write model shape in `models/*.md` and link to
it.
Do NOT invoke `plan-work`, `execute`, or another downstream skill after writing the architecture.
Do NOT overwrite an existing architecture doc unless the user explicitly asked to update it.
</HARD-GATE>

## Required Readiness

Use this skill only when there is enough source material to make architecture meaningful.

Required minimum:

- an approved epic root or equivalent source material
- the outcome and scope from requirements
- at least one scenario or concrete behaviour slice
- acceptance direction for the important scenarios
- known constraints, assumptions, and open questions
- enough codebase context to understand existing ownership and dependency direction

If requirements are still unclear, recommend `gather-requirements` or `create-epic`. If one
technical unknown blocks the design, recommend `spike`.

## Artifact Layout

Default output:

```text
docs/epics/<epic-slug>/architecture.md
docs/epics/<epic-slug>/models/<model-name>.md
```

Create `models/` only when at least one model is needed. If the solution defines a schema, contract,
interface, state model, event, prop shape, port, API payload, database table, or other exact shape,
write it as a separate model file by default.

If project config defines a different epic root, follow it only when the user confirms it for the
project. Keep workflow artifact names fixed: `architecture.md` and `models/`.

## Source Of Truth Boundary

Follow the repository-level source-of-truth discipline:

- Requirements own why, scope, constraints, assumptions, and work-item decisions.
- Scenarios own concrete behaviour and scenario-local acceptance criteria.
- Architecture owns solution shape, ownership, seams, flows, and design rationale.
- Models own exact shapes: schemas, contracts, interfaces, state models, events, props, ports, and
  invariants.
- Plans own execution order, files, commands, verification, and commits.
- Durable knowledge files own only accepted knowledge after promotion: `LANGUAGE.md` owns canonical
  terms, `DOMAIN.md` owns current-state doctrine, and ADRs own significant decision history.

If a detail would need to be updated in two places when it changes, move it to the artifact that
owns it and reference it elsewhere.

During architecture, record durable knowledge as candidates only. `freeze-knowledge` promotes the
surviving terms, doctrine, decisions, and rules after implementation, review, verification, or
explicit user confirmation.

## Workflow

### 1. Load The Epic

Read the user-supplied epic path when present. Otherwise locate the relevant epic under:

```text
docs/epics/<epic-slug>/
```

Read:

1. `requirements.md`
2. relevant `scenarios/*.md`
3. existing `architecture.md`, if updating
4. existing `models/*.md`, if updating
5. any spike findings referenced by the epic

Extract only what affects architecture:

- scope and out-of-scope boundaries
- requirements that constrain design
- scenario behaviours that require technical support
- scenario-local acceptance criteria
- domain language
- decisions, assumptions, anti-patterns, validation findings, and open questions

Do not duplicate the epic docs in the architecture doc. Link back to them as sources.

### 2. Load Project Context

Read enough project context to design inside the actual system:

1. target project `AGENTS.md`
2. work config when referenced
3. relevant package/module/app/service `AGENTS.md`
4. relevant `.agents/rules/` files by path or topic
5. relevant `.agents/agents/` personas when the area clearly matches
6. existing ADRs, architecture docs, `LANGUAGE.md`, `DOMAIN.md`, or equivalent decision/vocabulary
   docs
7. nearby code that shows current ownership, dependency direction, contracts, models, and state flow
8. dependency files and official docs when a library/API/version decision affects the design

Use `rg` first for locating code, docs, symbols, and existing decisions.

When editing this skills/workflow repository, also read its root `AGENTS.md` for workflow-wide
source-of-truth rules.

### 3. Identify Technical Guidance

Determine which technical domains the epic actually touches. Examples:

- API or public interface design
- database schema or persistence model
- event, message, queue, or protocol contracts
- state model, lifecycle, or synchronization
- frontend component props or UI interaction model
- module seam or internal interface design
- adapter, port, runtime, service, or package ownership
- migration, compatibility, deprecation, or rollout
- security, privacy, permissions, tenancy, or abuse prevention
- performance, reliability, observability, or operational behaviour

For each relevant domain, load the best available guidance in this order:

1. project rules, local `AGENTS.md`, and local persona docs
2. existing ADRs and architecture docs
3. nearby code and tests that show accepted patterns
4. relevant installed skills or references when available
5. official docs for current libraries, frameworks, APIs, or platforms

Do not import generic rules into this skill. If no domain guidance exists, state the gap in
`Open Architecture Questions` or `Plan Inputs` rather than pretending a generic rule is project law.

### 4. Validate Source-Checkable Claims

Before choosing the solution shape, verify source-checkable technical claims that affect the
architecture:

- repository behavior: inspect code, tests, docs, and existing artifacts
- library/framework/API/runtime behavior: use official docs for the detected version
- platform/browser/protocol behavior: use official docs or standards references
- existing decisions: check ADRs, `DOMAIN.md`, architecture docs, and local rules

Do not present unverified technical claims as architecture facts. If a claim cannot be verified:

- mark it as an `Open Architecture Question` when it affects the solution choice
- mark it as a `Plan Input` when planning can safely verify it before implementation
- recommend `spike` when the claim blocks architecture

If sources conflict, surface the conflict in `Open Architecture Questions` instead of silently
choosing one.

### 5. Map Existing Shape

Before choosing a design, identify:

- current owners, runtimes, packages, services, apps, modules, adapters, and models involved
- dependency direction and import boundaries
- existing public, cross-runtime, adapter-facing, or internal entry points that callers rely on
- observable behaviours that may already be depended on
- current data/state owners and lifecycle
- relevant tests or verification seams
- matched project rules, ADRs, anti-patterns, and technical guidance

Prefer project vocabulary. Use external design vocabulary only when it clarifies the architecture
and does not conflict with project terms.

### 6. Choose The Solution Shape

Design from the epic, loaded guidance, and current system shape.

Keep the architecture decision at the right level:

- Describe ownership, relationships, seams, flows, and rationale.
- Link to model files for exact shapes.
- Compare alternatives for load-bearing decisions.
- Record unresolved design questions instead of silently choosing.
- Leave execution ordering, files, commands, verification steps, and commits to `plan-work`.

For model-heavy work:

- `architecture.md` names the model and explains why it exists.
- `models/<model-name>.md` owns the exact shape and lifecycle.
- If a detail would need to be edited when the model changes, it belongs in the model file.
- A model may be a data model, database schema, API contract, event/message, state model, component
  props interface, module interface, adapter port, or other durable shape.

For load-bearing decisions, compare alternatives in prose before settling:

- option name
- why it could work
- why it was rejected or accepted
- consequence of the choice

For difficult architecture, seam, model, interface, ownership, or runtime choices, explore two or
three meaningfully different technical shapes before choosing:

- name the shape
- explain what it optimizes for
- state what it includes and deliberately excludes
- compare it against the requirements, scenarios, current system shape, loaded guidance, and
  verification seams
- recommend one shape with the tradeoff that makes it strongest

Do not produce a long menu. Do not reopen product scope; use the approved requirements and scenarios
as constraints.

### 7. Write Architecture

Before writing architecture or model files, read `references/templates.md` and use the matching
template:

- Architecture file -> `docs/epics/<epic-slug>/architecture.md`.
- Model file -> `docs/epics/<epic-slug>/models/<model-name>.md`.

Keep only sections with useful content. Omit empty optional sections rather than filling them with
boilerplate.

### 8. Self-Review The Artifact

Before reporting, review the architecture and model docs and fix issues inline:

- **Placeholder scan:** remove empty headings, generic filler, repeated "TBD", or vague design
  claims.
- **Internal consistency:** check chosen solution, alternatives, decisions, models, seams, flows,
  plan inputs, and open questions do not contradict each other.
- **Scope check:** verify the architecture stays between approved requirements/scenarios and
  `plan-work`; remove execution sequencing, file-by-file steps, commands, commits, and detailed
  test tasks.
- **Ambiguity check:** clarify any seam, owner, model, lifecycle, or decision that could be read two
  ways, or record it as an open architecture question.
- **Vocabulary check:** use project/domain terms consistently and introduce external design
  vocabulary only when it clarifies the design.
- **Source-of-truth check:** keep exact shapes in `models/*.md`, keep requirements/scenarios as
  product truth, and keep durable knowledge as candidates only.

### 9. Report The Result

After writing, report:

- architecture path
- model paths created or updated
- guidance loaded
- chosen solution in one or two sentences
- durable knowledge candidates recorded, if any
- open architecture questions that block planning
- whether the architecture is ready for user review

Do not invoke the next skill automatically. Recommend `plan-work` only after the user accepts the
architecture.

## Quality Bar

- The architecture doc should be specific to the repo, not generic best practice.
- It should show which technical guidance was loaded and applied.
- It should verify source-checkable technical claims or keep the uncertainty visible.
- It should explain why the solution fits the epic and current system.
- It should record alternatives for load-bearing choices.
- It should define models only where the design needs them and keep model shape in `models/*.md`.
- It should preserve requirements/scenarios as the source of truth for product/work-item meaning.
- It should record durable knowledge candidates only as candidates, not as already-promoted truth.
- It should give `plan-work` enough design truth without becoming an execution plan.
- Open questions stay visible; they are not silently solved.

## Anti-Patterns

### Requirements rewrite
- **Detection:** `architecture.md` repeats the whole problem statement, actor list, scenario text,
  or acceptance criteria.
- **Why:** requirements and scenarios are already the source of truth.
- **Resolution:** summarize only the design-relevant constraint and link back to source docs.

### Guidance-free architecture
- **Detection:** the design relies on generic architecture advice while relevant project rules,
  ADRs, code patterns, skills, references, or official docs were available but not loaded.
- **Why:** architecture should be grounded in the actual system and relevant technical domain.
- **Resolution:** load the relevant guidance and revise the design.

### Unverified technical claim
- **Detection:** architecture depends on a library, framework, API, runtime, platform, browser, or
  protocol behavior that was not checked against code or official docs.
- **Why:** planning and implementation will inherit false confidence.
- **Resolution:** verify the claim, mark it as an open architecture question or plan input, or
  recommend `spike` if it blocks the design.

### Domain-rule dumping
- **Detection:** this skill or the architecture doc contains broad API, database, frontend,
  security, or framework rules unrelated to the epic.
- **Why:** domain-specific guidance belongs in focused rules, skills, references, or source docs.
- **Resolution:** reference the relevant guidance and include only the decision-relevant rule.

### Model duplication
- **Detection:** model fields, methods, states, events, payloads, table columns, props, invariants,
  or validation rules are described in `architecture.md`.
- **Why:** model shape needs one source of truth as it evolves.
- **Resolution:** move exact shape to `models/<model-name>.md` and link to it from
  `architecture.md`.

### Plan disguised as architecture
- **Detection:** phase tasks, file-by-file steps, commands, commits, or detailed test tasks appear.
- **Why:** planning needs a different context shape and belongs to `plan-work`.
- **Resolution:** move sequencing and executable detail to `Plan Inputs` or leave it for planning.

### Greenfield design
- **Detection:** the solution ignores existing ownership, dependency direction, project rules, ADRs,
  or code patterns.
- **Why:** architecture must fit the system that exists.
- **Resolution:** reload project context and redesign against current constraints.

### Decision without rationale
- **Detection:** a design choice is recorded without alternatives or consequence.
- **Why:** future agents will re-litigate or accidentally undo it.
- **Resolution:** add the tradeoff, rejected alternative, and consequence.

### Durable knowledge promotion
- **Detection:** architecture creates or updates `LANGUAGE.md`, `DOMAIN.md`, ADRs, or rules by
  default, or phrases candidates as already true after implementation.
- **Why:** architecture is proposed solution shape; durable docs should reflect accepted or
  implemented truth.
- **Resolution:** keep candidates in `architecture.md` and let `freeze-knowledge` promote them
  after implementation, review, verification, or explicit user confirmation.
