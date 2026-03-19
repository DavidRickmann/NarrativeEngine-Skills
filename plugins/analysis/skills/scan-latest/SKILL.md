---
name: scan-latest
description: Researches latest Claude Code tips, features, and best practices from the web. Use periodically to stay current with new versions, community tools, and workflow innovations.
argument-hint: [time period, e.g. "7 days" or "30 days"]
---

Research the latest Claude Code tips, techniques, and best practices from the past $ARGUMENTS.

If no time period is given, default to "30 days".

## Process

1. **Check last scan**: Look for a scan log file in `context/scan-log.md` or the project root. If found, read it to see when this last ran and what was found. If no scan log exists, note that this is the first scan.
2. **Search broadly** across multiple sources:
   - Web search for recent Claude Code tips, CLAUDE.md patterns, workflow innovations
   - Search for new Claude Code skills, hooks, and community tools on GitHub
   - Look for discussions on Reddit (r/ClaudeAI, r/ChatGPTCoding) about Claude Code workflows
   - Check for Anthropic announcements or doc updates
3. **Filter for actionable insights**: Only report things we can actually use or implement
4. **Compare against what we already have**: Read the current CLAUDE.md, installed skills, and memory to avoid reporting things already in use
5. **Categorize findings**:
   - **New techniques** - Workflow patterns or approaches we haven't tried
   - **Tools & skills** - Community tools worth installing or adapting
   - **Configuration tips** - Settings, hooks, or optimizations
   - **Corrections** - Things we're doing that the community has found better alternatives for
6. **Update scan log**: If a `context/` directory exists, append today's date and summary to `context/scan-log.md` (create it if needed). Otherwise, write to `scan-log.md` in the project root.
7. **Propose changes**: For each actionable finding, suggest specific changes to the setup

Present findings as a concise briefing with links. Ask before making any changes to configuration.
