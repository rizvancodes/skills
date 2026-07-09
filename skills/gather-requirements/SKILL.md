---
name: gather-requirements
description: |
  Interactive requirements-gathering interview for rough ideas, ambiguous asks, external tickets,
  epic candidates, task candidates, or designs that need clarification before any artifact is
  written. Use when the user wants to gather requirements, be interviewed or grilled, stress-test
  intent, clarify scope, identify scenarios, surface assumptions, or decide whether work should
  become an epic or a task. This is conversation-only: it asks questions, verifies facts, and stops
  when the shape is clear enough for the user to choose create-epic, create-task, spike, or no
  artifact. Do NOT use for writing docs, outputting requirements summaries, creating
  epics/tasks/architecture docs, planning implementation, coding, review, or checkpointing.
---

# Gather Requirements

Clarify what the user actually wants before creating an epic, task, architecture artifact, or plan.

This skill is an interactive interview. It may inspect repository docs and code for facts, but it
does not write files, produce a structured requirements output, or invoke downstream workflow
skills.

<HARD-GATE>
Do NOT create or modify files.
Do NOT write an epic, task, architecture document, plan, story, acceptance document, checkpoint, ADR,
LANGUAGE.md, or DOMAIN.md.
Do NOT output a requirements packet, requirements summary, PRD, brief, architecture document, issue,
or task draft.
Do NOT invoke another skill after requirements feel clear.
Do NOT turn unresolved uncertainty into a requirement.
</HARD-GATE>

## Interview Rules

- Ask one question at a time.
- Include your current guess or recommended answer with each question.
- Verify facts from the repo or official docs instead of asking the user to remember them.
- Start with a current read, confidence level, and load-bearing assumptions.
- Separate what the user wants from what they think they should want.
- Challenge vague terms such as "robust", "scalable", "modern", and "best practice".
- When direction is unclear, explore a small set of meaningfully different product or scope
  options before converging.
- Use scenarios to make requirements concrete.
- Capture non-goals, assumptions, anti-patterns, and unresolved questions in the conversation.
- Stop when the user is ready to choose `create-epic`, `create-task`, `spike`, or no artifact.

## Output Boundary

Do not produce a formal output artifact. This skill's value is the interview itself.

The final message should be short and conversational. It should only say that the requirements are
clear enough and name the recommended next action:

```text
I think we have enough to move this into create-epic.
```

If important uncertainty remains, ask the next question instead of summarizing. If the uncertainty
needs investigation, recommend `spike`. If the user was only exploring, recommend stopping without
an artifact.

## Workflow

### 1. Context First

Before asking requirements questions, load enough context to interview in the project's actual
language.

Always read, when present:

1. The user's latest request and relevant prior conversation.
2. Project `AGENTS.md`.
3. Project work config when `AGENTS.md` or repo docs reference one.
4. Relevant package/module `AGENTS.md` when the ask clearly belongs to a package, service, app, or
   runtime.
5. Relevant local agent/persona docs when the request clearly matches an ownership area.
6. An inventory of `.agents/rules/`, then read only rule files whose path scope or topic matches the
   ask.
7. Existing `LANGUAGE.md`, `DOMAIN.md`, ADRs, or equivalent vocabulary/decision docs that overlap
   with the ask.

Check when relevant:

- existing epic/task docs under `docs/epics/` or `docs/work/tasks/`
- nearby implementation code when it can answer factual questions
- dependency files when library, framework, runtime, or version assumptions matter
- official docs when a source-checkable external claim affects the requirements discussion

Do not load everything. Context flooding makes the interview worse. Do not create a context summary
artifact; use the context to ask better questions.

### 2. State The Starting Read

Before asking the first question, state the current read, confidence, and load-bearing assumptions
in one opening block:

```text
CURRENT READ:
[One sentence describing what you think the user wants.]

CONFIDENCE:
[low / medium / high] - [what is still missing]

ASSUMPTIONS I'M MAKING:
1. [Load-bearing assumption]
2. [Load-bearing assumption]

Correct me now or I'll continue with these.
```

Use plain confidence labels unless a numeric confidence would help. Include only assumptions that
would materially affect the requirements. Do not include obvious or harmless guesses.

During the interview, classify assumptions as:

- verified from repo, docs, code, or official sources
- confirmed by the user
- unverified but accepted as a risk
- blocking and needing a question or spike

Do not let unverified assumptions become requirements.

### 3. Ask One Question At A Time

Every question must include a guess or recommended answer:

```text
Q: [single question]
GUESS: [your current best guess and why]
```

Wait for the user before asking the next question.

Ask about decisions, not facts you can verify. If a fact can be found in code or docs, inspect the
source and bring the finding back.

### 4. Watch For Misleading Answers

Probe harder when the user uses conventional or sophistication-signalling language:

- best practice
- scalable
- robust
- modern
- clean architecture
- standard dashboard
- flexible
- future-proof

