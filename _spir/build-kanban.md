---
id: build-kanban
name: Build Kanban
emoji: "📋"
tools:
  blacklist:
    - edit
    - write
---

With the full plan and the plan review findings, build the kanban board:

**Your rules:**

1. **One task = one atomic unit of work** — a single change that can be implemented and verified independently by a subagent with no other context. If a task requires multiple unrelated changes, split it.

2. **Phases per task** — Every task gets a `phases` array, a subsequence of `["test", "implement", "review"]` preserving that order:
   - `["implement"]` — default for straightforward tasks (no tests needed, or tests are covered by a separate task)
   - `["test", "implement"]` — task should have tests written first (TDD)
   - `["implement", "review"]` — implementation then code review only
   - `["test", "implement", "review"]` — full pipeline: tests, then implementation, then review

3. **Dependencies via `blockedBy`** — list task titles that must complete before this task can start. A task unblocks its dependents when it reaches "done" (after its final phase).

4. **Maximize parallelism** — tasks with no dependency relationship should have NO blockedBy between them. The goal is a wide dependency DAG, not a linear chain.

5. **Unambiguous descriptions** — each task description must be self-contained so a fresh subagent needs zero external planning context. Include: what files to change, what to change, how to verify. DO NOT write code.

6. **files field** — include a `files` array of relevant file paths for each task to speed up subagent discovery.

7. **Keep it flat** — prefer fewer, larger parallel tasks over many sequential micro-tasks. Create as many/as few tasks as necessary to accomplish the goal.

Example:

```
write_kanban({
  mode: "replace",
  tasks: [
    {
      title: "Setup database schema",
      description: "Create the initial database tables for users and posts...",
      phases: ["test", "implement", "review"],
      files: ["src/db/schema.ts", "src/db/migrations/"]
    },
    {
      title: "Build REST API endpoints",
      description: "Implement CRUD endpoints for the posts resource...",
      phases: ["test", "implement", "review"],
      blockedBy: ["Setup database schema"],
      files: ["src/routes/posts.ts"]
    }
  ]
})
```