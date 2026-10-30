# Skills

Personal Codex skills.

## Included Skills

- `system-design` — map a whole product at the system level before code, one agreed step at a time.
- `handover` — send a focused task handover to another Herdr pane, workspace, project, worktree, or branch.
- `open-session` — load or resume task or epic workflow state.
- `gather-requirements` — interview the user and clarify requirements without writing artifacts.
- `create-epic` — turn clear requirements into an epic requirements/scenarios workspace.
- `create-task` — turn clear narrow work into a compact task brief.
- `architect-solution` — turn approved requirements into architecture and model artifacts.
- `spike` — run focused discovery for one blocking uncertainty.
- `plan-work` — turn an approved artifact into execution-ready plans with task context.
- `execute` — implement one clear bounded change directly or run an approved task plan or epic phase.
- `review-code` — review completed work against the plan, artifact, rules, and diff.
- `functional-test` — create functional cases and execute a runtime verification pass.
- `checkpoint` — save durable workflow state at a session boundary.
- `freeze-knowledge` — promote stable runtime knowledge into durable docs.
- `finish-work` — close a task or epic with completion evidence.

## Use And Install

List the available skills without installing them:

```sh
npx skills add rizvancodes/skills --list
```

Load one skill for a single task without adding it to the persistent skill catalog:

```sh
npx skills use rizvancodes/skills@plan-work
```

Install selected skills globally for supported agents:

```sh
npx skills add rizvancodes/skills \
  --skill plan-work \
  --skill execute \
  --skill review-code \
  --agent claude-code \
  --agent codex \
  --agent kimi-code-cli \
  --agent opencode \
  --global \
  --yes
```

Workflow skills use source-qualified references for optional technical guidance. This lets an
agent load a dependency directly from its upstream source without requiring a separate registry or
persistent installation:

| Domain | Skill reference |
| --- | --- |
| Module seams, interface depth, and testability | `mattpocock/skills@codebase-design` |
| API and public interface design | `addyosmani/agent-skills@api-and-interface-design` |
| Frontend architecture and UI engineering | `addyosmani/agent-skills@frontend-ui-engineering` |
| Compatibility, migration, and rollout | `addyosmani/agent-skills@deprecation-and-migration` |
| Security, privacy, permissions, and tenancy | `addyosmani/agent-skills@security-and-hardening` |
| Observability and instrumentation | `addyosmani/agent-skills@observability-and-instrumentation` |
| PostgreSQL schema design | `wshobson/agents@postgresql-table-design` |
| Supabase/PostgreSQL performance and RLS | `supabase/agent-skills@supabase-postgres-best-practices` |
| Web performance | `addyosmani/web-quality-skills@performance` |

## Workflow Docs

The workflow overview lives under:

```text
docs/workflow-skills/
```

## Layout

```text
docs/
  workflow-skills/
skills/
  system-design/
    SKILL.md
    references/
  handover/
    SKILL.md
    agents/openai.yaml
  open-session/
    SKILL.md
  gather-requirements/
    SKILL.md
  create-epic/
    SKILL.md
  create-task/
    SKILL.md
  architect-solution/
    SKILL.md
    references/
  spike/
    SKILL.md
    references/
  plan-work/
    SKILL.md
    references/
  execute/
    SKILL.md
    references/
  review-code/
    SKILL.md
    references/
  functional-test/
    SKILL.md
    references/
  checkpoint/
    SKILL.md
    references/
  freeze-knowledge/
    SKILL.md
    references/
  finish-work/
    SKILL.md
    references/
references/
  ATTRIBUTIONS.md
```
