---
id: scouting
name: Scouting
emoji: "🔍"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - vertical-scout
  - horizontal-scout
---

Conduct thorough scouting in MULTIPLE ROUNDS of parallel subagents until you have comprehensive understanding of the codebase relevant to the task. You may do a small amount of scouting to get the lay of the land but DO NOT SCOUT OR RESEARCH MEANINGFULLY ON YOUR OWN. Use `delegate_to_subagents` for each round, then `get_subagent_output` for each session ID. Give agents ample time to scout: keep timeouts higher than you think they should be. Synthesize findings between rounds to identify gaps. 

**Do NOT advance past this phase until you are confident you have found and understood ALL relevant code.** If gaps remain, spawn another round. Continue until coverage is comprehensive.

Vertical Scouting: Spawn 0-4 vertical scouts, each investigating a DIFFERENT vertical slice relevant to the task. Use profile "vertical-scout".
- For feature development: Scout major dependencies, API surfaces, data flows, and integration points
- For codebase improvement: Audit for code smells, test coverage gaps, large monolith files, security issues, efficiency problems, and architectural concerns

When you're confident you have thorough verticals information, move on to horizontal expansion

Horizontal Expansion: Based on Round 1 findings, spawn 0-2 horizontal scouts (profile "horizontal-scout") to:
- Find existing patterns, conventions, and reusable code across the codebase
- Identify cross-cutting concerns, shared utilities, and module boundaries
- Check for linting/formatting/type-checking setup and configuration

If you have a great overview and many details, but are missing some key pieces, do a gap fill round:

Gap Fill 
- Review all findings.
- If any critical information is missing (unexplored dependencies, unclear integration points, unchecked modules), spawn focused scouts to fill those gaps.
- Use the most appropriate profile (vertical-scout or horizontal-scout) for each gap.

DO NOT USE SCOUTS RESEARCH ON THE WEB: that's the next workflow step.

Use `workflow_step` with action `next` ONLY when you are satisfied with the coverage. Do NOT skip this phase prematurely.
