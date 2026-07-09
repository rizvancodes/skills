# References

This file lists externally checkable research and method references behind the public workflow
language. It does not attempt to document private workflow iterations or personal shorthand.

## AI Agent Context

- Anthropic, [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents).
  Supports the package's treatment of context as a finite resource that should be curated rather
  than dumped into every agent turn.
- Anthropic, [Equipping agents for the real world with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills).
  Supports modular skills, progressive disclosure, and procedural knowledge captured as reusable
  agent capabilities.
- Nelson F. Liu, Kevin Lin, John Hewitt, Ashwin Paranjape, Michele Bevilacqua, Fabio Petroni, and
  Percy Liang, [Lost in the Middle: How Language Models Use Long Contexts](https://aclanthology.org/2024.tacl-1.9/).
  Supports the package's bias toward positioning important instructions close to the point of use
  and avoiding oversized context packets.

## Domain Language And Architecture Knowledge

- Martin Fowler, [Ubiquitous Language](https://martinfowler.com/bliki/UbiquitousLanguage.html).
  Supports `LANGUAGE.md` as a small, loadable home for canonical terms, avoid terms, relationships,
  and ambiguity reduction.
- Martin Fowler, [Domain-Driven Design](https://martinfowler.com/bliki/DomainDrivenDesign.html).
  Supports the package's emphasis on domain model, boundaries, rules, and vocabulary before
  implementation.
- Michael Nygard, [Documenting architecture decisions](https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions).
  Supports ADRs as lightweight records for architecturally significant decisions.
- Martin Fowler, [Architecture Decision Record](https://martinfowler.com/bliki/ArchitectureDecisionRecord.html).
  Provides a concise modern summary of ADR purpose and lineage.
- arc42, [Template overview](https://arc42.org/overview).
  Supports separating architecture constraints, solution strategy, decisions, and cross-cutting
  concepts instead of burying them in runtime plans.
- Diátaxis, [A systematic approach to technical documentation authoring](https://diataxis.fr/).
  Supports separating documentation by user need rather than mixing explanation, how-to, reference,
  and tutorial material in one artifact.

## Execution Discipline

- Martin Fowler, [Test-Driven Development](https://martinfowler.com/bliki/TestDrivenDevelopment.html).
  Supports red-green-refactor as a disciplined loop for behavior changes and bug fixes.
- Kent Beck, [Test Driven Development: By Example](https://www.oreilly.com/library/view/test-driven-development/0321146530/).
  Primary book reference for TDD practice.
- Martin Fowler, [Yagni](https://martinfowler.com/bliki/Yagni.html).
  Supports the package's scope discipline and resistance to speculative features or abstractions.
- Dave Thomas and Andy Hunt, [The Pragmatic Programmer: Tracer Bullets](https://www.oreilly.com/library/view/the-pragmatic-programmer/9780135956977/f_0030.xhtml).
  Supports quick, visible, repeatable end-to-end feedback through thin slices.
- Jimmy Bogard, [Vertical Slice Architecture](https://www.jimmybogard.com/vertical-slice-architecture/).
  Supports organizing implementation around coherent request or feature slices rather than generic
  backend/frontend buckets.
