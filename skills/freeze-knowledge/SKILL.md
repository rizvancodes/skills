---
name: freeze-knowledge
description: |
  Promote stable knowledge from completed or reviewed work into durable project documentation.
  Use after implementation, review, or close-out when the user asks to freeze knowledge, promote
  decisions, extract durable docs, update language/domain docs, capture architecture decisions, or
  decide which architecture/plan candidates survived the work. Do NOT use for implementation,
  architecture design, planning, review, or broad rewriting of documentation.
---

# Freeze Knowledge

Extract stable knowledge from completed work and place it where future humans and agents can find
it quickly.

Runtime artifacts are useful while work is active. This skill runs after implementation, review, or
close-out to freeze the parts that actually survived: language, current domain doctrine,
significant decisions, and enforceable rules.

<HARD-GATE>
Do NOT implement code.
Do NOT redesign the work item.
Do NOT rewrite runtime plans, checkpoints, or reviews except to read them.
Do NOT duplicate every idea into every durable file.
Do NOT treat architecture docs, plans, or candidate sections as durable truth until final code,
review, verification, functional-test evidence, or explicit user confirmation proves they survived.

Write once in the most appropriate durable place. Duplicate only when the duplicate has a different
operational job.
</HARD-GATE>

## Timing And Source Boundary

Default to post-implementation use. The strongest sources are final diff/commits, changed code,
tests, verification notes, functional-test run reports, review findings, and explicit user
decisions.

Architecture docs, models, plans, specs, conversations, and candidate sections are inputs for
discovery only. Treat them as proposed knowledge until they match the implemented/current state or
the user explicitly confirms they should be frozen before implementation.

If the user asks to freeze knowledge before implementation, restrict the pass to settled vocabulary
or decisions they explicitly approve now. Leave provisional architecture, unresolved choices, and
plan details in the work-item artifacts.

## Progressive Disclosure And Evidence

Use the smallest context set that can safely decide whether knowledge should be frozen.

Checkpoint, status, finish notes, plans, and conversations can point to candidates, but they are not
proof by themselves. Before promoting a claim, load the source that owns or proves it. If the
owning source has not been read, treat the claim as unknown or candidate-only.

Evidence hierarchy:

1. Source files plus git diff/commits prove current implementation state.
2. Tests, verification output, and `functional-tests/runs/*.md` prove observed behavior for one
   run.
3. Review artifacts prove reviewed findings, residual risks, and quality/contract concerns.
4. Explicit user decisions prove accepted pre-implementation knowledge.
5. Architecture, models, requirements, scenarios, plans, checkpoints, status, finish notes, and
   conversation provide candidates until backed by stronger evidence or explicit approval.

If evidence conflicts, surface the conflict and do not freeze the claim unless the user resolves it.

## Durable Artifact Model

Always consider these durable targets:

```text
LANGUAGE.md
  Canonical terms, avoid terms, relationships, flagged ambiguities.
  Always separate for maintained work areas because it is cheap to load and improves human/LLM
  orientation.

DOMAIN.md
  Scoped domain and architecture doctrine: model, ownership, boundaries, lifecycle, invariants,
  accepted patterns, local principles, and anti-patterns. It records what is true now, not desired
  future architecture, glossary definitions, or decision history.

ADR
  Significant decision record: context, options, decision, consequences.
  Use only when the "why" is substantial, surprising, controversial, hard to reverse, or likely
  to be reopened.

Rules / AGENTS
  Agent operating law and enforcement posture: repo/package defaults, required checks,
  context-loading expectations, review posture, and hard constraints. Use when doctrine must
  actively shape agent behavior while editing.
```

## Placement Rules

Prefer durable knowledge next to ownership, not next to the historical work item that discovered it.

Use this placement order:

1. **Module/package ownership:** if knowledge belongs to one code owner, place it beside that code
   owner, e.g. `workers/realtime/LANGUAGE.md` and `workers/realtime/DOMAIN.md`.
2. **Epic/work-item ownership:** if knowledge describes a product capability across multiple
   owners, place it in the work item or epic docs, e.g. `docs/epics/<slug>/DOMAIN.md`.
3. **Repo-wide ownership:** if knowledge applies everywhere, place it at repo root or in the
   repo-wide docs/rules location.
4. **Rules / AGENTS:** if agents must obey it while editing or the policy is broader than one
   code owner's doctrine, promote it into the applicable rules or `AGENTS.md` location.

