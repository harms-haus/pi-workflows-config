---
id: doc-planning
name: Doc Planning
emoji: "📋"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - markdown-planner
---

Based on the scouting findings, create a concrete documentation plan. This phase determines what documentation files need to be created or updated.

**STEP 1 — ASSESS EXISTING DOCS**:
- Check if `docs/` directory exists and what it contains
- Check README.md content and quality
- Identify gaps: undocumented modules, outdated sections, missing references

**STEP 2 — DECIDE APPROACH**:
If `docs/` exists with documentation:
- Plan updates to existing files that are outdated or incomplete
- Plan new files only for uncovered domains
- Plan README.md updates if needed

If `docs/` does NOT exist or is empty:
- Plan a full documentation set organized by functional domain
- Each major module/domain should get its own file
- Plan README.md content (overview, installation, usage, configuration, architecture)
- Typical domain-based files might include: architecture.md, api.md, configuration.md, development.md, testing.md, etc. — adapt to the actual codebase structure

**STEP 3 — CREATE THE PLAN**:
Delegate to the markdown-planner subagent:

`delegate_to_subagents: [{ name: "doc-plan", prompt: "Create a documentation plan for this codebase.\n\nCodebase summary: [paste complete scouting summary]\n\nExisting docs: [list what exists and what it covers]\n\nPlan must specify:\n1. Each file to create or update (full path)\n2. What topics/sections each file must cover\n3. Priority order (most impactful docs first)\n4. Whether README.md needs updates and what sections\n\nEach plan item must be atomic — a single documentation file to create or update.", profile: "markdown-planner" }]`

Start with a 600s timeout. Retrieve the plan with `get_subagent_output`.

Parse the plan into todos using `write_todos` — each documentation file becomes one todo item.

Use `workflow_step` with action `next` when the plan is finalized and todos are written.
