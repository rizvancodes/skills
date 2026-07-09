# Workflow Model

This package provides a lightweight workflow for agent-assisted software work. It supports small
tasks and larger epics without forcing every change through the same amount of ceremony.

## Core Aim

Provide enough structure for reliable agent work without turning every change into a heavyweight
process:

- runtime packets with enough context for the active LLM session
- explicit assumptions and scope boundaries
- codebase context before design
- checkpoints
- fresh review
- post-runtime promotion of stable knowledge

Keep durable knowledge easy to recover:

- stable language belongs in `LANGUAGE.md`, not buried in runtime plans/checkpoints
- small work can use compact task briefs instead of epic ceremony
- planning splits by actual owner, runtime, package, service, app, or module
- each idea should have one primary source of truth

## Work Item Sizes

```text
task = small local work packet
epic = larger product or architecture capability
```

Default roots:

```text
docs/work/tasks/<slug>/
docs/epics/<slug>/
```

## Work Item IDs

Work item IDs are stable local handles for coordination across docs, branches, commits, PRs,
Herdr panes, and external ticket systems. They are project-specific, not skill-specific.

Skills must not hard-code a prefix. Repositories that want durable IDs should define the convention
in `AGENTS.md`, `docs/work/config.md`, or an equivalent project-local file. If no convention exists,
skills should use the slug-only default paths and ask before creating the first project ID prefix.

Recommended convention:

```text
<PREFIX>-<number>             durable local work item ID
<prefix>-<number>-<slug>/     artifact folder slug
<prefix>-<number>/<slug>      branch name
```

Example:

```text
ABC-48
docs/epics/abc-48-course-player-navigation/
abc-48/course-player-navigation
```

Project config example:

```md
# Work Config

## Work Item IDs

This repo uses:

- Prefix: `ABC`
- Format: `ABC-<number>`
- Allocation: manual
- Epic folders: `docs/epics/abc-<number>-<slug>/`
- Task folders: `docs/work/tasks/abc-<number>-<slug>/`
- Branches: `abc-<number>/<slug>`

External tracker IDs are recorded as links or metadata. They do not replace the local work item ID.
```

The same shape is available as [`templates/work-config.md`](templates/work-config.md).

Project config controls local IDs, folder naming, branch naming, allocation, and external tracker
references. It does not rename workflow artifacts. Filenames inside task and epic roots are the
workflow contract.

External tracker IDs are references, not replacements for the local work item ID:

```text
Local ID: ABC-48
External references:
  - JIRA-12345
  - GitHub #82
```

If a raw external ticket becomes a local epic or task, record both IDs in the artifact.
Do not encode external tracker prefixes as the reusable workflow convention.

Expected shapes:

```text
docs/work/tasks/<slug>/
  checkpoint.md
  brief.md
  plan.md
  review.md
  finish.md
  spikes/
  functional-tests/

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

## Runtime Docs And Durable Knowledge

Runtime docs help the current work finish. Durable docs help future work start correctly.

Language is always separate because it has a distinct job: fast human and LLM orientation. This is
inspired by Domain-Driven Design's ubiquitous language: shared terms should be precise enough for
humans, code, docs, and agents to use consistently.

```text
LANGUAGE.md
  Shared vocabulary: canonical terms, avoid terms, relationships, flagged ambiguities.

DOMAIN.md
  Scoped domain and architecture doctrine: model, ownership, boundaries, lifecycle, invariants,
  accepted patterns, local principles, and anti-patterns.

ADR
  Significant decision record: context, options, decision, consequences.
  Use only when the "why" is substantial, surprising, controversial, hard to reverse, or likely
  to be reopened.

Rules / AGENTS
  Agent operating law and enforcement posture: repo/package defaults, required checks,
  context-loading expectations, review posture, and hard constraints.

checkpoint.md
  Session continuity: what changed, what is incomplete, what was decided, and the exact next action.

finish.md
  Completion evidence: why the task or epic is considered done.
```

Principle:

```text
Write once in the most appropriate durable place.
Duplicate only when the duplicate has a different operational job.
```

Different jobs:

- `LANGUAGE.md` improves naming, search, human alignment, and LLM vocabulary routing.
- `DOMAIN.md` explains scoped doctrine beside the code.
- ADRs preserve decision history and tradeoffs.
- Rules / AGENTS change agent behavior while editing.

`DOMAIN.md` and rules can overlap when the same idea needs explanation and enforcement. Prefer
`DOMAIN.md` for local doctrine and use rules/AGENTS for broader operating policy or hard edit-time
constraints.

## Skill Map

Current skills:

```text
open-session
  Read-only state loader for task/epic work.
  Loads checkpoint, project context, relevant vocabulary, git state, and recommends next action.

