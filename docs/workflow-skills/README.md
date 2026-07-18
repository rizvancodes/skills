# Agent Workflow Skills

An agent workflow for turning rough conversations into finished software without losing the
knowledge discovered along the way.

The package is built around three ideas:

- **Shape the work before coding.** Clarify intent, boundaries, language, and acceptance criteria.
- **Carry context to the point of use.** Plans include the rules, domain terms, assumptions, files,
  checks, and commit cadence needed by the executor.
- **Separate runtime state from durable knowledge.** Checkpoints preserve the current session.
  `LANGUAGE.md`, `DOMAIN.md`, ADRs, and rules preserve knowledge future humans and agents should
  reuse.

## The Workflow In 60 Seconds

Use the smallest local work item that fits:

```text
task    = small local work packet
epic   = larger product or architecture capability
```

Then follow the lifecycle:

```text
open-session
  -> gather-requirements   (when the work shape is unclear)
  -> create-task | create-epic
      -> architect-solution (when technical design is needed)
      -> spike            (optional, for one blocking uncertainty)
      -> plan-work
          -> execute
              -> review-code
                  -> functional-test   (when runtime functional verification is needed)
                  -> freeze-knowledge   (when stable knowledge was discovered)
                      -> finish-work
```

`checkpoint` can happen at any session boundary. `finish-work` is the terminal close step.
For one clear, bounded implementation request, `execute` can also be entered directly without
creating a task brief or plan. Use `plan-work` when accepted work needs durable decomposition,
sequencing, or coordination.

## Skill Map

| Skill | Use When | Output |
| --- | --- | --- |
| `open-session` | Starting or resuming work; asking where things stand | Read-only state summary and next recommended skill |
| `gather-requirements` | Interviewing the user to clarify intent, scope, constraints, assumptions, and scenarios | Conversation only; no files |
| `create-task` | Turning clear narrow work into a compact task brief | `brief.md` under `docs/work/tasks/` |
| `create-epic` | Turning clear larger work into durable epic requirements and scenarios | `requirements.md` and `scenarios/` under `docs/epics/` |
| `architect-solution` | Designing the technical shape for an approved epic before planning | `architecture.md` and optional `models/*.md` |
| `spike` | One uncertainty blocks artifact creation, architecture, or planning | Evidence-backed findings for the paused workflow |
| `plan-work` | An approved brief, requirements, or architecture artifact needs executable tasks | `plan.md`, plan index, or phase files |
| `execute` | Implementing one clear bounded change directly, or running an approved task plan or epic phase | Code changes, verification evidence, commits |
| `review-code` | Completed implementation needs fresh review before merge/finish | `review.md` or `reviews/phase-N.md` |
| `functional-test` | Completed behavior needs a runtime functional pass | `functional-tests/cases.md` and `functional-tests/runs/<run-id>.md` |
| `freeze-knowledge` | Stable language, domain facts, decisions, or rules should survive the runtime docs | `LANGUAGE.md`, `DOMAIN.md`, ADRs, or rules |
| `finish-work` | A task or epic is ready to be closed as complete | `finish.md`, final status/checkpoint updates |
| `checkpoint` | Ending a session, saving progress, or preserving a resume point | `checkpoint.md` |

## Agents And Personas

Skills are workflows. Agents or personas are specialist perspectives.

Use agents when a repo has knowledge-specific roles that improve the work, such as a backend
agent, frontend agent, security reviewer, test reviewer, or domain specialist. They are useful for
loading ownership rules, vocabulary, stack conventions, and review perspective.

Agents are optional support, not a required package dependency:

- `gather-requirements`, `create-task`, `create-epic`, `architect-solution`, and `plan-work` may
  read relevant local agent files for domain language and constraints.
- `review-code` may use a fresh specialist agent for independent review.
- `execute` remains inline by default; do not delegate implementation just because an agent exists.

The distinction:

```text
skill  = what process should run
agent  = which specialist knowledge or perspective should inform it
```

## Work Item Folders

```text
docs/work/tasks/<slug>/
  brief.md
  plan.md
  review.md
  spikes/
  functional-tests/
  checkpoint.md
  finish.md

docs/epics/<slug>/
  status.md
  checkpoint.md
  finish.md
  requirements.md
  scenarios/
  architecture.md
  models/
  plans/
  reviews/
  spikes/
  functional-tests/
```

