---
id: debug-build-tasks
name: Build Tasks
emoji: "🏗️"
tools:
  blacklist:
    - edit
    - write
---

Build the task board from the approved plan. **Prefer big phases over small ones** — combine related tasks into fewer, larger phases rather than splitting into many granular phases. The goal is to maximize parallelism within each phase while keeping the phase count low.

**Process:**

1. Using the approved plan, write the tasks into the task board:

```
write_tasks({mode: "replace", phases: [
    {title: "Phase 1: [descriptive title]", tasks: [
        {title: "task title", prompt: "detailed, unambiguous task description for the subagent to follow", profile: "task-worker"},
        {title: "task title2", prompt: "detailed, unambiguous task description for the subagent to follow", profile: "task-worker-lite"},
        etc...
    ]},
    {title: "Phase 2: [descriptive title]", tasks: [
        ...
    ]},
]})
```

2. Set dependencies between tasks that need them:

```
edit_tasks({tasks: [
    {id: "task-id", type: "blockers", data: {dependencies: ["list-of", "task-ids"]}},
    etc...
]})
```

3. Compile the tasks:

```
compile_tasks()
```

If there are dependency graph issues, resolve them and compile again.

**Guidelines for grouping tasks into phases:**
- Tasks that can run in parallel should be in the SAME phase
- Only create a new phase when tasks genuinely depend on outputs from a previous phase
- Aim for 2-4 phases total for a typical bug fix — fewer phases is better
- Use `task-worker-lite` for straightforward, well-scoped fixes
- Use `task-worker` for complex changes requiring deeper reasoning
- Use `task-worker-tests` for test-related tasks

Use `workflow_step` with action `next` when the tasks are compiled.
