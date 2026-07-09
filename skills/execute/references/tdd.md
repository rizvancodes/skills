# Test-Driven Development

Loaded when the selected execution unit includes test files, test steps, feature work, bug-fix
work, refactoring, or behavior changes.

## Iron Law

```text
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

Write code before the test? Delete it. Start over.

No exceptions without explicit user permission:

- Do not keep early production code as reference.
- Do not adapt early production code while writing tests.
- Do not look at it while writing the test.
- Delete means delete.

Allowed exceptions require user agreement:

- throwaway prototypes
- generated code
- configuration files
- static content with no behavioral impact

Thinking "skip TDD just this once" is a rationalisation. Stop.

## Red-Green-Refactor

### Red

Write one minimal failing test showing the behavior.

Requirements:

- One behavior.
- Clear name describing the behavior.
- Real code where practical; mocks only when unavoidable.
- One concept per test.

Run the test. Confirm:

- it fails
- it fails for the expected reason
- it fails because the behavior is missing, not because of a typo or broken setup

If the test passes immediately, it is testing existing behavior or the wrong thing. Fix the test.
If the test errors, fix the test/setup and rerun until it fails correctly.

### Green

Write the simplest code that makes the test pass.

Do not:

- add behavior beyond what the test requires
- refactor unrelated code
- over-engineer options or configuration the test does not need
- improve neighboring code outside the task

### Refactor

With tests green:

- remove duplication
- improve names
- extract helpers only when warranted

Keep tests green throughout. Do not add behavior during refactor.

### Repeat

Write the next failing test for the next behavior.

## Bug Fix Pattern

For a reported bug:

1. Write a test that demonstrates the bug.
2. Run it and confirm it fails.
3. Implement the fix.
4. Run it and confirm it passes.
5. Run the relevant broader suite.

The test proves the fix works and guards against regression.

Never fix bugs without a test unless the user explicitly accepts the exception.

## Test Double Preference

Use the simplest double that gives useful confidence:

1. Real implementation.
2. Fake in-memory implementation.
3. Stub returning canned data.
4. Mock for interaction verification.

Use mocks only when the real dependency is too slow, non-deterministic, external, or side-effectful.
Over-mocking creates tests that pass while production breaks.

## Good Tests

- Test outcomes and state, not incidental interactions.
- Prefer DAMP over DRY in tests. Each test should read as a self-contained story.
- Use Arrange / Act / Assert.
- If a test name needs "and", split it.
- Tests should demonstrate the desired API and behavior.

## Test Sizes

| Size | Constraints | Speed | Expected Use |
|------|-------------|-------|--------------|
| Small | Single process, no I/O, no network, no DB | Milliseconds | Most tests |
| Medium | Multi-process OK, localhost only | Seconds | Some tests |
| Large | External services allowed | Minutes | Few critical paths |

## Why Order Matters

Tests written after code usually prove less:

- they are biased by the implementation
- they may test what was built, not what was required
- they can pass immediately without proving the test catches the missing behavior
- they encourage testing implementation details

Tests first answer "what should this do?" before code exists.

## Common Rationalisations

| Rationalisation | Reality |
|-----------------|---------|
| "Too simple to test" | Simple code breaks. Add the small test. |
| "I will test after" | Tests passing immediately prove little. |
| "I manually tested it" | Manual checks are not repeatable. |
| "Need to explore first" | Fine. Throw away exploration, then start with TDD. |
| "Keep the code as reference" | You will adapt it. Delete means delete. |
| "Test is hard" | Hard to test often means hard to use. Reconsider the seam. |
| "Being pragmatic means adapting" | TDD is the pragmatic path for behavior changes. |

## Red Flags

Stop and restart with TDD if you notice:

- production code before test
- test added after implementation
- test passes immediately
- cannot explain why the test failed
- rationalising "just this once"
- manual testing offered as replacement
- keeping pre-test implementation as reference
- "spirit not ritual" argument

## Verification Checklist

Before marking behavior complete:

- [ ] Test written before production behavior.
- [ ] Test was run and failed for the expected reason.
- [ ] Minimal implementation made it pass.
- [ ] Relevant broader tests pass.
- [ ] Output is read before claiming success.
- [ ] Mocks are only used where justified.
- [ ] Edge cases and errors required by the plan are covered.

If these boxes cannot be checked, say so. Do not claim TDD was followed.
