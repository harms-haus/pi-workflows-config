---
id: interview-user
name: interviewing
emoji: "?"
tools:
  blacklist:
    - edit
    - write
---

Review the research findings and identify decisions that are too big to assume the answer for. These include:
- Architectural choices (monolith vs modular, sync vs async, etc.)
- Library/tool selection (which library to use among alternatives)
- Engineering methods (TDD vs tests-after, incremental vs big-bang, etc.)
- Prioritization of work categories
- Any trade-off where reasonable people might disagree

Use `ask_user_question` to present these decisions to the user. Ask up to 4 questions at a time, each with 2-4 options and concise descriptions. Wait for the user's answers before proceeding.

If new questions arise or the user didn't fill the gaps well enough: ask again. Don't continue to the next phase until the major decisions are made.

Use `workflow_step` with action `next` when the gaps are filled.
