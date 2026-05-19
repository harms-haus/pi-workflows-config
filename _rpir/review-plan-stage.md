---
id: review-plan-stage
name: Plan Reviewing
emoji: "✅"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - plan-reviewer
---

Review all planned tasks in this loop for completeness and logical flow. Spawn a plan reviewer subagent to find issues with the plan:

`delegate_to_subagents: [{ name: "review-N", prompt: "Review the plan for: [task description]. Bifrost rune ID: [if present]. Check for completion and executability.", profile: "plan-reviewer" }]`

Be sure to include `files: ["rel/file-name.md", {path: "rel/file-name2.ts", tail: 100}]` with a list of files and file chunks that the subagent will need.

After reviewing:
- Collect all findings and categorize by severity (CRITICAL, HIGH, MEDIUM, LOW)
- If MEDIUM, HIGH or CRITICAL issues are found, use `write_todos` to add fix tasks, then use `workflow_step` with action `loop` to restart the planning subworkflow.
- If the plan passes review (no MEDIUM/HIGH/CRITICAL issues), use `workflow_step` with action `next` to proceed to the next workflow loop (implementation)

Use `workflow_step` with action `next` when review is clean to move on, or `loop` to loop to the start of this workflow loop if fixes are needed.
