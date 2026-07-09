# Review Code Templates

Use these templates when `review-code` reaches Phase 3, Phase 4, or Phase 5.

## Contents

- Review Request Template
- Rubber-Stamp Recheck Text
- Review Artifact Template
- User Report Template

## Review Request Template

```md
# Review Request

You are reviewing code changes for production readiness. You are a fresh reviewer with no
implementation context. Do not trust claims about what was implemented. Verify by reading the code.

## Scope
- Work kind: [task/epic phase/ad hoc]
- Plan: [path or none]
- Source artifact: [path or none]
- Diff range: [base..head or working tree]
- Changed files: [summary]
- Risk areas: [list]

## Immediate Contract: Executed Plan
[Done-when criteria, task requirements, expected files, validation findings.]

## Source Contract
[Brief/requirements/architecture success criteria, scope, out-of-scope, key decisions, error
handling, testing strategy.]

## Scenarios
[Only relevant scenario constraints.]

## Project-Specific Review Questions
[Rules, anti-patterns, language/domain constraints converted into yes/no review questions.]

## Quality Heuristics
[Only when loaded: relevant heuristic baseline items. Project rules override these. Treat each as a
judgement call, not an automatic violation.]

## Validation And Assumption Checks
[Validated facts and unresolved gates. Ask reviewer to check drift.]

## Finding Context Requirement
Every Critical/Important/Minor finding must include the concrete context that makes it matter:
product behavior, source requirement, scenario, user workflow, domain invariant, project rule, or
operational/maintainer impact. Avoid abstract comments. If the issue is not user-facing, explain
why it matters for this codebase.

## Contract Review Axis
Question: did the implementation build the right thing?

Check:
- executed plan compliance
- source artifact compliance
- done-when criteria
- scenario requirements, when relevant
- scope and out-of-scope boundaries
- missing required behavior
- extra behavior not requested
- validation findings and unresolved assumption drift
- tests missing for required behavior

## Quality Review Axis
Question: was it built well?

Check:
- project AGENTS/rules compliance
- code correctness: edge cases, error paths, state invariants
- architecture: ownership, dependency direction, contracts, module boundaries
- security: auth, permissions, secrets, input validation, data isolation
- performance: hot paths, unbounded work, N+1 patterns, unnecessary re-rendering
- maintainability: names, readability, local patterns, unnecessary abstraction
- quality heuristics: apply loaded smell checks only with concrete evidence, and only where project
  rules are silent
- test quality: behavior-focused, stable seams, no brittle implementation coupling

## Required Output
Return exactly:

## Contract Findings

### Critical
[Must fix before merge. Each: file:line - issue - product/source context - why it violates the
plan/source contract - fix.]

### Important
[Should fix before merge. Each: file:line - issue - product/source context - why it matters - fix.]

### Minor
[Non-blocking contract notes or follow-up. Brief list.]

## Quality Findings

### Critical
[Must fix before merge. Each: file:line - issue - product/source/operational context - why it is
unsafe/broken - fix.]

### Important
[Should fix before merge. Each: file:line - issue - product/source/operational context - why it
matters - fix.]

### Minor
[Non-blocking quality notes or follow-up. Brief list.]

## Contract Compliance
[Plan/source requirements met, missed, or extra.]

## Validation Check
[Validated facts honoured? Any unresolved assumption drift?]

## Test Gaps
[Separate contract test gaps from quality test gaps. Say none when none.]

## Residual Risk
[Remaining uncertainty after review.]

## Evidence For Ship
[If no critical/important issues, cite specific files/lines and checks that justify clearance.]

## Verdict
[ship / ship with fixes / no-ship]
```

## Rubber-Stamp Recheck Text

```text
The previous pass found no issues. That may be correct, but empty approval is not acceptable.
Either identify issues or provide specific file/line evidence for clearance across contract,
quality, rules, tests, and risk areas.
```

## Review Artifact Template

```md
# Review: [Name]

**Reviewed:** [YYYY-MM-DD]
**Work kind:** [task/epic phase/ad hoc]
**Plan:** [path or none]
**Source artifact:** [path or none]
**Diff range:** [base..head or working tree]
**Files reviewed:** [count]
**Reviewer passes:** [1 or 2]

## Verdict
[ship / ship with fixes / no-ship]

## Contract Findings

### Critical
[Findings.]

### Important
[Findings.]

### Minor
[Findings.]

## Quality Findings

### Critical
[Findings.]

### Important
[Findings.]

### Minor
[Findings.]

## Contract Compliance
[Plan/source compliance.]

## Validation Check
[Validated facts and assumption drift.]

## Test Gaps
[Contract and quality test gaps.]

## Residual Risk
[Remaining risk.]

## Recommendations
[Non-blocking follow-up.]

---

## Review Request
[Full review request preserved for auditability.]
```

## User Report Template

```text
Contract findings:

Critical:
- [file:line] [summary] - Context: [product/source/workflow context]

Important:
- [file:line] [summary] - Context: [product/source/workflow context]

Minor:
- [summary]

Quality findings:

Critical:
- [file:line] [summary] - Context: [product/source/workflow context]

Important:
- [file:line] [summary] - Context: [product/source/workflow context]

Minor:
- [summary]

Test gaps:
- [summary]

Verdict: [ship / ship with fixes / no-ship]
Review artifact: [path, if written]
Recommended next action: [fix issues / execute fix plan / freeze-knowledge / finish-work / checkpoint / merge]
```
