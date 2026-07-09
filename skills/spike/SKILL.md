---
name: spike
description: |
  Run focused discovery to answer one blocking uncertainty for a task, epic, architecture decision,
  model, or plan. Use when the user asks to run a spike, answer a spike question, investigate an
  uncertainty, validate a source-checkable claim, trace existing behavior, or build a throwaway
  prototype to unblock requirements, architecture, models, or planning. Produces evidence-backed
  findings in a spike brief. Do NOT use for production implementation, broad planning, review,
  functional testing, or durable knowledge promotion.
---

# Spike

Answer one blocking uncertainty with the smallest useful exploration.

A spike is temporary discovery. It may read code, validate official docs, trace runtime behavior, or
build a throwaway prototype. Its output is an evidence-backed answer that a paused parent workflow
can fold into a task brief, epic requirement, scenario, architecture doc, model file, or plan.

<HARD-GATE>
Do NOT implement production work.
Do NOT broaden beyond the spike question.
Do NOT write execution plans.
Do NOT edit the parent task, epic, architecture, model, or plan unless the user explicitly asks.
Do NOT promote durable knowledge directly unless the user explicitly asks.
Do NOT automatically invoke the parent workflow after writing findings.
</HARD-GATE>

## Artifact Layout

Prefer a spike brief under the parent work item root:

```text
docs/work/tasks/<task-slug>/spikes/<spike-slug>.md
docs/epics/<epic-slug>/spikes/<spike-slug>.md
```

If project config defines different task or epic roots, use those roots and keep the fixed
`spikes/` artifact folder inside the parent work item root.

For an ad hoc spike with no work item root, write a temporary brief:

```text
$TMPDIR/workflow-spikes/<spike-slug>.md
```

Templates live in `references/templates.md`. Read that file before creating or updating a spike
brief.

## Input Contract

Prefer an explicit spike brief path or work item root.

If no path is supplied:

1. Look for relevant pending briefs in:
   - `docs/work/tasks/*/spikes/*.md`
   - `docs/epics/*/spikes/*.md`
   - `$TMPDIR/workflow-spikes/*.md`
   - `/tmp/workflow-spikes/*.md`
2. If exactly one pending brief is clearly relevant, use it.
3. If multiple briefs are plausible, ask which one to run.
4. If no brief exists, ask for or infer the single blocking question and create a minimal brief
   before exploring.

The brief must make one question primary. If source material contains several questions, choose the
blocking one and record the rest as out of scope or follow-up.

## Workflow

### 1. Load The Brief

Read the spike brief and extract:

- the question
- why it blocks the parent workflow
- known context
- relevant files and docs
- suggested exploration
- stop condition
- constraints and non-goals
- expected findings shape

If a brief does not exist yet, create the smallest useful brief from the current request before
exploring.

### 2. Load Minimal Context

Read only what is needed to answer the question:

- referenced files and docs
- relevant project instructions
- relevant `LANGUAGE.md`, `DOMAIN.md`, ADRs, or rules
- current code paths, tests, schemas, contracts, logs, or runtime docs

Use `rg` first for code and text search.

For source-checkable claims about current frameworks, APIs, libraries, browser behavior, platform
limits, standards, or cloud services, verify against current official docs before treating the
claim as evidence. If the source cannot be checked, mark the claim unresolved.

### 3. Explore

Use the lightest exploration that can satisfy the stop condition.

Common spike modes:

- **Code reading:** inspect existing implementation, contracts, tests, and ownership boundaries.
- **Source validation:** verify official docs, compatibility, or platform/library behavior.
- **Runtime trace:** run the smallest command or local flow that exposes current behavior.
- **Logic prototype:** create a tiny throwaway model, reducer, state machine, parser, or harness.
- **UI prototype:** create a throwaway visual or interaction variant only when layout, hierarchy, or
  interaction is the uncertainty.

For browser-facing runtime traces or UI prototypes, use Chrome DevTools MCP when available. If it is
not available, record the limitation instead of inventing browser observations.

Prefer scratch locations for prototypes:

```text
$TMPDIR/spikes/<spike-slug>/
/tmp/spikes/<spike-slug>/
```

Only modify the repo for a prototype when the question cannot be answered outside the app/runtime.
If repo changes are needed, keep them isolated, label them as spike-only, and report exactly what
must be deleted or absorbed.

### 4. Stop

Stop as soon as the stop condition is met.

Do not keep researching for completeness. Do not turn the spike into a design pass, implementation
plan, review, or functional test.

### 5. Write Findings

Update the spike brief in place when possible. If the brief is read-only, write a sibling findings
file:

```text
<spike-brief-name>.findings.md
```

Findings must include:

- direct answer
- evidence
- recommendation for the parent workflow
- impact on parent artifact
- residual risk
- scratch work and cleanup

Use current artifact names in `Impact On Parent Artifact`: task brief, epic requirements, scenario,
architecture, models, plan, `LANGUAGE.md`, `DOMAIN.md`, ADR, or rules.

### 6. Self-Review Findings

Before reporting, fix issues inline:

- **Single-question check:** findings answer the spike question, not adjacent questions.
- **Evidence check:** every recommendation is supported by a file, command, official source, trace,
  or prototype result.
- **Boundary check:** findings do not edit or rewrite the parent artifact.
- **Source check:** source-checkable claims are verified or marked unresolved.
- **Scratch check:** throwaway files, repo changes, commands, and cleanup are recorded.

### 7. Exit

End with:

- spike brief or findings path
- direct answer
- evidence summary
- recommendation for the parent workflow
- residual risk, if any
- scratch cleanup needed, if any
- explicit note that the parent workflow can resume from the findings

Do not invoke the next skill automatically.

## Anti-Patterns

### Research drift
- **Detection:** exploration continues after the stop condition is satisfied.
- **Resolution:** stop and write findings.

### Production by spike
- **Detection:** throwaway code becomes hidden implementation.
- **Resolution:** keep scratch work isolated and mark anything repo-local as spike-only.

### Evidence-free answer
- **Detection:** findings contain recommendations without file, command, source, or prototype
  evidence.
- **Resolution:** attach enough evidence for the parent workflow to trust the result.

### Scope expansion
- **Detection:** the spike answers adjacent product, architecture, or planning questions.
- **Resolution:** record adjacent questions as follow-ups, but answer only the brief.

### Parent artifact rewrite
- **Detection:** the spike updates requirements, scenarios, architecture, models, or plans directly.
- **Resolution:** write impact and recommendation in the spike findings; let the parent workflow
  decide how to apply them.
