---
name: review-code
description: |
  Fresh code review for completed execution work. Use after execute completes a task plan or
  epic phase; before merging; after ad hoc fixes; or when the user says "review this",
  "code review", "check my code", "review the diff", or "does this look right". Builds a
  project-primed review request from the executed plan, source artifact, rules, language/domain
  context when present, and the actual diff/commits. Separates Contract findings from Quality
  findings, with Critical/Important/Minor severity inside each axis and file/line references. Do
  NOT use for requirements gathering, artifact creation, planning, implementation, spike discovery,
  or fixing the issues it finds unless the user explicitly asks for a separate fix pass.
---

# Review Code

Review completed code changes from a fresh-review stance.

The reviewer judges the actual diff on two axes:

- **Contract:** did the implementation build the right thing?
- **Quality:** was it built well?

Review does not fix. Review findings are the output.

<HARD-GATE>
Do NOT fix issues during review.
Do NOT rewrite the implementation.
Do NOT rubber-stamp without evidence.
Do NOT review from implementer memory.
Do NOT block review just because LANGUAGE.md or DOMAIN.md is missing.
</HARD-GATE>

## Progressive Disclosure Rule

Checkpoint/status may identify the likely plan path, commit range, or resume state, but they are
not review evidence.

Before judging Contract or Quality, load the owning evidence for the claim: executed plan, source
artifact, relevant scenarios/models/rules, actual diff/commits, changed files, and verification or
functional-test reports when relevant. If an owning artifact is unavailable or intentionally not
loaded, state the limitation rather than assuming the claim is true.

## Operating Vocabulary

**Review Quality:** fresh reviewer, adversarial review, two-axis review, contextual finding,
severity classification, rubber-stamp detection, production readiness, ship/no-ship verdict
**Review Context:** executed plan, source artifact, task Context block, source contract, plan
contract, diff range, commit range, path-derived work kind
**Project Fit:** rules, local instructions, language, domain model, anti-patterns, dependency
direction, contract boundaries
**Validation Discipline:** source-verified facts, unresolved assumption drift, evidence before
clearance, test gap, residual risk

## Context Contract

Load review context in this order:

1. **Executed plan**
   The immediate contract for what `execute` was supposed to do.

2. **Source artifact**
   - task: `docs/work/tasks/<slug>/brief.md`
   - epic: `docs/epics/<slug>/architecture.md` when present, otherwise `requirements.md`

3. **Scenarios**
   Only for epic work when journeys, handoffs, entry paths, progression, permissions,
   resumptions, or terminal outcomes matter.

4. **Plan Context blocks**
   Rules, language, domain constraints, anti-patterns, validation findings, and task-local
   assumptions embedded by `plan-work`.

5. **Project rules and instructions**
   Project `AGENTS.md`, local `AGENTS.md`, and relevant `.agents/rules`.

6. **Durable knowledge, if present**
   Relevant `LANGUAGE.md` and `DOMAIN.md`. Missing durable knowledge does not block review.

7. **Diff/commits**
   The actual code under review.

The plan is the immediate execution contract. The source artifact is the design/product contract.
Durable knowledge is extra orientation when it exists.

## Output Paths

Resolve the review output path from the plan path when possible:

```text
docs/work/tasks/<slug>/plan.md
  -> docs/work/tasks/<slug>/review.md

docs/epics/<slug>/plans/phase-N.md
  -> docs/epics/<slug>/reviews/phase-N.md
```

For ad hoc diffs, write a review artifact only if the user asks or a work item is obvious.

## Anti-Pattern Watchlist

### Generic review
- **Detection:** review uses only a diff and generic code-quality criteria
- **Why:** generic review misses project-specific rules, language, scope boundaries, and
  anti-patterns
- **Resolution:** build a project-primed review request from plan, source artifact, rules, and
  relevant context

### Self-review masquerading as review
- **Detection:** the implementing agent reviews from memory of what it wrote
- **Why:** implementation context creates confirmation bias
- **Resolution:** use a fresh reviewer/subagent when available. If unavailable, explicitly state
  that this is an inline fallback and review from artifacts and diff only

