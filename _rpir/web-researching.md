---
id: web-researching
name: Web Researching
emoji: "🌐"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - web-researcher
---

Conduct web research to gather external information relevant to the task. Use 0-4 parallel subagents (`delegate_to_subagents` with the web-researcher profile for each research topic).

Research topics may include:
- Best practices and design patterns for the technologies involved
- Library documentation, API references, and migration guides
- Security considerations and common vulnerabilities
- Performance optimization techniques
- Testing strategies for the relevant frameworks

**Research Loop:** Continue spawning web-researcher subagents until all relevant topics have been adequately covered. Synthesize findings between rounds to identify knowledge gaps.

⚠️ **This phase is skippable:** If the scouting phase was sufficient and no external research is needed (e.g., the codebase is well-understood and the task is straightforward), use `workflow_step` with action `next` to advance. However, do NOT skip prematurely — only skip if you are confident no additional research would be valuable.

Use `workflow_step` with action `loop` if the research findings demand new scouting subagents, or with action `next` when all research is complete or when choosing to skip. This is the last time you can research the codebase or the web, be sure you have everything the planner needs to plan this change.