Use direct probes:

```text
If you did not have to justify this to anyone, what would you actually want?
```

or:

```text
Which tradeoff matters more here: [option A] or [option B]?
```

### Optional: Explore Product Options

Use this only when the product direction, scope boundary, or desired outcome is genuinely unclear.
Do not run it as a required phase.

Generate two or three meaningfully different options, then converge:

- name the option
- describe the user/product outcome it optimizes for
- state what it includes and deliberately excludes
- stress-test it against constraints, non-goals, assumptions, and scenarios
- recommend one option with a short reason
- ask the user to choose, combine, or correct the options

Keep this at the requirements level. Do not settle architecture, package boundaries, models,
implementation tasks, or technical design here.

### 5. Sharpen Domain Language

Apply the practitioner vocabulary test:

- Would a senior practitioner in this project/domain use this term with a peer?
- Does the term conflict with existing `LANGUAGE.md`, `DOMAIN.md`, code, or project docs?
- Is one word hiding multiple concepts?

When language is fuzzy, propose a precise term and ask for confirmation. Do not update durable
language files in this skill. Leave any durable language decisions to the artifact-creation skill.

### 6. Build Scenarios

Use scenarios to make requirements concrete. Prefer scenario language over formal Cucumber syntax
unless the user explicitly wants BDD.

Good scenario prompts:

- Who is acting?
- What situation are they in?
- What triggers the behavior?
- What should happen?
- What edge cases change the answer?
- What must not happen?
- What would prove this scenario is accepted?
- What would make this scenario not accepted?

Scenarios are not implementation tasks. Acceptance prompts are for direction only; do not draft the
scenario file during requirements gathering.

### 7. Surface Boundaries

Capture:

- non-goals
- out-of-scope ideas
- assumptions
- anti-patterns
- unresolved questions
- candidate spikes

If the ask contains multiple independent pieces, say so and help identify whether this should become
an epic with multiple tasks or one standalone task.

### 8. Validate Claims

Verify source-checkable claims before treating them as established in the conversation:

- repository behavior: inspect code/docs
- library/framework/API behavior: use official docs
- product/domain history: inspect existing specs, external tickets, decisions, or language docs

If a claim cannot be verified, mark it as an unverified assumption.

### 9. Confirm Readiness

When the requirements are clear enough, ask whether the user is ready to create an artifact. Do not
restate the whole conversation as a packet.

For `create-epic`, clear enough means the work has a requirements shape, at least one meaningful
scenario, and acceptance direction for the important scenarios.

```text
I think the shape is clear enough for create-epic. Are you ready to move to that, or is there one
more thing you want to clarify first?
```

Do not accept "whatever you think" as approval to create anything. The user chooses the next skill
or asks for more clarification.

Stop when:

- the user explicitly says the shape is clear enough or chooses the next action, and
- you can predict the user's answer to the next few likely clarification questions, or
- continued questioning is no longer improving confidence and you have surfaced the blocker.

## Recommended Next Artifact

Recommend exactly one next step:

- `create-epic` when the work is a larger epic, capability, or roadmap item with multiple
  scenarios or delivery slices, and enough acceptance direction for the important scenarios.
- `create-task` when the work is a compact standalone task, external ticket, or issue.
- `spike` when one blocking uncertainty needs focused discovery.
- `stop` when the conversation was exploratory and no artifact should be created yet.

If `create-epic` or `create-task` is not available in the current environment, name the desired
artifact type and stop. The user decides what to run next.

## Anti-Patterns

### Writing the artifact
- **Detection:** creating `docs/epics/...`, `docs/work/...`, architecture docs, plans, stories, or
  local work artifacts.
- **Why:** requirements gathering should not silently choose an artifact shape.
- **Resolution:** continue the interview, or stop with a short recommendation for the next skill.

### Question batching
- **Detection:** asking several questions in one message.
- **Why:** later questions depend on earlier answers and the user skim-answers.
- **Resolution:** ask one question with one guess.

### Surveying instead of interviewing
- **Detection:** asking open-ended questions without a current read or guess.
- **Why:** the user has to do all the framing work.
- **Resolution:** include a guess or recommended answer.

### Accepting generic language
- **Detection:** treating words like robust, scalable, dashboard, or modern as requirements.
- **Why:** generic terms route to generic output and hide tradeoffs.
- **Resolution:** translate into project/domain terms and concrete scenarios.

### Premature solutioning
- **Detection:** jumping to architecture, components, files, or tasks before intent is confirmed.
- **Why:** the wrong solution can become anchored before the right problem is understood.
- **Resolution:** return to confirmed intent, constraints, scenarios, and non-goals.

### False certainty
- **Detection:** treating assumptions as facts during the interview.
- **Why:** downstream artifact writers and planners will treat them as true.
- **Resolution:** verify them or mark them unverified.
