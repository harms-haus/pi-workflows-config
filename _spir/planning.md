---
id: planning
name: Planning
emoji: "📋"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - task-planner
---

DECISION IDENTIFICATION: Before creating a plan, review the research findings and identify decisions that are too big to assume the answer for. These include:
- Architectural choices (monolith vs modular, sync vs async, etc.)
- Library/tool selection (which library to use among alternatives)
- Engineering methods (TDD vs tests-after, incremental vs big-bang, etc.)
- Prioritization of work categories
- Any trade-off where reasonable people might disagree

Use `ask_user_question` to present these decisions to the user. Ask up to 4 questions at a time, each with 2-4 options and concise descriptions. Wait for the user's answers before proceeding.

Delegate to the planner subagent, incorporating the user's decisions and a VERY detailed research summary. Do not be afraid to over-supply research. Include file names and line numbers, example code from the codebase, summaries of systems, horizontal exploration results and vertical research:
- { name: "create-plan", prompt: "Based on this research: [complete summary so that no new research must be done]\n\nUser decisions: [decisions]\n\nCreate a detailed, unambiguous implementation plan. Each step must be atomic.", profile: "task-planner" }
- Be sure to include `files: ["rel/file-name.md", {path: "rel/file-name2.ts", tail: 100}]` with a list of files and file chunks that the subagent will need.

Planning can take quite some time: a lot of information is being collated and important decisions are being made. Start with a 600s timeout and resume if the timeout is reached.

Call `get_subagent_output` to retrieve the plan.

DO NOT WRITE TODOs. The plan reviewer is next, and following that the task writer.

Use `workflow_step` with action `next` when the plan is finalized.
