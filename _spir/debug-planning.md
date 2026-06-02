---
id: debug-planning
name: Task Planning
emoji: "📋"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - task-planner
---

Create a focused implementation plan from the bug scouting findings. This is a **lean** planning phase — no heavy planning documents, just actionable tasks.

**Process:**

1. Review the bug scouting summary (root cause, affected files, contributing factors) and any user decisions from the interview phase.

2. Delegate to the task-planner subagent:
   `delegate_to_subagents: [{ name: "create-plan", prompt: "Based on this bug investigation: [complete summary including root cause, affected files, and contributing factors]\n\nUser decisions: [any decisions from interview]\n\nCreate a detailed, atomic task plan for fixing this bug. Prefer fewer, larger phases over many small ones. Group related fixes together. Each task must be one atomic change.", profile: "task-planner" }]`

   Be sure to include `files: ["rel/file-name.md", {path: "rel/file-name2.ts", tail: 100}]` with a list of files and file chunks that the subagent will need.

3. Call `get_subagent_output` to retrieve the plan.

4. **Present the plan to the user** with a brief summary of what will be done and in what order. Ask if they want to proceed or modify anything.

Use `workflow_step` with action `next` when the plan is finalized and the user approves.
