---
name: claudiness
description: Detects AI writing patterns in text using the claudiness tool. Auto-activates when reviewing text for naturalness or when the user wants to check if writing sounds like AI. Requires the claudiness CLI tool to be installed separately.
---

The `claudiness` tool detects AI writing patterns by checking text against known markers that readers associate with AI-generated content. It doesn't use AI judgment - it searches for specific patterns that people identify as "AI-sounding."

## Prerequisites

This skill requires the `claudiness` CLI tool. Install it with:

```bash
pipx install claudiness
```

If the tool is not installed, inform the user and point them to the install command above.

## When to use

- User asks to check if text "sounds like AI" or "sounds like Claude"
- Reviewing written content for naturalness
- Before finalizing any long-form writing output
- When the user says "check for claudiness" or similar

## How to invoke

```bash
# Analyze a file
claudiness path/to/file.md

# Analyze inline text
claudiness --text "Your text here"

# Pipe text
echo "text" | claudiness

# Verbose mode (shows which patterns matched)
claudiness path/to/file.md --verbose

# Check specific model style
claudiness path/to/file.md --model claude

# JSON output for programmatic use
claudiness path/to/file.md --json
```

## Interpreting results

- Score 0-30: Natural sounding, low AI fingerprint
- Score 31-60: Some AI patterns detected, may warrant editing
- Score 61-100: Strong AI writing patterns, should be rewritten

## When flagged

If text scores above 40, suggest specific rewrites:

1. Run with --verbose to see which patterns triggered
2. Identify the specific phrases flagged (hedging, balance markers, Claude vocabulary)
3. Propose concrete rewrites that preserve meaning but sound more natural
4. Re-run claudiness on the rewritten version to verify improvement
