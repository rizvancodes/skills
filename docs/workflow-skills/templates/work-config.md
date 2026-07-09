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

## Artifact Contract

Artifact names inside a task or epic root are the workflow contract. Do not rename them in project
config.

Tasks use:

- `brief.md`
- `plan.md`
- `review.md`
- `checkpoint.md`
- `finish.md`
- `spikes/`
- `functional-tests/`

Epics use:

- `status.md`
- `checkpoint.md`
- `finish.md`
- `requirements.md`
- `scenarios/`
- `architecture.md`
- `models/`
- `plans/`
- `reviews/`
- `spikes/`
- `functional-tests/`
