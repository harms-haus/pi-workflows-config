---
id: review-plan
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

`delegate_to_subagents: [{ name: "review-N", prompt: "Review the plan for: [task description]. Check for completion and executability. Full plan: [plan details]", profile: "plan-reviewer" }]`

Be sure to include `files: ["rel/file-name.md", {path: "rel/file-name2.ts", tail: 100}]` with a list of files and file chunks that the subagent will need.

Use `workflow_step` with action `next` when review is complete to move on to the plan setup.
