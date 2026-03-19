---
name: techdebt
description: Scans the current project for technical debt, cleanup opportunities, and simplification targets. Use after completing a feature or when the codebase feels overgrown.
---

Scan the current project for technical debt and improvement opportunities.

Focus on:

1. **Duplication**: Code or logic repeated in multiple places
2. **Dead code**: Unused functions, variables, imports, or files
3. **Complexity**: Functions that are too long or do too many things
4. **Naming**: Variables or functions with unclear or misleading names
5. **Missing patterns**: Places where existing abstractions should be used but aren't
6. **Configuration**: Hardcoded values that should be configurable

For each finding:

- Location (file:line)
- What the issue is
- Suggested fix (brief)
- Priority (high/medium/low)

Keep the list actionable. Don't report style nitpicks unless they genuinely hurt readability.
