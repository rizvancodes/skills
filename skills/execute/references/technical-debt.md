# Technical Debt Register

Loaded during execution self-review and when noticed-but-not-touching observations need durable
tracking.

## When To Log Debt

Log an entry when:

- You took a shortcut to keep the task scoped.
- You noticed something worth fixing outside the current task.
- A reviewer or test revealed an issue deferred rather than fixed.
- You chose a simpler approach while knowingly leaving a stronger approach for later.

Do not log:

- approved design decisions
- work already tracked in the plan
- vague opinions about untouched code
- speculative cleanup with no clear impact

## Default Location

Use the project-level file when the repo has no more specific convention:

```text
docs/tech-debt.md
```

If the file exists, append to it.

If it does not exist, create it only when the debt is worth durable tracking. Otherwise keep the
observation in the checkpoint.

## Format

```md
# Tech Debt Register

| Date | Area | What | Why Deferred | Severity | Remediation |
|------|------|------|--------------|----------|-------------|
| 2026-04-09 | src/auth/session.ts | Missing rate limiting on token refresh | Out of scope for invitation ticket | high | Add rate limiter middleware and integration test |
```

## Severity

- **high**: could cause a bug, security issue, data issue, or repeated operational pain.
- **medium**: makes future work harder or slower.
- **low**: cleanup, cosmetic issue, or minor improvement.

## In The Checkpoint

When writing a checkpoint, include a count and summary:

```text
Tech debt logged: 2 entries added to docs/tech-debt.md (1 high, 1 low).
```

This gives the next session awareness without loading the full register.
