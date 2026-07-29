# Maestri Orchestrator Skill

A model-agnostic orchestration skill for agents acting as the Maestro inside a Maestri canvas. It follows the open [Agent Skills specification](https://agentskills.io/specification), so different compatible agent runtimes hosted inside Maestri can load the same repository without provider-specific prompt formats. It is not an orchestration skill for sessions running outside Maestri.

It turns the Maestro into a coordination layer rather than an implementation agent. The Maestro writes contracts, partitions work, recruits from the presets actually available in the workspace, maintains traceability, delegates execution, and integrates evidence. A dedicated `git-master` agent owns all Git and GitHub operations.

The repository ships the same operating contract in two forms:

- `SKILL.md` — progressively disclosed Agent Skill with templates and references;
- `SYSTEM_PROMPT.md` — self-contained system prompt for agent runtimes or Maestri presets that accept a prompt directly.

## Design principles

- Model- and provider-agnostic: no model names, commands, effort levels, or permission-bypass flags are prescribed.
- Execution by recruited agents: the Maestro does not modify product code, run commands, tests, Git, or GitHub operations.
- File-boundary ownership: active agents do not share writable paths.
- Auditable autonomy: reversible decisions are recorded; consequential choices return to the user.
- Minimal necessary discovery: use structured grilling only when ambiguity blocks a reliable contract.
- User-owned canvas: destructive node operations are never initiated for cleanup.

## Included files

```text
.
├── README.md
├── SKILL.md
├── SYSTEM_PROMPT.md
├── references/
│   └── operating-contract.md
└── templates/
    ├── autonomous-decisions.md
    ├── blockers-and-questions.md
    ├── executor-spec.md
    ├── executor-tasks.md
    ├── git-master-role.md
    └── project-spec.md
```

## Installation

To make the skill available to every compatible agent that a runtime launches inside Maestri, install it in that runtime's user/global skill scope when supported. Runtimes that expose only project- or workspace-scoped skills require installation in each relevant scope.

The installed directory must be named exactly `maestri-orchestrator-skill` because the Agent Skills specification requires the parent directory to match the `name` field. Preserve `SKILL.md`, `references/`, and `templates/` together.

Examples:

```bash
# Clone to a temporary or permanent location
git clone https://github.com/hendrixfreire/maestri-orchestrator-skill.git

# Copy or symlink the whole repository as a directory named
# maestri-orchestrator-skill in the runtime's supported skill scope.
# The parent path varies by agent; consult that runtime's documentation.
```

No specific agent runtime, provider, or preset is required. Activation is valid only for an agent terminal running inside Maestri with the `maestri` CLI and Maestro Mode available.

## System prompt usage

Use the complete contents of `SYSTEM_PROMPT.md` as the system prompt for the terminal configured as Maestro. The file is self-contained: it does not require the runtime to support Agent Skills or load the repository's templates.

Do not assign this prompt to implementation agents. Their roles are created by the Maestro from the project contract and ownership boundaries.

## Optional discovery skills

When installed, the orchestrator prefers:

1. `wayfinder` for large efforts with decision fog;
2. `grill-me` for consequential ambiguity, one question at a time;
3. `grill-with-docs` when supplied documents govern decisions;
4. `to-questionnaire` for several independent decisions.

These are optional. Their absence does not block orchestration; the Maestro must still avoid unnecessary questioning.

## Git workflow

The skill requires a dedicated logical agent named `git-master`. This agent discovers repository conventions and owns branches, staging, commits, remotes, pushes, gates, PR preparation, and GitHub operations. Implementers only hand off changed paths and evidence.

External actions remain approval-gated: PR creation, merge, deployment, release, publication, and destructive Git operations require explicit user authorization.

## License

MIT
