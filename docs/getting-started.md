# Getting Started

## Installation

```bash
# Add the marketplace
/plugin marketplace add DavidRickmann/NarrativeEngine-Skills

# Install the core workflow (start here)
/plugin install narrative-engine-lifecycle

# Add project scaffolding
/plugin install narrative-engine-scaffolding

# Add deep analysis tools
/plugin install narrative-engine-analysis

# Add auto-activating quality guards
/plugin install narrative-engine-quality-guards
```

## Your first session: a walkthrough

You've just cloned a project you've never seen before. Here's how the Narrative Engine workflow handles your first hour.

### 1. Scaffold the project

```
/init-project web app
```

`init-project` scans the directory, detects the language/framework from config files, and generates:

- **CLAUDE.md** - tailored to your stack, with build/test/lint commands
- **context/** directory - a place for priorities and reference docs
- **.gitignore** - if the project doesn't have one

It only asks questions about things it can't detect.

### 2. Orient

```
/prime
```

`prime` reads CLAUDE.md, scans the context directory, checks memory from previous sessions, and reports back:

- What the project is
- What context was loaded
- Current priorities
- Anything it remembers from last time

This takes seconds. It prevents the "where was I?" confusion that wastes the first 10 minutes of every session.

### 3. Plan your task

```
/create-plan add user authentication
```

`create-plan` offers to interview you first (for ambiguous tasks) or skip to planning (for well-defined ones). It then:

- Explores the codebase to understand existing patterns
- Identifies 2-3 approaches with tradeoffs
- Recommends one and explains why
- Lists every file to create/modify, in order
- Defines verification steps

The plan is written clearly enough that `/implement` can execute it.

### 4. Build

```
/implement
```

`implement` picks up the approved plan and works through it:

- Runs existing tests first (baseline)
- Makes changes step by step
- Stops if something unexpected happens
- Re-runs tests after changes
- Summarizes everything that was done

### 5. Review before merging

```
/code-review
```

`code-review` automatically detects what changed (uncommitted changes, branch diff, or ask you) and dispatches 5 agents in parallel:

| Agent          | Focus                                         |
| -------------- | --------------------------------------------- |
| Bug Hunter     | Logic errors, null access, race conditions    |
| Security       | Injection, secrets, auth gaps                 |
| Test Gaps      | Untested paths, weak assertions               |
| Performance    | N+1 queries, unbounded growth, blocking calls |
| Simplification | Duplication, unnecessary complexity           |

Results come back in 2-3 minutes as a concise summary: "Reviewed N files. Found X critical, Y high, Z medium, W low issues."

For deeper architectural review (does this change fit the system's design?), also run:

```
/review
```

### 6. Reflect

```
/learn
```

`learn` reviews the session and captures:

- What worked and what didn't
- New knowledge about the codebase
- Process improvements for next time
- CLAUDE.md updates needed based on any corrections

### 7. Archive and close

```
/archive
```

`archive` writes a session summary to memory, updates CLAUDE.md if corrections were made, and lists open items for next time. When you run `/prime` tomorrow, all of this context is waiting for you.

## The analysis tools

These are for periodic use, not every session.

### /deep-audit

A comprehensive multi-agent code audit. Dispatches 5-9 specialized agents (security, error handling, database, testing, patterns, API contracts, concurrency, configuration, cross-service) that read every file in scope. Produces a structured report in `outputs/audit/`.

Best used at milestones, before releases, or when onboarding to an unfamiliar codebase. Takes 10-15 minutes. You can walk away while it runs.

Create a `.claude/audit.md` file (template included in the plugin) to tell the audit which patterns are intentional, which areas are high-risk, and which lenses to skip.

### /techdebt

Quick scan for duplication, dead code, complexity, naming issues, and hardcoded values. Use after completing a feature or when the codebase feels like it needs a cleanup pass.

### /scan-latest

Searches the web for recent Claude Code tips, features, and community tools. Compares findings against your current setup and proposes changes. Run monthly to stay current.

## The quality guards

These activate automatically - you don't invoke them.

- **code-quality** - activates during code changes
- **research-methodology** - activates during research tasks
- **session-management** - activates during session lifecycle
- **claudiness** - activates during text review (requires `claudiness` CLI: `pipx install claudiness`)

You'll notice their effect as consistent coding standards, well-sourced research, and writing that doesn't sound like it was generated by AI.
