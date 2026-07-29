# AGENTS.md — Maestri Orchestrator Skill

## Primary reference

Read `SKILL.md` before modifying this project. Detailed authority boundaries live in `references/operating-contract.md`.

## Golden rule

Keep the skill model-, provider-, preset-, and project-agnostic; execution belongs to recruited agents, while Git and GitHub operations belong to `git-master`.

## Validation

Confirm that no model command, skip-permission flag, fixed base branch, project path, or agent-runtime-specific installation path has been introduced.
