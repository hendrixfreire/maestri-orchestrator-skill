# Maestri Orchestrator Skill

A model-agnostic orchestration skill for agents acting as the Maestro inside a Maestri canvas. It follows the open [Agent Skills specification](https://agentskills.io/specification), so compatible runtimes can load the same repository without provider-specific prompt formats.

It turns the Maestro into a coordination layer rather than an implementation agent. The Maestro writes contracts, partitions work, recruits from the presets actually available in the workspace, maintains traceability, delegates execution, and integrates evidence. A dedicated `git-master` agent owns all Git and GitHub operations.

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
├── SKILL.md
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

Install the repository as a global skill using the skill directory supported by your agent runtime. The installed directory must preserve `SKILL.md`, `references/`, and `templates/` together.

Examples:

```bash
# Clone to a temporary or permanent location
git clone https://github.com/hendrixfreire/maestri-orchestrator-skill.git

# Copy or symlink the repository into your runtime's global skills directory
# The destination varies by agent; consult that runtime's skill documentation.
```

No specific agent runtime, provider, or preset is required. The executing environment must expose the `maestri` CLI and Maestro Mode.

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
