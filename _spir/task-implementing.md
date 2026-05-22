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
  - task-reviewer
---

While you have tasks remaining, claim up to 4 ready tasks:

```
get_ready_tasks({count:4})
```

This will return a list of tasks that are ready to implement. They are now in `implementing` status. Your job is to spawn a task-worker for each ready task, passing the prompt on directly to the subagent.

After the subagents are done, move each task into the `reviewing` status:

```
advance_tasks({tasks: ["task-id", "task-id2"]})
```

Once the tasks are in `reviewing`, spawn 1-4 "task-reviewer" subagents to review the tasks for completion. DO NOT SKIP THIS STEP, it is CRITICAL. Self-review is not acceptable.

When the reviewer(s) are done, evaluate the problems found and spawn task-workers to fix the CRITICAL, HIGH, and MEDIUM issues found.

When the issues are fixed, advance the tasks to the `done` status:

```
advance_tasks({tasks: ["task-id", "task-id2"]})
```

Next, claim new tasks and repeat until all tasks in all phases are complete.

Use `workflow_step` with action `next` when all tasks are done.
