# Functional Test Templates

Use these templates for `functional-test` artifacts. Keep planned coverage in `cases.md` and
execution facts in `runs/<run-id>.md`.

## cases.md

```md
# Functional Test Cases: [Work Item Name]

**Work item:** [task/epic path]
**Source artifacts:** [brief/requirements/scenarios/architecture/plan/review paths]
**Status:** draft

## Scope

### In Scope
- [Behavior or journey covered by this functional pass]

### Out Of Scope
- [Behavior not covered] - [reason]

## Environment Assumptions

- **Runtime:** [app/API/CLI/worker/surface]
- **URL or entry point:** [URL, command, endpoint, or "not supplied"]
- **Required data/accounts:** [available data, account, role, fixture, or "not supplied"]
- **Browser tool:** Chrome DevTools MCP for browser-facing cases

## Case Matrix

### Journey 1: [Journey Name]

#### FT-001: [Case Name]

**Trace:** [requirement/scenario/acceptance/plan/review source]
**Preconditions:**
- [State required before execution]

**Steps:**
1. [Action]
2. [Action]

**Expected:**
- [Observable expected behavior]
- [Observable expected behavior]

**Evidence To Collect:**
- [route / console / network / screenshot / API response shape / command output summary]

#### FT-002: [Case Name]

**Trace:** [...]
**Preconditions:**
- [...]

**Steps:**
1. [...]

**Expected:**
- [...]

**Evidence To Collect:**
- [...]

## Coverage Notes

- [Known boundary, risk, or reason a behavior is covered elsewhere]

## Blocked Coverage

- [Case or behavior] - [missing prerequisite]
```

## runs/<run-id>.md

```md
# Functional Test Run: [Work Item Name]

**Run ID:** [yyyy-mm-dd-hhmm]
**Date:** [ISO or local date/time]
**Cases file:** ../cases.md
**Work item:** [task/epic path]
**Environment:** [URL/app/process/command/runtime]
**Tooling:** [Chrome DevTools MCP / project command / manual observation source]
**Commit:** [git sha or "not recorded"]

## Summary

- **Passed:** 0
- **Failed:** 0
- **Blocked:** 0
- **Not run:** 0

## Results

### FT-001: [Case Name]

**Status:** passed | failed | blocked | not run
**Observed:**
- [What actually happened]

**Evidence:**
- [route/request/command/screenshot path/console note]

**Notes:**
- [Unexpected detail, if any]

### FT-002: [Case Name]

**Status:** passed | failed | blocked | not run
**Observed:**
- [...]

**Evidence:**
- [...]

**Notes:**
- [...]

## Failures

- **[case ID]:** [what failed] - [product/workflow impact] - [reproduction summary]

## Blockers

- **[case ID]:** [missing prerequisite] - [what is needed to run it]

## Follow-Up

- [Fix, investigation, data/setup request, or re-run action]
```
