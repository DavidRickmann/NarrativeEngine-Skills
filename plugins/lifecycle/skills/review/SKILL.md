---
name: review
description: Architectural review of recent changes for correctness, security, and design quality. Use for high-level review of design decisions; use /code-review for quick pre-PR checks; use /simplify for code-level quality.
---

Review recent changes for correctness, security, and architectural quality.

Note: For quick pre-PR checks, use `/code-review`. For code-level quality (reuse, naming, efficiency), use the bundled `/simplify` command. This review focuses on higher-level concerns that those tools don't cover.

1. Check git diff (or recent file modifications if not a git repo) to identify what changed
2. For each changed file, evaluate:
   - **Correctness**: Does the logic do what was intended?
   - **Security**: Any injection, XSS, or credential exposure risks?
   - **Architecture**: Does the change fit the system's design? Any coupling concerns?
   - **Consistency**: Does it match existing patterns and conventions?
   - **Edge cases**: Are boundary conditions handled?
3. List any issues found with severity (critical/warning/suggestion)
4. If no issues found, confirm the changes look good

Be honest and thorough. It's better to catch issues now than in production.
