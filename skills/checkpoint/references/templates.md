# Checkpoint Templates

Read this file only when writing `checkpoint.md` or epic `status.md`.

## checkpoint.md

```md
# Checkpoint: [Work Item Name]

**Work kind:** [task/epic]
**Work root:** [path]
**Timestamp:** [YYYY-MM-DD HH:MM]
**Reason:** [session end / context pressure / user requested / unit complete / blocked]

## Current State
**Activity:** [gather-requirements/create-task/create-epic/architect-solution/spike/plan-work/execute/review-code/functional-test/freeze-knowledge/finish-work/mixed]
**Source artifact:** [brief/requirements/scenario/architecture/model path or "not needed"]
**Plan:** [task plan or epic plans/index.md + active phase, or "not written"]
**Execution:** [not started / in progress / completed unit]
**Review:** [review path/verdict or "not reviewed"]
**Functional test:** [cases path/run path/status or "not started/not needed"]
**Durable knowledge:** [paths changed, candidates only, or "none"]

## Completed This Session
- [item]: [what it produced, with artifact path or commit]

## In Progress
**Current:** [specific item or "none"]
**Done so far:** [specific partial work]
**Remaining:** [specific remaining work]

## Git State
**Branch:** [branch]
**Commits:**
- [sha] [message] - [scope]
**Uncommitted changes:**
- [path] - [why uncommitted]

## Decisions And Deviations
- **Decision:** [decision] - [why/source]
- **Deviation:** [deviation] - [why approved or why unresolved]

## Blockers And Open Questions
- [blocker/question] - [owner or next action]

## Verification State
- [command/tool/run]: [pass/fail/blocked/not run] - [summary]

## Durable Knowledge Candidates
- **Language:** [candidate terms or "none"]
- **Domain:** [model facts/invariants or "none"]
- **Decision:** [ADR candidates or "none"]
- **Rule:** [enforceable constraints or "none"]

## Context For Next Session
- [only facts not obvious from artifacts/code/git]

## Required Source Reads For Next Session
- [path] - [why the next skill must read it]

## Not Loaded / Unknown
- [artifact or claim] - [why it was not loaded, and when to load it]

## Next Step
Skill: [open-session/gather-requirements/create-task/create-epic/architect-solution/spike/plan-work/execute/review-code/functional-test/freeze-knowledge/finish-work/blocked]
Input: [exact path or none]
Reason: [why this is next]
```

## Epic status.md

Keep this short. It is a dashboard, not a session log.

```md
# Status: [Epic Name]

**Epic root:** [path]
**Updated:** [YYYY-MM-DD HH:MM]

## Dashboard
**Current state:** [requirements/architecture/planning/executing/reviewing/functional-test/blocked/ready-to-finish/complete]
**Current phase:** [phase path/name or "none"]
**Blocked:** [yes/no] - [reason if yes]
**Next:** [skill] with [path]

## Artifacts
- Requirements: [path/status]
- Architecture: [path/status]
- Models: [path/status]
- Plan index: [path/status]
- Active phase: [path/status]
- Latest review: [path/status]
- Functional tests: [path/status]

## Progress
- Done: [short list]
- In progress: [short list]
- Remaining: [short list]

## Latest Evidence
- Review: [path/verdict or "none"]
- Functional test: [path/status or "none"]
- Verification: [latest meaningful check or "none"]

## Active Blockers
- [blocker or "none"]

## Latest Checkpoint
- [checkpoint path] - [timestamp]
```
