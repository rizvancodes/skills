# Skills

Personal Codex skills.

## Included Skills

- `handover` — send a focused task handover to another Herdr pane, workspace, project, worktree, or branch.
- `open-session` — load or resume task or epic workflow state.
- `gather-requirements` — interview the user and clarify requirements without writing artifacts.
- `create-epic` — turn clear requirements into an epic requirements/scenarios workspace.
- `create-task` — turn clear narrow work into a compact task brief.
- `architect-solution` — turn approved requirements into architecture and model artifacts.
- `spike` — run focused discovery for one blocking uncertainty.
- `plan-work` — turn an approved artifact into execution-ready plans with task context.
- `execute` — run one approved task plan or epic phase.
- `review-code` — review completed work against the plan, artifact, rules, and diff.
- `functional-test` — create functional cases and execute a runtime verification pass.
- `checkpoint` — save durable workflow state at a session boundary.
- `freeze-knowledge` — promote stable runtime knowledge into durable docs.
- `finish-work` — close a task or epic with completion evidence.

## Install

Install one skill into Codex from this repo with the Codex skill installer:

```sh
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo rizvancodes/skills \
  --path skills/handover
```

Install all Herdr helper skills:

```sh
for skill in handover; do
  python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
    --repo rizvancodes/skills \
    --path "skills/$skill"
done
```

Install all workflow skills:

```sh
for skill in open-session gather-requirements create-epic create-task architect-solution spike plan-work execute review-code functional-test checkpoint freeze-knowledge finish-work; do
  python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
    --repo rizvancodes/skills \
    --path "skills/$skill"
done
```

Restart Codex after installing new skills.

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
  guides/
```
