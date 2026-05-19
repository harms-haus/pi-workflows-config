---
id: task-reviewing
name: Task Reviewing
emoji: "✅"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - task-reviewer
---

Review all implemented tasks in this loop for correctness and completeness. For each significant implementation, spawn a task-reviewer subagent:

`delegate_to_subagents: [{ name: "review-N", prompt: "Review the implementation of: [task description]. Bifrost rune ID: [if present]. Check for correctness, edge cases, error handling, and adherence to the plan.", profile: "task-reviewer" }]`

Be sure to include `files: ["rel/file-name.md", {path: "rel/file-name2.ts", tail: 100}]` with a list of files and file chunks that the subagent will need.

After reviewing:
- Collect all findings and categorize by severity (CRITICAL, HIGH, MEDIUM, LOW)
- If MEDIUM, HIGH or CRITICAL issues are found, use `write_todos` to add fix tasks, then use `workflow_step` with action `loop` to restart the implement subworkflow from task-implementing
- If all implementations pass review (no MEDIUM/HIGH/CRITICAL issues), use `workflow_step` with action `next` to proceed to the final workflow loop (full review)
- For each task with a Bifrost rune assigned that has completed without MEDIUM OR HIGHER issues, fulfill the rune: `bf fulfill {rune-id}`
- For each task completed, mark the todo for that task as complete: `edit_todos: { action: 'complete', indices: [N] }`

Use `workflow_step` with action `next` when review is clean to move on, or `loop` to loop to the start of this workflow loop if fixes are needed.
