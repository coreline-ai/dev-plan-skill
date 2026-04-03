# dev-plan-skill

Reusable development-plan tooling for both **Codex** and **Claude Code**.

## Layout

- `dev-plan-generator/`
  - Codex skill folder (`SKILL.md`, `agents/openai.yaml`, `scripts/new_dev_plan.py`)
- `.claude/commands/dev-plan.md`
  - Claude Code project slash command
- `.claude/agents/dev-plan-generator.md`
  - Claude Code custom subagent

## Codex

Install or copy `dev-plan-generator/` into your Codex skills directory.

Example destination:
- `~/.codex/skills/dev-plan-generator`

## Claude Code

Claude Code supports:
- project commands in `.claude/commands/`
- project subagents in `.claude/agents/`

This repository already includes both forms.

If you want them project-local, copy these paths into your repository root:
- `.claude/commands/dev-plan.md`
- `.claude/agents/dev-plan-generator.md`
- `dev-plan-generator/` (shared reference + script bundle)

## Shared idea

The workflow is the same in both tools:
- read relevant project markdown docs first
- fix purpose / scope / exclusions / references / rules at the top
- create or update `dev-plan/implement_YYYYMMDD_HHMMSS.md`
- use phased checkbox tasks with per-phase self-tests
- create a new plan file for each new workstream so history accumulates
