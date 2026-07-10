---
name: handover
description: Send a focused task handover to another Herdr pane, workspace, project, worktree, or branch. Use when the user asks to hand over work, send context to another agent/session, continue work in another repo/worktree/branch, or ask another Herdr pane to investigate, implement, review, or report back. Do not use for ordinary answers, simple file opening, or automatic multi-agent orchestration.
---

# Handover

Prepare and deliver a compact task handover through Herdr. The user remains the orchestrator; this skill packages the useful context and sends it to the requested destination.

This skill does not create a workflow pipeline. It only transfers a focused unit of work to another agent/session with enough context to continue safely.

## Preconditions

- Use Herdr commands only when `HERDR_ENV=1`.
- If not running inside Herdr, create the handover block for the user and say it was not sent.
- If detailed Herdr command behavior is needed, read the `herdr` skill before sending.
- Re-read live Herdr ids before acting. Pane, tab, and workspace ids can change after layout changes.
- Never change git state as part of handover unless the user explicitly asked for that exact change.

## Principles

- Prefer direct Herdr prompt delivery over temporary files.
- Reference durable context by path, branch, diff, issue, PR, URL, or commit.
- Include conversation history only when it contains facts not captured elsewhere.
- Give the receiving agent task-local context, not a transcript.
- Surface uncertainty in the handover instead of silently filling gaps.
- Keep the receiving agent's first step explicit.
- Tell the receiving agent how to report back.

## Workflow

1. Resolve the destination.
   - Existing pane: run `herdr pane list` and identify the current pane id.
   - Workspace/project: focus or create a Herdr workspace at the requested path.
   - Worktree path: verify the path exists and use it as the intended working directory.
   - Branch: inspect repo state first. Prefer an existing worktree for that branch. Ask before checking out a branch, creating a worktree, or changing a dirty working tree.
   - Unclear target: ask the user where to send the handover.

2. Verify task context.
   - Check the current directory and relevant git state.
   - Identify the project rules, specs, plans, tickets, diffs, files, logs, or URLs the receiving agent should read first.
   - Prefer paths and pointers over pasted content.
   - Include only facts the receiving agent cannot get from those artifacts.

3. Build the handover.
   - Use the format below.
   - Keep `Summary` scannable.
   - Make `Task brief` complete enough to explain what needs doing, why it matters, and what answer or work shape is expected.
   - Put any uncertainty or conflict in `Relevant state` or `Scope / constraints`.

4. Deliver it.
   - If the target pane is already an agent input, send the handover with `herdr pane send-text` plus `herdr pane send-keys Enter`, or `herdr pane run` when that is appropriate for the target.
   - If the target pane is a shell, do not paste the handover into the shell. Start the requested agent only if the user asked for a new agent; otherwise ask.
   - If creating a new pane, tab, or workspace, parse the id from the Herdr JSON response rather than guessing it.
   - Use `--no-focus` when creating side panes unless the user wants focus moved.

5. Confirm briefly.
   - Say where the handover was sent.
   - Include the live target id when useful.
   - Do not repeat the full handover unless the user asks.

## Handover Format

```md
TASK CONTEXT

Use this brief to perform the task in this session. Do not hand it over again unless the user explicitly asks.

Destination:
- Workspace/project:
- Worktree path:
- Branch:
- Target pane/agent:
- Role:

Summary:
[A short label for scanning]

Task brief:
[Enough context for the receiving agent to understand what needs doing, why it matters, and what shape of answer or work is expected. Keep it focused. Do not paste conversation history unless it contains facts not captured elsewhere.]

Desired outcome:
- [What should be produced]
- [What should be true when done]

Read first:
- [Files, docs, specs, tickets, diffs, commits, PRs, or URLs]

Relevant state:
- [Facts the receiving agent needs that are not already captured in the linked artifacts]

Scope / constraints:
- Allowed:
- Do not:
- Branch/worktree safety:

Next action:
1. Verify cwd and branch.
2. Read the listed context.
3. Report the first finding, plan, or blocker before broader work.

Return with:
- [Expected response shape: findings, plan, patch, review, question, etc.]
```

## Safety Gates

Stop and ask before sending when:

- The destination pane, workspace, project, worktree, or branch is ambiguous.
- Sending the handover would require changing git state.
- The target pane is not clearly an agent input.
- The receiving agent would need secrets.
- The task needs product or architecture judgment the user has not confirmed.
- A simple answer, command, or file link would satisfy the request without involving another agent.

## Do Not

- Do not create a meta-orchestrator.
- Do not summarize the whole conversation.
- Do not run a sequential workflow automatically.
- Do not spawn extra agents unless the user asked for a destination that needs one.
- Do not treat Herdr ids as durable across layout changes.
- Do not hide cost, uncertainty, branch mismatch, dirty worktrees, or destination ambiguity.
