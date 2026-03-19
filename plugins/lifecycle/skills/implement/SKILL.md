---
name: implement
description: Executes an approved plan from the current conversation. Use after /create-plan to carry out the agreed approach step by step.
---

Execute the plan that was just approved in this conversation.

Follow these rules:

1. **First run the tests** - If the project has a test suite, run it before making any changes to establish a baseline
2. Work through the plan step by step in the agreed order
3. After each major step, briefly confirm what was done
4. If you encounter something unexpected that deviates from the plan, STOP and explain before continuing
5. **Re-run tests after changes** - Verify nothing was broken and new functionality works
6. Run any additional verification steps defined in the plan
7. At the end, provide a summary of all changes made

If no plan exists in this conversation, ask the user what they'd like implemented or suggest running /create-plan first.
