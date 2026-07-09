# Spike Templates

Use these templates for `spike` artifacts. Keep the question, exploration boundary, findings, and
parent-workflow impact in one brief when possible.

## Spike Brief

```md
# Spike Brief: [Name]

**Status:** pending
**Parent work:** [task/epic/artifact path or "ad hoc"]
**Created:** [date]

## Question

[One blocking question.]

## Why This Blocks The Parent Workflow

[Why this must be answered before requirements, architecture, models, or planning can continue.]

## Known Context

- [Relevant fact already known]

## Relevant Files And Docs

- [Path or source]

## Suggested Exploration

- [Code reading / source validation / runtime trace / prototype]

## Stop Condition

- [The minimum evidence needed to stop.]

## Constraints And Non-Goals

- [Boundary]

## Out Of Scope

- [Adjacent question or tempting expansion]

## Return Findings As

- Direct answer
- Evidence
- Recommendation
- Impact on parent artifact
- Residual risk
- Scratch work and cleanup
```

## Findings Section

Append this to the brief when the spike is answered.

```md
## Findings

**Status:** answered
**Answered:** [date]

### Answer

[Direct answer to the spike question.]

### Evidence

- [File, command, official doc source, runtime trace, or prototype result]

### Recommendation

[What the parent workflow should do with this answer.]

### Impact On Parent Artifact

- [Task brief / epic requirements / scenario / architecture / models / plan section that should
  change or consume this finding]

### Knowledge Candidates

- [Stable LANGUAGE.md, DOMAIN.md, ADR, or rule candidate, if any]

### Residual Risk

[What remains uncertain, or "none known".]

### Scratch Work

- [Prototype path, repo changes, commands, cleanup, or "none"]

### Follow-Up Questions

- [Adjacent questions intentionally left out of this spike]
```
