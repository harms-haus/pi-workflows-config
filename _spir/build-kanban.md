---
id: build-kanban
name: Build Kanban
emoji: "📋"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - kanban-planner
---

Build the kanban board from the approved plan. Delegate to the kanban-planner subagent:

```
delegate_to_subagents: [{
  name: "build-kanban",
  prompt: "Build a kanban board from this plan:\n\n[INSERT THE FULL PLAN HERE]\n\nMaximize parallelism. Each task must be atomic and self-contained.",
  profile: "kanban-planner"
}]
```

Be sure to include `files: ["rel/file-name.md", {path: "rel/file-name2.ts", tail: 100}]` with relevant files the planner needs to understand the codebase structure.

The planner will call `create_kanban` directly. Call `get_subagent_output` to confirm the board was created successfully. Then call `list_kanban` to verify the board state looks correct.

**Verify:**
- Task count matches the plan
- Dependencies are correct (no orphaned blockers, no cycles)
- Phases are valid subsequences of ["test", "implement", "review"]
- No task is ambiguously described

If the board looks wrong, you can manually call `create_kanban` yourself with corrections (but note: create_kanban will fail if a board already exists — you'd need to ask the user to restart).

Use `workflow_step` with action `next` when the board is built and verified.
