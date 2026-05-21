---
id: doc-scouting
name: Doc Scouting
emoji: "📖"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - vertical-scout
  - horizontal-scout
---

Identify gaps between the codebase changes made during this workflow and the existing documentation. The goal is to find what documentation is missing, outdated, or incomplete as a result of the work done so far.

**IMPORTANT**: Do NOT advance past this phase until you are confident you have identified all documentation gaps. Use multiple rounds of scouts if necessary.

ROUND 1 — Change Impact Analysis:
Review the todo list and the work completed in previous phases. Spawn 1-2 vertical scouts to:
- Identify all files that were created, modified, or deleted during this workflow session
- Find all existing documentation files (README.md, docs/ directories, .md files, inline docs, API docs)
- Map which code changes should be reflected in which documentation files

ROUND 2 — Gap Identification:
Based on Round 1 findings, spawn 0-2 scouts to:
- Compare existing documentation against the actual current code state
- Identify new public APIs, configuration options, CLI commands, or behaviors that are undocumented
- Find outdated references (removed functions, renamed modules, changed signatures)
- Check for missing cross-references between docs

ROUND 3+ — Deep Dive (if needed):
If critical areas remain unexplored, spawn focused scouts to:
- Trace specific data flows or API surfaces for documentation coverage
- Verify code examples in documentation match current signatures
- Check README quality and completeness

SYNTHESIS: Compile a clear, prioritized list of documentation gaps:
- What needs to be created (new docs for new features/modules)
- What needs to be updated (existing docs that are now inaccurate)
- What needs to be removed (docs referencing deleted/changed functionality)

Use `workflow_step` with action `next` when your gap analysis is complete.
