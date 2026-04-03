---
description: Create or update a scoped phased development plan document for the current task
argument-hint: [task summary or goal]
---

Use the workflow in @dev-plan-generator/SKILL.md to create or update a lightweight phased development plan.

Requirements:
- Read relevant project `.md` files first.
- Fix the goal, scope, exclusions, references, and common rules near the top.
- Keep the document lightweight and implementation-oriented.
- Use `dev-plan/implement_YYYYMMDD_HHMMSS.md` for a new workstream.
- Update the same document while the same workstream is in progress.
- Create a new file for a new refactor or new development effort.
- Break work into ordered phases.
- Every phase must include implementation tasks, self-tests, issue tracking, and completion conditions.
- Do not move to the next phase until the current phase self-tests are complete.
- Do not expand beyond the documented scope.

When a new file is needed, you may scaffold it with @dev-plan-generator/scripts/new_dev_plan.py.

User request / current task:
$ARGUMENTS
