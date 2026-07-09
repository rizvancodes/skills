# Debugging Protocol

Loaded when `execute` enters autonomous fix mode after a test, typecheck, build, or runtime
failure caused by code just written.

## Stop-The-Line Rule

When something unexpected happens:

1. Stop adding features or further changes.
2. Preserve evidence: command, output, exit code, changed state.
3. Diagnose using the triage checklist.
4. Fix the root cause, not the symptom.
5. Guard against recurrence with a test when practical.
6. Resume only after verification passes.

Do not push past a failing check to work on the next step.

## Triage Checklist

### 1. Reproduce

Make the failure happen reliably.

- For tests, run the specific failing test if possible.
- For typecheck, run the exact typecheck command and read the cited locations.
- For intermittent failures, gather more context before fixing.

### 2. Localise

Identify the layer:

```text
Type error      -> read the error, inspect types at the cited line
Import error    -> check module path and exports
Test assertion  -> decide whether the test or code is wrong
Runtime error   -> trace data flow to the bad value
Build error     -> check config, dependencies, and environment
```

### 3. Reduce

Strip to the minimal failing case. Remove unrelated variables until the cause is visible.

### 4. Fix Root Cause

Fix the underlying issue, not the symptom.

```text
Bad:  swallow the error
Good: fix the invalid state that causes the error
```

Make one change at a time.

### 5. Guard

Write or adjust a test that catches this failure when practical.

The guard should fail without the fix and pass with it.

### 6. Verify

Run the relevant narrow check first, then the broader check required by the plan.

## Attempt Limit

- Attempts 1-2: autonomous fixing is allowed for code just written.
- Attempt 3: stop and escalate.

Escalation phrasing:

```text
I have attempted two fixes for [problem]. Each points to [summary]. This may be a plan or
architecture issue rather than a local implementation bug. Do you want to reassess?
```

Do not attempt a fourth fix without user input.

## Red Flags

- Guessing and trying changes without a theory.
- Fixing without reproducing.
- Multiple changes at once.
- Quick fix now, investigate later.
- Blaming the toolchain before reading the full error.
