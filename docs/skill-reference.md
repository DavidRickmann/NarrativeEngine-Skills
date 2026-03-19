# Skill Reference

Quick-reference for every skill in Narrative Engine.

## Lifecycle (narrative-engine-lifecycle)

The core workflow arc. Use these every session.

| Skill           | Command               | When to use                                                                                        |
| --------------- | --------------------- | -------------------------------------------------------------------------------------------------- |
| **prime**       | `/prime`              | Start of every session. Loads CLAUDE.md, context, memory, priorities.                              |
| **create-plan** | `/create-plan [task]` | Before complex tasks (3+ files). Interviews you, explores codebase, drafts plan with options.      |
| **implement**   | `/implement`          | After approving a plan. Executes step by step, runs tests before/after.                            |
| **code-review** | `/code-review`        | Before opening a PR. 5 parallel agents check bugs, security, tests, perf, simplification. 2-3 min. |
| **review**      | `/review`             | After significant changes. Architectural review - correctness, security, design fitness.           |
| **learn**       | `/learn`              | After surprises or mistakes. Captures lessons, proposes CLAUDE.md updates.                         |
| **archive**     | `/archive`            | End of session. Writes summary to memory, lists open items for next time.                          |

**The arc:** `/prime` -> `/create-plan` -> `/implement` -> `/code-review` -> `/review` -> `/learn` -> `/archive`

Not every session needs every step. Quick fixes can skip planning. Exploratory sessions can skip review. But the more steps you use, the more knowledge compounds.

## Scaffolding (narrative-engine-scaffolding)

| Skill            | Command                | When to use                                                                                                               |
| ---------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **init-project** | `/init-project [type]` | New project setup or adding workflow to existing project. Detects stack, generates CLAUDE.md, creates context/ directory. |

## Analysis (narrative-engine-analysis)

Periodic deep inspection. Not for every session.

| Skill           | Command                 | When to use                                                                                      |
| --------------- | ----------------------- | ------------------------------------------------------------------------------------------------ |
| **deep-audit**  | `/deep-audit`           | Milestones, releases, onboarding. Multi-agent audit across 9 lenses. 10-15 min, runs unattended. |
| **techdebt**    | `/techdebt`             | After features or when things feel messy. Scans for duplication, dead code, complexity.          |
| **scan-latest** | `/scan-latest [period]` | Monthly. Researches latest Claude Code tips, features, and community tools.                      |

## Quality Guards (narrative-engine-quality-guards)

Auto-activating. You don't invoke these - they run in the background.

| Skill                    | Activates when                 | What it does                                                                                    |
| ------------------------ | ------------------------------ | ----------------------------------------------------------------------------------------------- |
| **code-quality**         | Writing or modifying code      | Enforces read-first, pattern matching, simplicity, security, no phantom features                |
| **research-methodology** | Researching or investigating   | Enforces cross-referencing, source citation, structured findings                                |
| **session-management**   | Throughout sessions            | Nudges toward lifecycle pattern, proactive compaction, one-task-per-session                     |
| **claudiness**           | Reviewing text for naturalness | Checks against known AI writing patterns via external tool. Requires `pipx install claudiness`. |

## Review tool comparison

| Tool           | Speed     | Depth              | Output         | Best for                         |
| -------------- | --------- | ------------------ | -------------- | -------------------------------- |
| `/code-review` | 2-3 min   | Focused on changes | Inline results | Pre-PR sanity check              |
| `/review`      | 3-5 min   | Architectural      | Inline results | Design decision review           |
| `/simplify`    | 2-3 min   | Code-level         | Auto-fixes     | Reuse, naming, efficiency        |
| `/deep-audit`  | 10-15 min | Comprehensive      | Report files   | Milestones, releases, onboarding |
