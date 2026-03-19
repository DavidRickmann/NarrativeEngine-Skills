---
name: code-quality
description: Enforces code quality standards automatically when writing, reviewing, or modifying code. Auto-activates during code changes to maintain consistent quality.
---

When writing or modifying code:

1. **Read before writing** - Always read existing code before modifying it
2. **Match existing patterns** - Follow the conventions already in the codebase
3. **Verify your work** - Run tests, check output, use available verification methods
4. **Keep it simple** - Minimum viable complexity; three similar lines beat a premature abstraction
5. **Security first** - Check for injection, XSS, credential exposure in every change
6. **No phantom features** - Only implement what was asked; don't add extra "improvements"
7. **Clean boundaries** - Validate at system edges (user input, APIs), trust internal code
8. **Self-review** - Before presenting changes, mentally walk through edge cases