If placement is ambiguous, present the options and recommend one. Do not silently scatter the same
knowledge across many files.

## Promotion Tests

### Promote to LANGUAGE.md when:
- A term is stable enough that future work should reuse it.
- The wrong synonym would cause confusion or architectural drift.
- Humans or agents need the term for fast orientation.
- The term crosses module, runtime, or capability boundaries.
- Final code, docs, review, or verification repeatedly uses the same concept name.

LANGUAGE.md entries should be concise:

```md
**Billing adapter**
The boundary that translates provider-specific billing data into the app's billing model.
Avoid: billing API, provider wrapper.
```

### Promote to DOMAIN.md when:
- The current model matters for future reasoning.
- The model matches the implemented/current state, not just the intended architecture.
- Ownership, authority, lifecycle, or boundaries are stable.
- There are invariants that should be known before editing.
- Accepted local patterns or anti-patterns should be visible beside the code.
- The knowledge is scoped to one module, package, capability, or work area.
- A short current-state note helps prevent generic or wrong architecture. Put deeper rationale in
  an ADR and link it from Decision History.

DOMAIN.md should be current-state doctrine, not a decision log:

```md
## Authority

The **Billing adapter** owns provider-specific request/response translation, webhook
normalization, and billing-state synchronization. Application services consume normalized billing
events, not provider payloads directly.
```

Do not copy architecture rationale, alternatives, phased implementation, future-state intent, or
historical debate into `DOMAIN.md`. Put current truth in `DOMAIN.md`; put durable "why" in ADRs;
leave active or provisional solution shape in the work-item architecture artifact.

### Promote to ADR when:
- The decision was architecturally significant.
- There were real alternatives.
- The rationale is likely to be asked again.
- The decision is hard to reverse or surprising without context.
- The decision survived implementation or is explicitly accepted before implementation.
- Future agents would otherwise reopen the same debate.

ADR entries should capture decision memory:

```md
# ADR: Billing Provider Payloads Stay Behind The Adapter

## Context
...

## Decision
...

## Consequences
...
```

### Promote to rules/AGENTS when:
- The constraint must change agent behavior during edits.
- The instruction is broader than one local doctrine file.
- The repo/package needs a default operating posture, required checks, or context-loading rule.
- Violating it would create bugs, drift, or architectural damage.
- The rule is path-scoped or ownership-scoped.
- It is more useful as an editing guardrail than as passive documentation.

Rule entries should be enforceable:

