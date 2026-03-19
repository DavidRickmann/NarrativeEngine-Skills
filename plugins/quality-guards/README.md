# Quality Guards

Auto-activating skills that run in the background. You don't invoke these - Claude activates them based on what you're doing.

## Skills

| Skill                    | Activates when                      | What it does                                                                           |
| ------------------------ | ----------------------------------- | -------------------------------------------------------------------------------------- |
| **code-quality**         | Writing or modifying code           | Enforces read-first, pattern matching, simplicity, security-first, no phantom features |
| **research-methodology** | Researching or investigating topics | Enforces 3-source cross-referencing, structured findings, source citation              |
| **session-management**   | Throughout sessions                 | Nudges toward lifecycle pattern, proactive compaction, one-task-per-session            |
| **claudiness**           | Reviewing text for naturalness      | Checks writing against known AI patterns via external CLI tool                         |

## How auto-activation works

When you install this plugin, Claude reads each skill's description and activates it when the context matches. You'll notice the effects as:

- Fewer "phantom features" (code you didn't ask for)
- Research that cites sources and flags contradictions
- Gentle nudges to `/prime` at session start and `/archive` at session end
- Writing that avoids the patterns readers associate with AI

## claudiness dependency

The claudiness skill requires an external CLI tool:

```bash
pipx install claudiness
```

Without it, the skill will notify you that the tool is missing and suggest installation. The other three quality guards have zero dependencies.

## These aren't linters

Quality guards don't reject or block anything. They shape how Claude approaches work - they're principles, not gatekeepers. The effect is subtle but cumulative over sessions.
