# Analysis

Deep inspection tools for periodic use. Not every session - use these at milestones, before releases, or when things feel off.

## Skills

| Skill           | Type          | Description                                                                                          |
| --------------- | ------------- | ---------------------------------------------------------------------------------------------------- |
| **deep-audit**  | Slash command | Multi-agent audit across up to 9 lenses (security, tests, patterns, etc.). Writes structured report. |
| **techdebt**    | Slash command | Quick scan for duplication, dead code, complexity, naming issues, hardcoded values.                  |
| **scan-latest** | Slash command | Researches latest Claude Code tips, features, and community tools from the web.                      |

## deep-audit

The most sophisticated skill in Narrative Engine. Dispatches 5-9 specialized agents in parallel, each focused on a different concern:

1. Security
2. Error Handling & Resilience
3. Database & Data Integrity
4. Test Coverage & Quality
5. Code Patterns & Consistency
6. API Contracts & Interfaces
7. Concurrency & Performance
8. Configuration & Deployment
9. Cross-Service Consistency

Each agent reads every file in scope, writes structured findings with severity levels and confidence scores. Results are aggregated, deduplicated, cross-referenced, and written to `outputs/audit/YYYY-MM-DD_audit/`.

Create a `.claude/audit.md` file to customize which lenses to run, flag known patterns to skip, and add project-specific checks. A template is included at `skills/deep-audit/references/audit-template.md`.

Designed to run unattended. Start it, walk away, read the report when you're back.

## techdebt

Lighter than deep-audit. Use after completing a feature or when the codebase feels overgrown. Produces an actionable list with file:line locations and priorities.

## scan-latest

Searches the web for recent Claude Code developments and compares them against your current setup. Proposes specific changes. Run monthly to stay current.
