---
id: doc-reviewing
name: Doc Reviewing
emoji: "👁️"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - doc-reviewer
---

Review all created/updated documentation for accuracy and quality.

Spawn 1-2 doc-reviewer subagents to verify the documentation:

`delegate_to_subagents: [{ name: "doc-review-1", prompt: "Review all documentation files that were created or updated in this session. Cross-reference every claim against the actual source code. Check for accuracy, completeness, freshness, structure, and code examples. Be thorough — read the source files that the docs describe.", profile: "doc-reviewer" }]`

If the codebase is large, split review across 2 subagents:
- One reviews the README and architecture-level docs
- One reviews the API/module-level docs

Collect results using `get_subagent_output` for each session.

**SYNTHESIZE FINDINGS**: Categorize each finding:
- **CRITICAL**: Documentation describes non-existent APIs or fundamentally wrong behavior
- **HIGH**: Missing documentation for important public APIs, incorrect examples
- **MEDIUM**: Outdated references, missing cross-references, structural issues
- **LOW**: Style improvements, minor wording suggestions

**DECISION**:
- If MEDIUM, HIGH, or CRITICAL issues exist: use `write_todos` to add fix tasks for each issue, then `workflow_step` with action `loop` to return to doc-writing
- If only LOW issues or no issues: mark all todos complete and `workflow_step` with action `next`

Use `workflow_step` with action `next` when review is clean, or `loop` to fix issues.
