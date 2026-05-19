---
id: debug-fixing
name: Debug Fixing
emoji: "🔧"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - task-worker
  - task-worker-lite
---

This is a fix phase using the task-worker/task-worker-lite profile. Check if there are issues to resolve from the task-reviewer phase BEFORE you move on to new todos.

Choose 1-4 new or fix todos that are ready to start in parallel.

If a todo is no longer relevant, use `edit_todos: { action: 'abandon', indices: [N] }`.

1. `edit_todos: { action: 'start', indices: [N] }` for the next incomplete todo(s)
2. Spawn task-worker subagent(s) (task-worker for harder tasks and task-worker-lite for straightforward tasks):
   `delegate_to_subagents: [{ name: "fix-N", prompt: "Fix todo #[N]: [text]. The root cause has been identified and this fix is planned. Implement the fix, then verify with lsp-diagnostics, lint-files, and affected tests.", profile: "task-worker", timeout: 600 }]`
3. `get_subagent_output` to see the resulting message(s)

DO NOT complete more than the 1-4 that you start with — the next task will review the subagents' work and loop back to continue fixing or proceed. REVIEWING IS NOT YOUR JOB in this phase.

Use `workflow_step` with action `next` when all subagents are done to perform the completed tasks' review.
