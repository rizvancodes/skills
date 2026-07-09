# Architect Solution Templates

Use these templates only after `architect-solution` has loaded epic requirements, scenarios,
project context, technical guidance, current system shape, and selected solution shape.

## Architecture File

Use for `docs/epics/<epic-slug>/architecture.md`.

```md
# Architecture: [Epic Name]

**Work kind:** epic architecture
**Status:** draft
**Epic root:** docs/epics/<epic-slug>
**Sources:** requirements.md, scenarios/[...]

## Read This First
[Short statement of the chosen solution and the most important technical constraint.]

## Guidance Loaded
- [Rule, ADR, skill, reference, official doc, or code path] - [why it matters]

## Current Shape
[Relevant existing ownership, dependency direction, public entry points, models, state owners, and constraints.]

## Chosen Solution
[The technical shape and why it fits the requirements, scenarios, current system, and loaded guidance.]

## What This Is Not
- [Rejected mental model, design shape, or tempting approach] - [why]

## Owners, Runtimes, Packages, Services, Apps
- [Owner/runtime/package/service/app/module] - [responsibility in this solution]

## Models
- `models/<model-name>.md` - [what shape it owns and where it is used]

## Seams And Responsibilities
- [Seam/interface/owner] - [responsibility and relationship to linked models]

## Flow And Lifecycle
- [Important flow, sequence, or lifecycle rule]

## Operational Concerns
- [Security, privacy, permissions, performance, reliability, observability, migration, or rollout
  concern when relevant]

## Alternatives Considered
### [Alternative]
- **Why it could work:** [...]
- **Why not / why accepted:** [...]
- **Consequence:** [...]

## Decisions
- [Decision] - [rationale and consequence]

## Plan Inputs
- [Constraint, seam, model, migration concern, unresolved risk, or verification focus that
  `plan-work` must carry]

## Durable Knowledge Candidates
### LANGUAGE.md
- [Term that may need to become canonical after implementation, or "none"]

### DOMAIN.md
- [Current-state doctrine candidate if this architecture lands, or "none"]

### ADR
- [Decision that may deserve an ADR if accepted/surviving, or "none"]

## Open Architecture Questions
- [Question] - [why it matters / whether it blocks planning]
```

## Model File

Use for `docs/epics/<epic-slug>/models/<model-name>.md`.

```md
# Model: [Model Name]

**Kind:** data model | database schema | API contract | event/message | state model | component
interface | module interface | adapter port | other
**Status:** draft
**Owner:** [package/service/app/module]
**Used by:** [callers/consumers]
**Guidance:** [rules, ADRs, skills, references, or docs used for this model]

## Purpose
[What this model represents and why it exists.]

## Shape
[Fields, methods, messages, props, states, table columns, or type sketch.]

## Invariants
- [Rule that must always hold.]

## Lifecycle
[How it is created, changed, persisted, emitted, consumed, synchronized, or destroyed.]

## Validation And Errors
- [Validation rule, invalid state, failure mode, or error behaviour.]

## Compatibility
[Public/private status, backwards compatibility, migration, versioning, or deprecation notes.]

## Open Questions
- [Question]
```