### Rubber-stamp
- **Detection:** verdict is ship with no issues and no specific file/line evidence
- **Why:** empty approval is a common review failure
- **Resolution:** redispatch or rerun review once with stronger adversarial framing. If still no
  issues, require evidence for clearance

### Scope-creep review
- **Detection:** review asks for new features, broad redesigns, or unrelated refactors
- **Why:** review evaluates the change against its contract, not a new imagined project
- **Resolution:** mark broader ideas as non-blocking recommendations

### Nitpick avalanche
- **Detection:** review focuses on style while missing correctness, scope, contracts, or tests
- **Why:** cosmetic issues can mask real risk
- **Resolution:** order review by severity: correctness and contract first, style last

### Abstract finding
- **Detection:** a review comment says code is "unclear", "wrong", "too complex", or "risky"
  without tying it to product behavior, source-contract context, user workflow, domain invariant,
  or project rule
- **Why:** abstract findings force the user to ask why the issue matters
- **Resolution:** every finding must include the concrete product/source/workflow context that
  makes it actionable

### Axis collapse
- **Detection:** contract failures and quality concerns are merged into one generic issue list
- **Why:** good code can implement the wrong thing, and correct behavior can still be unsafe,
  brittle, or architecturally wrong
- **Resolution:** keep Contract findings and Quality findings separate. Apply
  Critical/Important/Minor severity inside each axis

### Missing source contract
- **Detection:** review ignores the task brief or epic artifact
- **Why:** the diff can look good while failing the actual requested behavior
- **Resolution:** compare implementation to both executed plan and source artifact

### Checkpoint-only evidence
- **Detection:** review accepts checkpoint/status claims about completed work, verification,
  commits, or behaviour without checking the plan, diff, source files, review artifacts, or run
  reports that own those claims
- **Why:** checkpoint/status is a navigation layer, not proof
- **Resolution:** use checkpoint/status only to locate evidence, then verify against the owning
  artifact or git state

### Rule drift
- **Detection:** code violates AGENTS/rules but the review only checks the plan
- **Why:** plans can stale or omit constraints
- **Resolution:** read relevant project/local rules and include them as review questions

## Behavioural Instructions

### Phase 1: Resolve Review Scope

1. Determine review mode:
   - plan-based review
   - commit-range review
   - working-tree diff review
   - ad hoc path review

2. Prefer a plan path when supplied or inferable:
   - `docs/work/tasks/*/plan.md`
   - `docs/epics/*/plans/phase-*.md`

3. Infer work kind from the plan path:
   - task plan -> task
   - epic phase plan -> epic phase

4. Determine diff range:
   - Use commits created by `execute` when the plan/checkpoint lists them.
   - Else use the user-supplied range.
   - Else use merge-base with main/origin main.
   - Else review the working tree diff.
   - If no diff can be determined, ask for the range.

   If checkpoint lists commits, verify that those commits exist and match the selected work item
   before treating them as the review range.

5. Get diff stats and changed files.
   Categorize risk:
   - schemas/contracts
   - auth/security/permissions
   - migrations/data
   - cross-runtime messages
   - public APIs
   - UI flows
   - tests only

### Phase 2: Load Review Context

6. Read the executed plan. Extract:
   - execution unit scope
   - done-when criteria
   - tasks and expected files
   - task Context blocks
   - validation findings
   - unresolved gates
   - commit cadence or produced commits

7. Read the source artifact:
   - task brief or epic artifact as determined by the plan path.
   - If the plan is ad hoc and no source artifact exists, continue with the plan/diff and note the
     limitation.

8. For epic work, read relevant scenario files only when the changed files or plan touch journey,
   progression, handoff, permission, or terminal-outcome behavior.

9. Read project `AGENTS.md` and local `AGENTS.md` for changed file owners.

10. Read relevant `.agents/rules`.
    Prefer path-scoped matches against changed files. If no path scope exists, match by content
    relevance. Include only rules that affect the diff.

11. Read relevant `LANGUAGE.md` and `DOMAIN.md` if present.
    Missing files are fine. Fall back to source artifact language/domain sections and plan Context
    blocks.

