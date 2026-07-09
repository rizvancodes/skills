# Quality Heuristics

Use these heuristics only when project rules do not already cover the quality concern. Project
rules override this baseline. Each heuristic is a judgement call, never an automatic violation.
Report it only with concrete file/hunk evidence and the product, source, workflow, domain, or
maintainer context that makes it matter.

Do not report issues that tooling already catches cleanly unless the tool output is part of the
review evidence.

## Baseline Smells

- **Mysterious Name:** a function, variable, type, file, or module name does not reveal what it does
  or holds. Prefer an honest name; if no honest name exists, the design may be unclear.
- **Duplicated Code:** the same logic shape appears in more than one hunk or file. Extract or
  centralize only when the duplication is real in this change.
- **Feature Envy:** code reaches deeply into another object's or module's data more than its own.
  Consider moving behavior toward the data or exposing a better interface.
- **Data Clumps:** the same fields or parameters repeatedly travel together. Consider giving the
  group a named type/value object.
- **Primitive Obsession:** a primitive or string stands in for a meaningful domain concept. Consider
  a named type, enum, schema, or small wrapper where it clarifies invariants.
- **Repeated Switches:** the same branch cascade on the same concept appears in multiple places.
  Consider one map, dispatch table, polymorphic shape, or central decision point.
- **Shotgun Surgery:** one logical change forces scattered edits across many files. Consider moving
  the changing responsibility behind one module or interface.
- **Divergent Change:** one file or module changes for several unrelated reasons. Consider splitting
  responsibilities so each owner changes for one reason.
- **Speculative Generality:** abstraction, configuration, parameters, or extension hooks are added
  for needs not required by the current artifact. Prefer deleting or inlining until need is proven.
- **Message Chains:** callers navigate through long object/module chains and depend on internal
  structure. Consider hiding the walk behind a method or module-level interface.
- **Middle Man:** code mostly delegates without adding policy, translation, validation, or useful
  simplification. Consider removing the pass-through layer.
- **Refused Bequest:** a subtype, implementation, or adapter ignores most of the contract it
  inherits or implements. Consider composition, a narrower interface, or splitting the contract.