gather-requirements
  Conversational clarification loop for rough work.
  Interviews for intent, scope, assumptions, constraints, and scenarios without writing files.

create-task
  Produces a compact task brief for narrow standalone work.

create-epic
  Initializes an epic workspace and writes requirements plus scenarios.

architect-solution
  Produces architecture and model artifacts for approved epic requirements when technical design is
  needed before planning.

spike
  Focused discovery from a spike brief.
  Reads code/docs, validates claims, traces behavior, or builds throwaway prototypes.
  Writes evidence-backed findings for the paused parent workflow to consume.

plan-work
  Turns approved task briefs or epic artifacts into execution-ready plans.
  Carries rules, language, domain constraints, anti-patterns, validation, file paths, and
  verification commands into per-task Context blocks.

execute
  Executes one approved plan unit: a task plan or epic phase.
  Trusts per-task Context blocks, keeps scope tight, verifies before claims, classifies failures,
  and stops after a slim self-review.

review-code
  Fresh code review for completed execution work or ad hoc diffs.
  Judges the diff against the executed plan, source artifact, project rules, optional durable
  language/domain docs, quality heuristics when useful, and actual code.

functional-test
  Creates and runs functional test cases for implemented task or epic behavior.
  Writes functional-tests/cases.md and per-run reports under functional-tests/runs/.

checkpoint
  Writes durable workflow state at the task or epic root.
  Captures progress, commits, checks, decisions, deviations, blockers, knowledge candidates, and
  the next skill plus exact input path.

freeze-knowledge
  Promotes stable runtime knowledge into LANGUAGE.md, DOMAIN.md, ADRs, or rules.

finish-work
  Final close step for a task or epic.
  Checks source artifact, plan, execution, review, verification, git state, blockers, and durable
  knowledge decisions. Writes finish.md and marks epic status complete when applicable.
```

## Requirements, Spikes, Architecture, Planning

Requirements gathering and artifact creation are not planning.

```text
gather-requirements
  Decide what the work is and what outcome matters through conversation.
  Establish objective, language, scope, assumptions, open questions, and out-of-scope.
  Output: no files.

create-task
  Turn clear narrow work into docs/work/tasks/<slug>/brief.md.

create-epic
  Turn clear larger work into docs/epics/<slug>/requirements.md and scenario files.

architect-solution
  Turn approved epic requirements into architecture.md and models/*.md when technical design is
  needed.

spike
  Optional focused discovery created when one uncertainty blocks the parent workflow.
  Read code, validate docs, trace runtime behavior, or build a throwaway prototype.
  Output: an evidence-backed answer, not production code.
  The parent workflow waits for this answer before writing or updating its artifact.

plan-work
  Turn approved artifacts and spike findings into execution slices.
  Output: task plan, epic plan index, or phase plan.
```

## Workflow Principles

- Codebase context before artifact creation or architecture
- Vocabulary before design
- Explicit assumptions
- "What this is not"
- Source-checkable validation
- Scope decomposition
- No implementation before accepted shape/plan
- Runtime packets for execution
- Checkpoints
- Fresh review
- Functional evidence when runtime behavior needs proof
- Freezing stable knowledge after runtime work
- Final completion evidence before a task or epic is marked done

## Source Validation

There is no standalone source-verification skill by default. Validation is embedded in the skills
that need it:

- gather-requirements and architect-solution verify source-checkable claims before they become
  artifacts.
- plan-work verifies framework, API, and library assumptions before they become tasks.
- execute checks current docs before coding against unfamiliar or version-sensitive APIs.
- review-code flags unverified claims that affected implementation.

Use Context7 first when it has the relevant official docs. Use web search as fallback, restricted
to official docs, changelogs, standards references, and compatibility tables.

## Workflow Influences

- Smaller skill boundaries
- Ubiquitous language as durable `LANGUAGE.md`
- Throwaway prototypes that answer one question
- Handoffs that reference existing artifacts instead of duplicating them
- Vertical slices / tracer bullets
- Avoiding stale implementation details in high-level briefs/specs