12. Load quality heuristics when useful.
    If project rules do not already cover maintainability/design-quality concerns for the diff,
    read `references/quality-heuristics.md` and include the relevant heuristics in the review
    request. Project rules override the heuristic baseline. Heuristics are judgement calls, not
    automatic violations, and should be reported only with concrete file/hunk evidence.

    If the executed plan or task Context blocks name installed technical skill dependencies, load
    only those relevant to the changed files and use them as generic quality guidance. Project
    rules, accepted ADRs, and the source contract remain authoritative when they conflict.

13. Read relevant changed files and nearby code as needed to understand the diff.

Output: review context summary with scope, diff range, source artifact, risk areas, and criteria.

### Phase 3: Build Review Request

Assemble a review request for a fresh reviewer. If subagent tooling is available, send this request
to a fresh reviewer. If not, use the same request as the inline review rubric and state the fallback.

Before writing the request, read `references/templates.md` and use the Review Request Template.
Populate every section with the loaded context. Keep the Contract and Quality axes separate.

### Phase 4: Review And Rubber-Stamp Check

14. Run the review using a fresh reviewer/subagent when available.

15. Check for rubber-stamp:
    - verdict is `ship`
    - no Contract or Quality critical/important issues
    - no specific file/line evidence
    - generic praise such as "looks good"

16. If rubber-stamp is detected, rerun once with stronger instruction:
    Use the Rubber-Stamp Recheck Text in `references/templates.md`.

Do not loop more than once.

### Phase 5: Write And Report

17. Write the review artifact when a work item path exists or the user asked for one.
    Use the Review Artifact Template in `references/templates.md`.

18. Report findings to the user with findings first:
    Use the User Report Template in `references/templates.md`.

19. Do not fix issues unless the user explicitly asks for a separate fix pass.

## Severity

Apply severity inside each axis.

Contract findings answer whether the code built the right thing:

- **Critical:** clear failure to meet required behavior, broken required flow, excluded behavior
  implemented, failing required checks, or required data/state outcome wrong.
- **Important:** missing acceptance criterion, contract drift, meaningful scope ambiguity, or
  significant missing test coverage for required behavior.
- **Minor:** non-blocking contract note, small documentation mismatch, or follow-up that does not
  block the current artifact.

Quality findings answer whether the code was built well:

- **Critical:** security or permissions break, data loss or corruption, broken state invariant,
  unsafe external data handling, or a quality flaw that makes the implementation unsafe to ship.
- **Important:** architecture or ownership violation, wrong dependency direction, brittle test seam,
  significant maintainability problem, meaningful performance issue, or unnecessary abstraction in
  touched code.
- **Minor:** local naming/readability issue, small convention miss, non-blocking documentation or
  cleanup, or future improvement suggestion.

## Error Handling

- **No diff:** ask for a commit range or changed files.
- **No source artifact:** continue with plan/diff and report the limitation.
- **No durable LANGUAGE/DOMAIN:** continue; use runtime artifact language and plan Context blocks.
- **No subagent tooling:** perform an inline fallback review and state that reviewer isolation was
  not available.
- **Plan/source conflict:** surface the conflict before judging implementation.
- **Review finds implementation should change scope:** classify as recommendation unless the
  current artifact requires it.

## What This Is Not

- **Not implementation** - does not fix issues during review
- **Not planning** - does not create the fix plan unless separately asked
- **Not requirements gathering or artifact creation** - does not redefine product scope
- **Not durable knowledge promotion** - can identify candidates, but does not update durable docs
- **Not a rubber stamp** - clearance requires evidence

## Key Principles

- **Fresh review stance** - do not rely on implementer memory
- **Findings first, with context** - lead with defects and explain the product/source context
- **Separate contract from quality** - right thing and well-built are different questions
- **Contract before style** - plan/source compliance beats generic polish
- **Project-specific questions** - rules and anti-patterns become review checks
- **Heuristics as fallback** - generic quality smells help only where project rules are silent
- **Evidence for clearance** - no empty ship verdicts
- **Review and fix are separate** - keep findings uncontaminated
