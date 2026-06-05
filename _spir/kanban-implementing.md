---
id: kanban-implementing
name: Kanban Implementing
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
  - code-quality-reviewer
---

Implement tasks from the kanban board. This phase loops until all tasks reach "done" status.

**Main loop:**

1. **Claim tasks** if you have fewer than 4 tasks claimed (eg: if you advance tasks and are back here from step 5 and 2 have moved to "done", claim_tasks anyway to get more tasks to run in parallel):
   ```
   claim_tasks({count: 4})
   ```
   This returns up to 4 tasks ready for work. Outstanding (already claimed) tasks are ALWAYS re-included.

2. **Spawn subagents** — For each claimed task, check its current `phase` and `profile`:
   - `test` phase → spawn `task-worker-tests` subagent
   - `implement` phase → spawn `task-worker` (complex) or `task-worker-lite` (straightforward) subagent
   - `review` phase → spawn **both** a `task-reviewer` subagent AND a `code-quality-reviewer` subagent in parallel. **IMPORTANT**: DO NOT SKIP REVIEWS. Always review with subagents. Self-reviews and tests passing does NOT constitute a successful review. The task-reviewer checks completion/compliance; the code-quality-reviewer checks maintainability/structure.

   Use `delegate_to_subagents` to spawn 1-4 parallel subagents. Pass the task description as the prompt, and include `files` from the task if available.

3. **Collect results** — Use `get_subagent_output` for each session.

4. **Decide outcomes** — For each completed subagent:
   - **Test phase success** → `advance_tasks({ids: ["task-id"]})` to move to implement
   - **Implement phase success** → `advance_tasks({ids: ["task-id"]})` to move to review
   - **Review phase, no issues** → `advance_tasks({ids: ["task-id"]})` to mark done
   - **Review phase, issues found** → `reject_tasks({ids: ["task-id"], reason: "..."})` to send back to implement (or test)
   - **Implementation failed** → `reject_tasks({ids: ["task-id"], reason: "..."})` to send back a phase

5. **Repeat** — Go back to step 1. New tasks may have become unblocked.

**Guidelines:**
- `advance_tasks` releases the claim and moves to the next phase
- `reject_tasks` releases the claim and moves back one phase
- Tasks that reach "done" automatically unblock their dependents
- If no tasks are claimable but some remain blocked, report the situation and use `workflow_step next`

Use `workflow_step` with action `next` when ALL tasks are "done".
Use `workflow_step` with action `loop` if you need to restart the implementation from scratch (rare).
