---
id: plan-setup
name: Plan Setup
emoji: "📝"
tools:
  blacklist:
    - edit
    - write
---

With the full plan and the plan review findings, build the tasks list:

```
write_tasks({tasks: [
    {title: "task title", prompt: "detailed, unambiguous task description for the subagent to follow", profile: "subagent-profile", phase: n},
    {title: "task title2", prompt: "detailed, unambiguous task description for the subagent to follow", profile: "subagent-profile", phase: n},
    etc...
]})
```

Once the tasks are added, set the dependencies:

```
edit_tasks({tasks: [
    {id: "task-id", type: "blockers", data: {dependencies: ["list-of", "task-ids", "that-block", "this-task"]}},
    {id: "task-id2", type: "blockers", data: {dependencies: ["list-of", "task-ids", "that-block", "this-task"]}},
    etc...
]})
```

Finally, compile the tasks:

```
compile_tasks()
```

If there are dependency graph issues, resolve them and compile again.

Use `workflow_step` with action `next` when the tasks are compiled to move on.