These roots are defaults. Repositories can adapt task/epic root naming, but workflow artifact names
inside each root are fixed so agents can find the source of truth consistently.

Projects that want durable local work item IDs should define them in `docs/work/config.md` and
point to that file from `AGENTS.md`. The skills should use that project-local convention, for
example `ABC-48` with folders like `docs/epics/abc-48-<slug>/`. External tracker IDs are
recorded as references; they do not replace the local work item ID. See
[`templates/work-config.md`](templates/work-config.md) for the suggested config shape.

## Runtime Docs And Durable Knowledge

Runtime docs help the current work finish. Durable docs help future work start correctly.

```text
LANGUAGE.md
  Shared vocabulary for an area of the codebase. Records canonical terms, avoid terms,
  relationships, and ambiguities. Inspired by Domain-Driven Design's ubiquitous language.

DOMAIN.md
  Scoped domain and architecture doctrine for an area. Records the model, ownership, boundaries,
  invariants, accepted patterns, local principles, and anti-patterns. It may overlap with rules
  when the same doctrine needs both explanation and enforcement.

ADR
  Decision history. Records context, options, decision, and consequences for significant choices.

Rules / AGENTS
  Operating instructions for agents. Records repo/package-wide defaults, required checks,
  context-loading expectations, review posture, and hard constraints that agents must obey while
  editing.

checkpoint.md
  Session continuity. Records current state, incomplete work, commits, checks, decisions,
  deviations, blockers, and the next exact action.

finish.md
  Completion evidence. Records why the work item is considered done.
```

Write once in the best durable home. Duplicate only when the duplicate has a different job.

`DOMAIN.md` and rules are allowed to overlap. The distinction is operational:

```text
DOMAIN.md      = scoped doctrine beside the code
Rules / AGENTS = agent operating law and enforcement posture
```

For example, `DOMAIN.md` can explain the auth and tenancy model. A rule or `AGENTS.md` entry can
turn the same idea into an editing constraint: never accept caller-provided `orgId` when
authenticated context provides tenant identity.

## Source Validation

There is no standalone source-verification skill by default. Validation is embedded where it is
needed:

- `gather-requirements` and `architect-solution` verify source-checkable claims before they become
  artifacts.
- `plan-work` verifies framework, API, and library assumptions before they become execution steps.
- `execute` checks current docs before coding against unfamiliar or version-sensitive APIs.
- `review-code` flags unverified claims that affected implementation.

Use Context7 first when it has the relevant official docs. Use web search as a fallback, limited to
official documentation, changelogs, standards references, or compatibility tables.

See [references.md](references.md) for externally checkable research and method references behind
the public workflow language.

## Example Flows

### Small Task

```text
User: "Rename this confusing helper and update the tests."
open-session -> create-task -> plan-work -> execute -> review-code? -> finish-work
```

For a narrow task, `create-task` writes the compact `brief.md`. Review may be skipped if no
production behavior changed. If behavior changed, use `review-code` before `finish-work`.

### External Ticket To Task

```text
User: "Add retry handling to the checkout flow."
gather-requirements? -> create-task -> spike? -> plan-work -> execute -> review-code
  -> freeze-knowledge? -> finish-work
```

An external ticket, issue, or support request can become a local task brief. The task brief captures
acceptance direction. The plan creates a vertical slice. Execution commits after verified tasks.
Review decides whether it is ready to finish.

### Epic

```text
User: "Design the live response aggregation capability."
gather-requirements -> create-epic -> architect-solution? -> spike? -> plan-work
          -> execute phase 1 -> review-code
          -> execute phase 2 -> review-code
          -> freeze-knowledge -> finish-work
```

Epic requirements and scenarios own product behavior and boundaries. Architecture and models own
technical shape. Phase plans own execution. `finish-work` checks that phases, reviews,
verification, and durable knowledge decisions are complete.
For epic work, `create-epic` initializes the epic workspace before writing requirements and
scenarios.

## What This Package Is Not

- Not a replacement for project rules. Keep repo-specific constraints in `AGENTS.md` or equivalent.
- Not a project-management system. It creates lightweight artifacts that help agents work reliably.
- Not a mandatory epic pipeline for every change. Use tasks for small standalone work, including
  external tickets or issues that do not need an epic workspace.
- Not a general engineering encyclopedia. Add specialist skills only when the workflow needs them.
