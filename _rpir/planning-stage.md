---
id: planning
name: Planning
emoji: "📋"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - bifrost-planner
  - markdown-planner
  - todo-planner
---

DECISION IDENTIFICATION: Before creating a plan, review the research findings and identify decisions that are too big to assume the answer for. These include:
- Architectural choices (monolith vs modular, sync vs async, etc.)
- Library/tool selection (which library to use among alternatives)
- Engineering methods (TDD vs tests-after, incremental vs big-bang, etc.)
- Prioritization of work categories
- Any trade-off where reasonable people might disagree

Use `ask_user_question` to present these decisions to the user. Ask up to 4 questions at a time, each with 2-4 options and concise descriptions. Wait for the user's answers before proceeding.

PLANNER SELECTION:
- If the task appears simple enough, the "todo-planner" should be used to avoid complexities.
- Check if Bifrost is configured for this project by running `bf list --help` or checking for a `.bifrost.yaml` file. If Bifrost is configured, use the "bifrost-planner" profile to create a rune tree.
- If Bifrost is NOT configured, use the "markdown-planner" profile to create a text-based plan.

Delegate to the planner subagent, incorporating the user's decisions and the research summary:
- { name: "create-plan", prompt: "Based on this research: [complete summary so that no new research must be done]\n\nUser decisions: [decisions]\n\nCreate a detailed, unambiguous implementation plan. Each step must be atomic.", profile: "<bifrost-planner or markdown-planner>" }

Planning can take quite some time: a lot of information is being collated and important decisions are being made. Start with a 600s timeout and resume if the timeout is reached. Don't abandon bifrost runes: prefer to resume the subagent or if an unresolvable issue is found, `bf shatter` the successfully created runes before switching to markdown-planner.

Call `get_subagent_output` to retrieve the plan.

If using bifrost-planner: The planner will create a rune tree and forge it. Parse the reported rune IDs into todos using `write_todos` — each top-level leaf rune becomes one todo item. Include the rune ID in each todo text so implementing agents can claim their rune.
If using markdown-planner: Parse the text plan into todos using `write_todos` — each plan step becomes one todo item.
If using the todo-planner: parse the output from the planner's subagent session into todo items.

Use `workflow_step` with action `next` when the plan is finalized and todos are written.
