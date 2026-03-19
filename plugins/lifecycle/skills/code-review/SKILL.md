---
name: code-review
description: Quick multi-agent code review for pre-PR sanity checks. Lighter and faster than /deep-audit - runs 5 parallel agents focused on bugs, security, tests, performance, and simplification. Returns inline results, no output files. Use after completing a feature or before opening a PR.
---

# Code Review

Fast, parallel code review for day-to-day use. Designed to catch real issues before a PR, not produce a comprehensive audit report. Runs in 2-3 minutes, not 10-15.

**When to use this vs other review tools:**

- `/code-review` - Quick pre-PR sanity check (this skill)
- `/review` - Architectural review of design decisions and correctness
- `/simplify` - Code-level quality, reuse, and efficiency (bundled with Claude Code)
- `/deep-audit` - Comprehensive multi-lens audit for milestones or unfamiliar codebases

## Scope Detection

1. Check what's changed. In priority order:
   - If the user specified files or a scope, use that
   - If there are uncommitted changes, review those: `git diff --name-only HEAD`
   - If the current branch differs from main/master, review the branch diff: `git diff --name-only main...HEAD`
   - If nothing has changed, ask the user what to review

2. Read CLAUDE.md and `.claude/audit.md` (if they exist) for project context and known patterns to skip.

3. Briefly tell the user what you're reviewing and how many files, then proceed immediately. Do not ask for confirmation.

## Agent Dispatch

Spawn exactly 5 agents in parallel using the Agent tool. Each agent gets the file list, project context, and a focused concern.

All agents use `subagent_type: "Explore"` (read-only). Each agent should read every changed file in scope, not sample.

### Agent 1: Bug Hunter

Look for:

- Logic errors, off-by-one, wrong comparisons
- Null/undefined access without guards
- Missing return statements or wrong return values
- Incorrect error handling (swallowed exceptions, wrong error types)
- Race conditions in async code
- Resource leaks (unclosed handles, connections, listeners)

### Agent 2: Security

Look for:

- Injection vulnerabilities (SQL, command, template, XSS)
- Hardcoded secrets, tokens, keys
- Missing input validation at system boundaries
- Auth/authz gaps
- Path traversal, SSRF, open redirects
- Insecure deserialization

### Agent 3: Test Gaps

Look for:

- Changed code paths that have no test coverage
- Edge cases in the changed code that aren't tested
- Tests that assert trivial things (testing the mock, not the code)
- Missing error path tests
- Property-based testing opportunities for serialization, parsing, normalization

### Agent 4: Performance & Correctness

Look for:

- N+1 query patterns
- Unnecessary allocations in loops or hot paths
- Blocking calls in async contexts
- Missing indexes implied by new queries
- Unbounded growth (collections, queues, listeners that never clean up)
- Incorrect caching (stale data, missing invalidation)

### Agent 5: Simplification

Look for:

- Code that duplicates existing utilities or helpers in the codebase
- Overly complex logic that could be simplified
- Abstractions that add complexity without value
- Dead code introduced by the changes
- Inconsistency with established patterns elsewhere in the codebase

### Agent Prompt Template

```
You are reviewing recent code changes in [PROJECT] for [CONCERN].

## Project Context
[From CLAUDE.md and audit.md]

## Files to Review
[List of changed files with paths]

## Known Patterns (Don't Flag)
[From audit.md if it exists]

## Output Format
For each finding, write:

**[SEVERITY: critical/high/medium/low] [Short title]**
- **File:** path/to/file.py:line_number
- **What:** One sentence describing the issue
- **Why it matters:** One sentence on impact
- **Fix:** Brief, actionable suggestion

Sort by severity (critical first). If you find nothing, say "No issues found" - don't invent findings to justify your existence.

Do NOT flag:
- Style preferences or formatting
- Patterns documented as intentional
- Hypothetical issues with no concrete path
- TODO/FIXME comments (those are tracked separately)
```

## Results

After all agents complete, present results inline (no output files). Structure as:

1. **Summary line**: "Reviewed N files. Found X critical, Y high, Z medium, W low issues."

2. **Critical/High findings first** - these need attention before merging

3. **Medium/Low findings** - grouped, briefly

4. If multiple agents flagged the same issue, mention it once with a note that it was caught by multiple reviewers (reinforces confidence).

5. If nothing was found, say so clearly. A clean review is a valid outcome.

Keep the presentation concise. The user wants to know "is this safe to merge?" not read a 500-line report.
