---
id: debug-investigation
name: Debug Investigation
emoji: "🔬"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - bug-scout
---

Quickly investigate the bug to identify root cause and affected files. This is a **lean** research phase — no multi-round parallel scouting.

**Investigation steps (do sequentially, NOT in parallel):**

1. **Reproduce & Scope**: Read the bug description. Use `bash` to reproduce the issue if possible (run tests, trigger the error). This is not an investigation step, you're working to get a general idea or identify the exact error message, stack trace, or incorrect behavior quickly without diving too deep.

2. **Spawn Bug Scout**: Delegate to 1-4 bug-scout subagents to trace the root cause:
   `delegate_to_subagents: [{ name: "trace-root-cause", prompt: "Investigate this bug: [bug description]. Trace the root cause through the codebase. Use lsp-goto-definition, lsp-find-references, and lsp-call-hierarchy to follow the chain. Report: exact file(s) and line(s) involved, the root cause, and any contributing factors.", profile: "bug-scout" }]`

3. **Synthesize**: Collect findings with `get_subagent_output`. You should now know:
   - The root cause of the bug
   - Which files need to change
   - Any edge cases or related code that might be affected

Once you know the root cause and affected files and have a good view of the problem space, advance. If you need to understand the broader architecture for planning, a quick scan is fine — but this is a debug workflow, not a feature workflow. Find the possible surfaces, causes, etc, then continue to planning.

Use `workflow_step` with action `next` when the root cause is identified.
