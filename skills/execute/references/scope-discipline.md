# Scope Discipline

Loaded at execution setup. Applies to every task.

## Rule

Touch only what the current task requires.

## Do Not

- Clean up adjacent code because you noticed it.
- Refactor imports in files you are not modifying.
- Remove comments you do not fully understand.
- Add features outside the current execution contract because they seem useful.
- Modernize syntax in files you are only reading.
- Build abstractions before the third use case demands them.
- Create utility files for one-time operations.

## Simplicity Check

Before writing code, ask:

```text
What is the simplest thing that could work?
```

After writing code, check:

- Can this be done in fewer lines?
- Are these abstractions earning their complexity?
- Would a senior engineer ask why this was not simpler?
- Am I building for the current execution contract or a hypothetical future requirement?

Three similar lines of code are better than a premature abstraction.

## Noticed But Not Touching

When you spot something worth fixing outside the task scope, do not fix it.

Record it in the execution summary and, when useful, in the project tech debt register:

```text
NOTICED BUT NOT TOUCHING:
- src/utils/format.ts has an unused import unrelated to this task.
- The auth middleware could use clearer error messages as separate work.
```

## Increment Rules

Each task should leave the system in a working state:

- One logical change per task.
- Build and existing tests should pass after the task when relevant commands exist.
- Commit after each verified task.
- Each commit should be independently revertable.
- Use conventional commit types when the repo uses them: `feat:`, `fix:`, `refactor:`, `test:`,
  `chore:`.
- Commit messages should explain the task outcome clearly.

## Change Summary

After each task commit, report a short summary:

```text
CHANGES MADE:
- src/routes/tasks.ts: Added validation middleware to POST endpoint.
- src/lib/validation.ts: Added TaskCreateSchema.

DID NOT TOUCH:
- src/routes/auth.ts: Similar validation gap, unrelated to current task.

CONCERNS:
- The schema rejects extra fields. Confirm this is desired if not specified.
```

## Dead Code Hygiene

After changes, check for orphaned code created by your work.

If you find unreachable or unused code:

- List it explicitly.
- Ask before deleting anything you are unsure about.
- Do not silently delete unrelated code.

## Red Flags

- More than 100 lines written without running a relevant check.
- "Let me quickly add this too."
- Touching files outside the task file list.
- Building abstractions before the third use case.
- Uncommitted changes accumulating across multiple tasks.
