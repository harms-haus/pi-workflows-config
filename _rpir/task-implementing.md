---
id: task-implementing
name: Task Implementing
emoji: "🔨"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - task-worker
  - task-worker-lite
  - task-worker-tests
---

This is an implementation OR fixing phase. Check if there are issues to resolve from the task-reviewer phase BEFORE you move on to new todos.

**Profile selection:**
- For **large/hard** NEW implementation todos (greenfield work, features): use `task-worker`. Suggested timeout: 600s
- For **small/straightforward** NEW implementation todos (greenfield work, features): use `task-worker-lite`. Suggested timeout: 450s
- For test-related todos: use `task-worker-tests`. Suggested timeout: 600s

Choose 1-4 new or fix todos that are ready to start in parallel.

If a todo is no longer relevant, use `edit_todos: { action: 'abandon', indices: [N] }`.

1. `edit_todos: { action: 'start', indices: [N] }` for the next incomplete todo(s)
2. Check if the todo item(s) have Bifrost rune(s) assigned.
   a. If the todo(s) have a Bifrost rune assigned, claim the rune before delegating to the subagent(s):
      `bf claim {rune-id}`
      Also be sure to include it in the prompt:
      `delegate_to_subagents: [{ name: "task-N", prompt: "Implement todo #[N]: [text]. Bifrost rune ID: [rune-id from todo]", profile: "task-worker" or "task-fixer", timeout: 600 }]`
   b. If no Bifrost rune is assigned, omit it:
      `delegate_to_subagents: [{ name: "task-N", prompt: "Implement todo #[N]: [text]", profile: "task-worker" or "task-fixer" , timeout: 600}]`
3. `get_subagent_output` to see the resulting message(s)

Be sure to include `files: ["rel/file-name.md", {path: "rel/file-name2.ts", tail: 100}]` with a list of files and file chunks that the subagent will need.

For test-related todos, use the `task-worker-tests` profile instead.

IMPORTANT: Perform ONLY ONE wave of parallel agents, NO MORE THAN THAT.

IMPORTANT: DO NOT REVIEW the changes; REVIEWING IS NOT YOUR JOB in this phase.

Use `workflow_step` with action `next` when all subagents are done to perform the completed tasks' review.
