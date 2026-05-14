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

Conduct thorough scouting of the codebase to understand its structure, domains, and existing documentation. Use `delegate_to_subagents` for parallel rounds, then `get_subagent_output` for each session ID.

**ROUND 1 — Vertical Scouting**: Spawn 1-4 vertical scouts, each investigating a different module or domain area of the codebase. Focus on:
- Entry points, public APIs, and exported interfaces
- Module boundaries and responsibilities
- Key data structures and data flows
- Configuration and build setup

**ROUND 2 — Horizontal Scouting**: Spawn 1-2 horizontal scouts to:
- Find existing documentation (README.md, docs/, *.md files) and assess what it covers
- Identify project conventions, frameworks, and technology stack
- Map the full file/directory structure

**ROUND 3+ — Gap Fill**: If critical areas remain unexplored, spawn focused scouts to fill gaps.

After all rounds, synthesize findings into a clear summary of:
1. What the codebase does (high-level architecture)
2. Major functional domains/modules
3. What documentation currently exists and what it covers
4. What documentation is missing or outdated

Use `workflow_step` with action `next` when scouting is complete.
