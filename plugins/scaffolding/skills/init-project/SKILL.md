---
name: init-project
description: Scaffolds CLAUDE.md and context structure for a new project. Detects language, framework, test tools, and linters automatically. Use when starting a new project or adding Claude Code workflow to an existing one.
argument-hint: [project type hint, e.g. "web app", "CLI tool", "python library"]
---

You are initializing a project to use the Claude Code workflow. The user may have provided a hint about the project type: $ARGUMENTS

Follow this process:

## 1. Discover

Scan the current directory to understand what already exists:

- Check if CLAUDE.md already exists (if so, confirm before overwriting)
- Check if context/ directory exists
- Look at package.json, pyproject.toml, Cargo.toml, go.mod, etc. to detect language/framework
- Read any existing README.md for project purpose
- Check for test frameworks, linters, build tools already configured
- Check if this is a git repo

## 2. Git Init

If the directory is NOT already a git repo, run `git init` and create a sensible `.gitignore` based on the detected language/framework (e.g., node_modules/ for JS, **pycache**/ for Python, target/ for Rust). If it IS a git repo, skip this step.

## 3. Ask (only what you can't detect)

Based on what you found, ask the user to confirm or fill gaps. Typical questions:

- Project purpose (1 line) - if not obvious from README
- Key conventions to enforce - if not obvious from existing config
- For empty projects: language/framework, project type

Do NOT ask about things you can detect from existing files.

## 4. Generate CLAUDE.md

Create a CLAUDE.md tailored to this project. Follow these rules:

**Structure:**

```markdown
# [Project Name]

## Purpose

[One-line description]

## Workflow

1. Start: Run `/prime` at session start
2. Plan: Use `/create-plan` before complex tasks (3+ files)
3. Execute: Work the plan, use `/implement` for approved plans
4. Review: Run `/code-review` before PRs, `/review` for architecture
5. Archive: Run `/archive` at session end

## Tech Stack

[Language, framework, key dependencies - detected from project files]

## Conventions

[Project-specific rules: coding style, naming, architecture patterns]
[Include detected linter/formatter config references]

## Key Paths

[Important directories and files for navigation]

## Build & Test

[Commands to build, test, lint - detected from project config]

## Rules

- Never guess when you can verify
- Prefer editing existing files over creating new ones
- Keep solutions simple; avoid over-engineering
- When uncertain, ask rather than assume
```

**Guidelines for CLAUDE.md content:**

- Keep it under 50 lines - concise and scannable
- Only include what Claude needs to know, not general documentation
- Reference config files rather than duplicating their contents (e.g., "See .eslintrc for style rules")
- Include actual build/test/lint commands so Claude can verify its own work
- Don't include workflow instructions that are already in global skills

## 5. Create context/ directory

Create these files:

- `context/README.md` - Brief note explaining what this directory is for
- `context/current-priorities.md` - Template with Active/Next Up/Done sections

## 6. Confirm

Show the user what was created and remind them:

- Workflow skills (`/prime`, `/create-plan`, `/review`, etc.) are available
- Run `/prime` to verify everything works
- Edit CLAUDE.md as the project evolves - it should be a living document
- Commit these files to the repo so the whole team benefits

Report: "Project initialized. Run `/prime` to verify."
