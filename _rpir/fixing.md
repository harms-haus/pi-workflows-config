---
id: fixing
name: Fixing
emoji: "🔧"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - task-worker
  - task-worker-tests
---

ISSUE CHECKPOINT: Check if there are any issues from the code review phase. If there are none, immediately perform `workflow_step next` to skip this phase.

If there are issues from the code review phase, address the findings now. Loop through each MEDIUM, HIGH or CRITICAL severity finding:

1. Create a todo for each fix using `write_todos` or `edit_todos`
2. For each fix, spawn a task-worker subagent (maximum: 4 in parallel with atomic changes only, utilize multiple rounds of fixes as necessary):
   `delegate_to_subagents: [{ name: "fix-N", prompt: "Fix this review finding: [finding description]. Severity: [level].", profile: "task-worker" }]`
3. After the subagent(s) complete, verify the fix with `get_subagent_output`
4. `edit_todos: { action: 'complete', indices: [N] }` when done

For test-related fixes, use the `task-worker-tests` profile.

Use `workflow_step` with action `loop` to return to the review phase after making changes. Or if no changes were needed `workflow_step next`.
