---
id: todo-planning
name: Todo Planning
emoji: "📝"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - todo-planner
---

Create a focused todo list from the investigation findings. **Skip Bifrost entirely** — no rune trees, no markdown planning documents. Go straight to todos.

**Process:**

1. Review the investigation summary. Identify the discrete fix steps needed.

2. Delegate to the todo-planner subagent:
   `delegate_to_subagents: [{ name: "create-todos", prompt: "Based on this investigation: [complete summary including root cause, affected files, and contributing factors]\n\nCreate a focused, atomic todo list for fixing this bug. Each todo should be one self-contained fix step. Order them so dependencies are resolved first (e.g., add validation before updating callers).", profile: "todo-planner" }]`

3. Parse the returned todo list and write it using `write_todos`. Each step becomes one todo item.

4. **Present the plan to the user** with a brief summary of what will be done and in what order. Ask if they want to proceed or modify anything.

Use `workflow_step` with action `next` when todos are finalized and the user approves.
