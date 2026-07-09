# Freeze Knowledge Templates

Read this file only when creating or substantially updating `LANGUAGE.md`, `DOMAIN.md`, or ADR
artifacts.

## LANGUAGE.md

Use `LANGUAGE.md` for compact vocabulary that helps humans and agents orient quickly. Keep entries
short. Do not turn it into a domain model or decision log.

```md
# [Area] Language

## Terms

**[Term]**
[One-sentence definition.]
Avoid: [bad synonym], [overloaded synonym].

## Relationships

- A **[Term]** [relationship].

## Flagged Ambiguities

- "[word]" was used for [A] and [B]. Prefer **[Term]** for [A] and **[Term]** for [B].
```

## DOMAIN.md

Use `DOMAIN.md` for current-state doctrine: model, ownership, boundaries, lifecycle, invariants,
accepted patterns, and anti-patterns. Link ADRs for durable "why"; do not copy decision history or
active architecture planning into domain docs.

```md
# [Area] Domain

Read first: ./LANGUAGE.md

## Model

[Current model.]

## Ownership And Boundaries

[What this area owns and does not own.]

## Lifecycle

[Current lifecycle, if important.]

## Invariants

- [Stable invariant.]

## Accepted Patterns

- [Pattern future work should follow.]

## Anti-Patterns

- [Pattern future work should avoid.]

## Decision History

- [ADR link if deeper decision history exists.]
```

## ADR

Use ADRs for significant decisions where the durable "why" matters. Keep current model
documentation in `DOMAIN.md`.

```md
# ADR: [Decision]

## Status

Accepted

## Context

[Why the decision was needed.]

## Options Considered

- [Option 1]
- [Option 2]

## Decision

[Chosen option.]

## Consequences

- [Positive/neutral/negative consequence.]
```
