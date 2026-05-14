---
id: code-reviewing
name: Code Reviewing
emoji: "👁️"
tools:
  blacklist:
    - edit
    - write
availableProfiles:
  - efficiency-reviewer
  - security-reviewer
  - ui-ux-reviewer
---

BIFROST STATUS CHECK: If this workflow used the bifrost-planner (check todos for rune IDs), run `bf show <main-rune-id>` to verify all runes are fulfilled. If they are not fulfilled, but the work is complete, fulfill them now. DO NOT leave bifrost dirty with unfulfilled runes. Report the overall rune tree status in your summary at the end.

Spawn 1-4 parallel review specialists using `delegate_to_subagents`:
- { name: "efficiency-review", prompt: "Review the codebase for performance and resource efficiency issues.", profile: "efficiency-reviewer" }
- { name: "security-review", prompt: "Audit the codebase for security vulnerabilities.", profile: "security-reviewer" }
- { name: "ui-ux-review", prompt: "Evaluate UI/UX quality and check for any broken user-facing behavior.", profile: "ui-ux-reviewer" }

Collect all review results using `get_subagent_output` for each session.

SYNTHESIZE FINDINGS: Categorize each finding by severity:
- **CRITICAL**: Extremely high importance issue (unreleasable)
- **HIGH**: Must fix (bugs, security vulnerabilities, critical performance issues)
- **MEDIUM**: Should fix (code smells, suboptimal patterns, missing error handling)
- **LOW**: Nice to have (style improvements, minor optimizations)

Create a summary of all findings with severity levels. This summary determines whether the fixing phase is needed.

Use `workflow_step` with action `next` when the review synthesis is complete whether or not there are issues found.