```md
When editing `billing/**`, do not let application services depend directly on provider webhook
payloads; normalize them in the billing adapter first.
```

## Anti-Pattern Watchlist

### Four-file duplication
- **Detection:** every idea is copied into LANGUAGE.md, DOMAIN.md, ADRs, and rules.
- **Why:** repetition creates drift and makes docs feel ceremonial.
- **Resolution:** put the idea in its primary durable home. Duplicate only if the duplicate has a
  different operational job.

### Runtime artifact archaeology
- **Detection:** future agents must read old plans, checkpoints, and reviews to understand stable
  language or domain model.
- **Why:** runtime artifacts are session fuel, not durable orientation.
- **Resolution:** freeze stable language/model into LANGUAGE.md and DOMAIN.md.

### ADR as model dump
- **Detection:** ADRs contain broad current-state documentation with no clear decision, options, or
  consequences.
- **Why:** ADRs are decision memory, not the current architecture map.
- **Resolution:** put current model in DOMAIN.md; use ADR only for significant decisions.

### Rules as passive docs
- **Detection:** rules mainly define terms or explain concepts that do not affect planning,
  execution, review, or edit-time behavior.
- **Why:** passive knowledge in rules bloats the edit-time rule surface.
- **Resolution:** move pure vocabulary to LANGUAGE.md. Move local doctrine to DOMAIN.md unless the
  explanation is needed to enforce an edit-time policy.

### Premature freezing
- **Detection:** unstable speculation, open questions, or one-off task details are promoted as
  durable truth.
- **Why:** future work will treat unsettled ideas as canonical.
- **Resolution:** leave unstable material in runtime artifacts until it is accepted and used.

### Candidate promotion without proof
- **Detection:** architecture, model, plan, or ADR-candidate sections are copied into durable docs
  without checking the final code, review, verification, or user confirmation.
- **Why:** durable docs become a record of intent instead of reality.
- **Resolution:** compare candidates against what landed. Freeze only surviving knowledge.

### Checkpoint/status as proof
- **Detection:** a checkpoint or status summary is used as the reason to update `LANGUAGE.md`,
  `DOMAIN.md`, ADRs, or rules without checking the owning evidence.
- **Why:** checkpoint/status files are navigation aids and can be intentionally incomplete.
- **Resolution:** use them to find sources, then verify against code, git, review, functional-test
  reports, durable docs, or explicit user decisions.

### DOMAIN as architecture replay
- **Detection:** `DOMAIN.md` repeats architecture alternatives, phased plans, implementation order,
  or future-state design.
- **Why:** domain docs should be quick current-state orientation. Architecture docs and ADRs own
  solution rationale and decision history.
- **Resolution:** keep only current model, ownership, invariants, accepted patterns, and
  anti-patterns in `DOMAIN.md`.

## Behavioural Instructions

### Step 1: Load Sources

1. Identify final-state evidence:
   - changed code
   - final diff or commits
   - tests and verification notes
   - `functional-tests/runs/*.md`
   - review findings
   - explicit user decisions
   - existing LANGUAGE.md / DOMAIN.md / ADRs / rules

2. Identify candidate sources, but treat them as proposals until confirmed by final-state evidence:
   - architecture.md and models/*.md
   - ADR candidates, knowledge candidates, and plan inputs
   - requirements, scenarios, task brief, architecture, models, plan, checkpoint,
     status, finish notes, review, or conversation

3. Read only relevant source material. Avoid dumping entire historical folders unless the user
   explicitly asks for a full freeze pass.

4. If code ownership is relevant, inspect the owning directories and local instructions before
   deciding placement.

### Step 2: Extract Candidates

5. Extract candidate durable knowledge into these buckets:
   - **Language:** canonical terms, avoid terms, relationships, ambiguity resolutions
   - **Domain doctrine:** ownership, boundaries, lifecycles, invariants, accepted patterns,
     anti-patterns, stable model facts
   - **Decision:** significant choices with alternatives and consequences
   - **Operating rule:** repo/package defaults, required checks, context-loading expectations,
     review posture, hard constraints, enforcement rules

6. For every candidate from architecture, models, plans, specs, checkpoint/status, finish notes, or
   conversation, verify whether it survived in final code, durable docs, review, verification,
   functional-test evidence, or explicit user decision.

7. Reject candidates that are:
   - task-local only
   - speculative
   - unresolved
   - architecture-only intent not reflected in the final state
   - plan detail, task order, command, or implementation step
   - already represented clearly in existing durable docs
   - implementation detail with no durable reasoning value

### Step 3: Choose Placement

8. For each accepted candidate, choose one primary durable home:
   - LANGUAGE.md
   - DOMAIN.md
   - ADR
   - rules/AGENTS

9. If a candidate needs multiple homes, state the different job each duplicate serves.

10. Prefer updating existing durable docs over creating new files. Create files only when the owner
   has no suitable durable artifact yet.

### Step 4: Present Freeze Plan

11. Present a short freeze plan before editing unless the user explicitly asked for direct edits.

Use this structure:

```markdown
## Freeze Plan

### Language
- [term] -> [LANGUAGE.md path]

### Domain Model
- [model/invariant] -> [DOMAIN.md path]

### Decisions
- [decision] -> [ADR path or "no ADR needed"]

### Rules
- [constraint] -> [rules/AGENTS path or "no rule needed"]

### Not Freezing
- [item] -> [reason]
```

12. If the placement or stability is uncertain, ask the user before editing.

### Step 5: Edit Durable Artifacts

Before creating or substantially changing `LANGUAGE.md`, `DOMAIN.md`, or ADR files, read
`references/templates.md` and use the relevant template shape.

13. Apply focused edits only to the approved durable docs.

14. Preserve existing terminology and file style.

15. Use concise entries. Durable docs should orient quickly.

16. After editing, scan for accidental duplication or contradictions with existing docs.

### Step 6: Report Result

17. Report:
   - files updated
   - what was frozen
   - what was intentionally left in runtime artifacts
   - any follow-up ADR/rule/domain work deferred

## Tasks This Skill Handles

- "freeze this knowledge"
- "promote these decisions"
- "extract durable docs"
- "update language docs"
- "update domain docs"
- "capture architecture decisions"
- "what from this finished work should become durable?"
- "which architecture candidates survived implementation?"
- "turn this runtime knowledge into docs"
