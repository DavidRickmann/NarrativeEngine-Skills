---
name: create-plan
description: Research and draft an execution plan before making changes. Use before complex tasks to align on approach, explore options, and define verification steps.
argument-hint: [task description]
---

Enter plan mode and create a detailed execution plan for the task: $ARGUMENTS

Follow this process:

1. **Understand**: Restate the goal in your own words. Then use AskUserQuestion to ask whether the user wants a requirements interview first. Offer two options:
   - "Interview me first" - for ambiguous or complex features where requirements need fleshing out
   - "Skip to planning" - for well-defined tasks where the scope is already clear
     If the user chooses the interview, conduct a focused interview using AskUserQuestion:
   - Ask about technical requirements, edge cases, UX expectations, and tradeoffs
   - Don't ask obvious questions - dig into the hard parts they might not have considered
   - Keep going until you've covered the key decisions (typically 3-5 rounds)
   - Summarize what you learned before moving to research
2. **Research**: Explore the codebase/context thoroughly. Read relevant files, search for patterns, understand existing architecture.
3. **Identify Options**: List 2-3 possible approaches with trade-offs for each.
4. **Recommend**: Pick the best approach and explain why.
5. **Detail the Plan**:
   - Files to create/modify (with specific changes)
   - Order of operations
   - Dependencies and risks
   - Verification steps (how we'll know it works)
6. **Estimate Scope**: Small (1-3 files), Medium (4-8 files), Large (9+ files)

Write the plan clearly so it can be executed with /implement after approval.
Do NOT make any changes yet - planning only.
