# Finish Work Templates

Read this file only when writing `finish.md`.

## finish.md

```md
# Finish: [Work Item Name]

**Work kind:** [task/epic]
**Work root:** [path]
**Finished:** [YYYY-MM-DD HH:MM]
**Final status:** complete

## Source
- [brief/requirements/architecture path]

## Implementation Evidence
- [commit sha] [message]
- [artifact or plan phase completed]

## Review Evidence
- [review path] - [verdict]
- [summary of any residual risk]

## Verification Evidence
- [command] - [pass/fail summary]
- [manual/runtime check, if any]

## Functional Evidence
- [functional-tests/cases.md or "not applicable"]
- [functional-tests/runs/<run-id>.md] - [passed/failed/blocked/not run summary]

## Durable Knowledge
- [LANGUAGE.md/DOMAIN.md/ADR/rule paths updated, "none", or explicit deferral]

## Deferred Follow-Up
- [non-blocking follow-up item, owner/path, or "none"]

## Git State
- Branch: [branch]
- Clean worktree: [yes/no]
- Unrelated changes excluded: [list or "none"]

## Completion Rationale
[Short factual explanation of why this work item is complete.]
```
