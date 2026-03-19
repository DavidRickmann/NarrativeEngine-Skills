# Lifecycle

The core of Narrative Engine. Seven skills that guide you through a complete coding session.

```
/prime ........... orient (load context, memory, priorities)
    |
/create-plan ..... think (explore, interview, plan)
    |
/implement ....... build (execute plan, run tests)
    |
/code-review ..... verify (5-agent parallel review)
/review .......... verify (architectural fitness)
    |
/learn ........... reflect (capture lessons, update rules)
/archive ......... preserve (write memory, list open items)
```

## Skills

| Skill           | Type          | Description                                                                                |
| --------------- | ------------- | ------------------------------------------------------------------------------------------ |
| **prime**       | Slash command | Loads CLAUDE.md, context directory, memory, and recent activity at session start           |
| **create-plan** | Slash command | Interviews you (optional), explores codebase, identifies approaches, drafts execution plan |
| **implement**   | Slash command | Executes an approved plan step by step with test verification                              |
| **code-review** | Slash command | Quick 5-agent parallel review: bugs, security, tests, performance, simplification          |
| **review**      | Slash command | Deeper architectural review of correctness, security, design fitness, and consistency      |
| **learn**       | Slash command | Captures session lessons, reviews error logs, proposes CLAUDE.md updates                   |
| **archive**     | Slash command | Writes session summary to memory, checks CLAUDE.md, lists open items                       |

## How they connect

- `/prime` reads what `/archive` wrote last time
- `/implement` executes what `/create-plan` drafted
- `/learn` proposes CLAUDE.md changes that `/prime` will load next session
- `/code-review` and `/review` complement each other (speed vs depth)

Not every session needs every step. But the more you use, the more knowledge compounds.
