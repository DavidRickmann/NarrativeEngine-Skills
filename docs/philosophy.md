# Philosophy

## Why an opinionated workflow?

Most Claude Code skill collections are grab bags. Install 50 skills, use 3 of them, forget the rest exist. Clutter with a package manager.

Narrative Engine is opinionated because opinions are what make a workflow repeatable. When you don't have to decide _how_ to start a session, you just start. When the review step is built into the arc, you don't skip it because you're tired. The structure frees you from decision fatigue so you can focus on the actual problem.

## The narrative arc

Every coding session tells a story. It has a beginning, a middle, and an end. When you skip the beginning (orientation) you make assumptions. When you skip the end (reflection) you lose knowledge.

The arc:

**Orient** (`/prime`) - Load context. Read CLAUDE.md, check priorities, recall what happened last time. This takes 30 seconds and prevents 30 minutes of confusion.

**Plan** (`/create-plan`) - Think before building. Explore the codebase, identify options, pick an approach, write it down. Plans survive context compaction. Assumptions don't.

**Build** (`/implement`) - Execute the plan step by step. Run tests before and after. Stop if something unexpected comes up. The plan is a contract - deviations need discussion.

**Review** (`/code-review`, `/review`) - Two levels. `/code-review` is a quick 5-agent parallel scan for bugs, security holes, test gaps, performance issues, and simplification opportunities. Takes 2-3 minutes. `/review` goes deeper into architectural fitness. Use both before shipping.

**Reflect** (`/learn`) - What worked? What didn't? What surprised you? What should go into CLAUDE.md so the mistake doesn't happen again? This is where the system compounds.

**Preserve** (`/archive`) - Write a session summary to memory. List open items. Update CLAUDE.md if corrections were made. Now the next session starts with everything this session learned.

## Compounding knowledge

The real power isn't any individual skill. It's the feedback loop:

1. `/learn` captures lessons and proposes CLAUDE.md updates
2. `/archive` writes them to memory
3. `/prime` reads memory and CLAUDE.md at the next session start
4. Claude's behavior improves because the instructions improved

Over weeks, CLAUDE.md becomes a living rulebook shaped by real mistakes and real corrections. Memory accumulates context that would otherwise be lost between sessions. Each session stands on the shoulders of every session before it.

## Ambient quality

The quality-guards plugin runs automatically. You don't invoke these skills - Claude activates them based on context:

- **code-quality** activates during code changes. Enforces "read before writing," pattern matching, security-first, no phantom features.
- **research-methodology** activates during research tasks. Enforces cross-referencing, source citation, structured findings.
- **session-management** activates during sessions. Nudges you toward the lifecycle pattern without getting in the way.
- **claudiness** activates during text review. Checks writing against known AI patterns that readers find off-putting. (Requires the `claudiness` CLI tool.)

These aren't linters or gatekeepers. They're principles that shape how Claude approaches work. The effect is subtle but cumulative - fewer phantom features, better-sourced research, more natural writing.

## When to break the rules

Not every task needs the full arc. Quick one-off questions, small fixes, exploratory research - just do them. The workflow is for sessions with real stakes: features, refactors, debugging sessions, code that's going to ship.

Use your judgment. The arc is a default, not a cage.
