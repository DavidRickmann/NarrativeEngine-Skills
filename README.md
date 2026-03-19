# Narrative Engine

An opinionated Claude Code workflow system which compounds knowledge over time.

Every coding session tells a story: orient, plan, build, review, reflect, archive.

```
/init-project .... scaffold a new project
      |
   /prime ......... orient at session start
      |
/create-plan ...... think before building
      |
 /implement ....... execute the plan
      |
/code-review ...... quick pre-PR check (5 parallel agents)
   /review ........ deeper architectural review
      |
   /learn ......... capture what surprised you
   /archive ....... preserve context for next time
```

Quality guards run underneath the whole arc, automatically activating when you write code, research topics, or produce text.

## Install

```bash
# Add the marketplace
/plugin marketplace add DavidRickmann/NarrativeEngine-Skills

# Install everything
/plugin install narrative-engine-lifecycle
/plugin install narrative-engine-scaffolding
/plugin install narrative-engine-analysis
/plugin install narrative-engine-quality-guards

# Or just the parts you want - lifecycle is the core
/plugin install narrative-engine-lifecycle
```

## What's in the box

| Plugin             | Skills                                                             | What it does                                                  |
| ------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------- |
| **lifecycle**      | prime, create-plan, implement, code-review, review, learn, archive | The core workflow arc. Start here.                            |
| **scaffolding**    | init-project                                                       | Detects your stack, generates CLAUDE.md and context structure |
| **analysis**       | deep-audit, techdebt, scan-latest                                  | Multi-agent code audits, debt scanning, community research    |
| **quality-guards** | code-quality, research-methodology, session-management, claudiness | Auto-activating principles that run in the background         |

## Quick start

See [docs/getting-started.md](docs/getting-started.md) for a full walkthrough.

The short version:

1. Install the lifecycle and scaffolding plugins
2. Run `/init-project` in your project
3. Start every session with `/prime`
4. Use `/create-plan` before anything touching 3+ files
5. Run `/code-review` before opening PRs
6. End sessions with `/archive`

## Philosophy

These skills are opinionated. They enforce a specific workflow because that workflow works. If you want a collection of independent utilities you can mix and match, there are 20,000 skills on the marketplace for that.

This system is built around three ideas:

1. **Sessions have a lifecycle.** Orient first, plan before building, review before shipping, reflect before closing. Skipping steps is how you end up with context loss and repeated mistakes.

2. **Knowledge compounds.** `/learn` and `/archive` write to memory. `/prime` reads it back. Each session starts a little smarter than the last. CLAUDE.md evolves with corrections. The system gets better the more you use it.

3. **Quality is ambient, not manual.** The quality-guards plugin activates automatically during code changes, research, and writing. You don't invoke them - they're just there, keeping standards consistent without adding friction.

Read more in [docs/philosophy.md](docs/philosophy.md).

## Skill reference

See [docs/skill-reference.md](docs/skill-reference.md) for a one-page cheat sheet of every skill.

## Dependencies

Most skills have zero external dependencies - they use Claude Code's built-in tools.

**claudiness** (in quality-guards) requires the `claudiness` CLI tool:

```bash
pipx install claudiness
```

## License

MIT - see [LICENSE](LICENSE).
