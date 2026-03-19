# Scaffolding

Project initialization that detects your stack and generates workflow structure.

## Skills

| Skill            | Type          | Description                                                                                  |
| ---------------- | ------------- | -------------------------------------------------------------------------------------------- |
| **init-project** | Slash command | Detects language/framework, generates CLAUDE.md, creates context/ directory, initializes git |

## What it creates

```
your-project/
  CLAUDE.md              # Tailored to your detected stack
  context/
    README.md            # Explains what this directory is for
    current-priorities.md # Active/Next Up/Done template
  .gitignore             # If not already present
```

## Usage

```
/init-project              # Auto-detect everything
/init-project web app      # Hint at project type
/init-project python CLI   # More specific hint
```

The skill reads package.json, pyproject.toml, Cargo.toml, go.mod, and other config files to detect your stack. It only asks about things it can't figure out on its own.
