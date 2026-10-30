---
name: system-design
description: |
  Design a whole product at the system level with the user, before more code: context, spine,
  deployed apps, a tiered capability map, ownership, data models, shared language, code shape,
  decisions with their automated checks, and a walking-skeleton first slice. Use when the user
  starts a product, rebuilds one, or wants to step back and map an existing one. A discussion, one
  step at a time, shown in standard diagrams. Do NOT use for planning or coding.
disable-model-invocation: true
---

# System design

Design the product at the system level with the user, one agreed step at a time, before more code.

The map is a north star, not a build plan. Map the whole product so later work lands in the right
place, but build only the next small slice. Nothing in code, tables or checks exists for a
capability until a slice needs it.

<HARD-GATE>
Do NOT change code, tickets or infrastructure until the user says "build".
Do NOT interrupt running agents, servers or stacks while reading.
Do NOT move to the next step until the user agrees the current one.
Do NOT invent constraints, users or requirements; ask for them.
</HARD-GATE>

## How to talk

- Stay big picture.
- Give one clear recommendation at a time, with your own reasoned view. Don't just agree.
- Ask at most one focused question, then wait.
- Show your work visually, readable on a phone. See `references/diagrams.md` for which diagram
  goes with which step.

## Start

1. Find the product. Use the text the skill was called with, if any; otherwise read the README,
   AGENTS.md and the repository's apps and packages.
2. Find the sources: docs, tickets and any older version of the product. Use an older version as
   evidence of what was tried and what went wrong, not as a template. Say what you found, not what
   you assume.
3. Choose where decisions will be recorded. Default to `docs/decisions/` in the repository.
4. Open with one short message: what you understand the product to be, the sources you found,
   where you'll record decisions, your first draft of the spine, and one question covering
   whatever you couldn't find (for example an older version outside the repository).

## The steps

Work through these in order, one at a time.

1. **Context.** Who the product is for, and anything that limits or fixes the choices ahead. Ask
   for what you can't find; don't invent constraints. Every later choice is checked against these.

2. **The spine.** The core flow of value through the product, in one line.

3. **What gets deployed.** Each app or service, where it runs, and its one job, as a table.

4. **A capability map in three tiers.**
   - Product capabilities: what users experience, each with exactly one job.
   - Shared foundations: building blocks several capabilities use.
   - Platform services: cross-cutting plumbing with no product knowledge (auth, permissions,
     audit, files, notifications, usage, integrations, AI providers, privacy).

   For each: its job in one sentence, what it uses, and what it never does. Label each one core,
   supporting or generic (domain-driven design's subdomains), with a one-line reason: build the
   core with the most care, keep supporting parts simple, and buy and wrap generic ones.

   Then a matrix: capabilities down the side, apps across the top, showing where each capability
   appears.

5. **Pressure-test the map.** Challenge every name until it means one thing; if the user and you
   use a word differently, stop and settle it. Look for capabilities doing two jobs, two
   capabilities doing one job, and things that serve some capabilities but not others (a sign
   they're separate). Check whether the map can absorb plausible future features without
   reshuffling.

   Draft the map from the product first. Then, if an older version exists, compare: for each
   capability, keep, change or new, with the evidence. The old version shows what was tried and
   what went wrong; it is not a template, and without real users it says nothing about what users
   need.

6. **Ownership.** For each kind of fact, which part of the system is its single authority (for
   example live state versus lasting records), and how parts hand facts to each other.

7. **Data models**, capability by capability in dependency order: the main entities, their keys
   and relationships, and who owns each. Detail the first slice's entities fully; sketch the rest
   as names and relationships only, to be detailed when their slice is shaped.

8. **The language.** Every agreed term goes into a `LANGUAGE.md` glossary, one meaning each, with
   the words to avoid. Code, tests and tickets use it.

9. **How the map shows in code.** Code lives in the app that runs it; backends are organised one
   folder per capability and per platform service, with clear layers; frontends by screens;
   packages only for genuinely shared code. For each runtime, name its layering in plain words and
   the established pattern it follows (for example layered architecture, or functional core and
   imperative shell), with a source.

10. **Decisions and their enforcers.** List every decision made. For each, say whether a program
    can decide it exactly (layer imports, which folder may touch the database, permissions only
    through one service, row-level security, pinned versions, and so on). Those become automated
    checks, written with a test proving each fires, before feature code. Checks are written with
    the code they protect, not ahead of it. Each check names the real mistake it prevents: from the
    older version's history, or a cited, well-known failure. No check without a reason. Judgement
    calls stay in written guidance, never in brittle checks.

11. **A gap check.** What's missing, duplicated or in the wrong place, and what to fix structurally
    now rather than bolt on later.

12. **The first slice.** A walking skeleton: the thinnest path along the spine that touches every
    app, built end to end before any real feature. Say what each part of it proves and what it
    deliberately leaves out.

## How to judge choices

- Against external best practice and well-regarded practitioners, with sources, not against the
  user's other projects.
- Prefer conventional, minimal structure and no speculative architecture. When you recommend
  something unconventional, say why it's worth it.
- Use domain-driven design's strategic ideas (bounded contexts, ubiquitous language), not its
  tactical patterns, unless the domain needs them.
- A big choice is one that's expensive to reverse. For each, research the options separately,
  compare them in a table with sources against the context from step 1, and make one
  recommendation.

## Record decisions

Record each agreed decision as an architecture decision record, using
`references/adr-template.md`, in the place agreed at the start. Create it once the user confirms.
