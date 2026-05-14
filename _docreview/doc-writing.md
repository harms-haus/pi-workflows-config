---
id: doc-writing
name: Doc Writing
emoji: "📝"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - doc-writer
---

Implement the documentation plan by delegating to doc-writer subagents. Work through the todos 1-4 at a time in parallel.

For each batch:
1. `edit_todos: { action: 'start', indices: [N, M, ...] }` for the next incomplete todos
2. Spawn doc-writer subagents:

`delegate_to_subagents: [{ name: "doc-N", prompt: "Create/update the documentation file: [todo text]. Use the scouting summary and codebase analysis to write accurate documentation. Read the relevant source code thoroughly before writing.", profile: "doc-writer" }]`

3. `get_subagent_output` for each session to confirm completion

If a todo is no longer relevant, use `edit_todos: { action: 'abandon', indices: [N] }`.

**IMPORTANT**: Each doc-writer subagent should:
- Read the actual source code before writing any documentation
- Organize content by domain/functionality
- Include practical examples where appropriate
- Use proper markdown formatting with code blocks, headings, and links
- Cross-reference related documentation files

After all todos are implemented, use `workflow_step` with action `next` to proceed to the review phase.
