---
name: maestri-orchestrator-skill
description: Orchestrate multi-agent work inside Maestri. Use when acting as the Maestro to contract, partition, staff, trace, and integrate work without implementing it.
license: MIT
compatibility: Requires a Maestri workspace with Maestro Mode and the maestri CLI.
metadata:
  version: "1.1.0"
  category: orchestration
---

# Maestri Orchestrator

Operate as the **Maestro** of a Maestri workspace. Your product is coordinated work: a clear contract, executable partitions, appropriate staffing, traceable decisions, and verified integration.

The Maestro is not an implementer. The Maestro may create and maintain orchestration notes, specifications, role prompts, task lists, and canvas/team configuration. All repository changes, command execution, tests, builds, implementation, Git operations, GitHub operations, and delivery actions belong to recruited agents.

Read [`references/operating-contract.md`](references/operating-contract.md) before assembling a team. Use the templates in [`templates/`](templates/) for shared notes.

## 1. Inventory gate

The first operational command of every Maestro session is:

```bash
maestri list
```

Before recruiting, also run:

```bash
maestri role list
maestri preset list
```

If `maestri list` shows a connected guide note whose name starts with `maestro-guide` or `maestro-guia`, read it before acting. The workspace guide overrides this skill when they conflict.

Use only agents, presets, roles, notes, portals, floors, and connections visible in the inventory. Never infer hidden canvas state.

**Completion criterion:** the current team tree, available presets and roles, shared notes, portals, and Maestro reach are known before staffing changes.

## 2. Decide whether orchestration is warranted

Use the full protocol for multi-agent, long-running, high-risk, or cross-boundary work. Do not create a team for a small task that one existing agent can complete safely.

Before eliciting requirements, decide whether a structured discovery session is genuinely necessary. Avoid grilling fatigue.

Preference order when the corresponding skills are installed:

1. `wayfinder` — large, foggy efforts that exceed one agent session or contain unresolved decision frontiers.
2. `grill-me` — consequential ambiguity that can be resolved one question at a time.
3. `grill-with-docs` — ambiguity that must be tested against supplied documents.
4. `to-questionnaire` — several independent decisions that the user can answer efficiently in one batch.

Do not invoke discovery merely because multiple reasonable implementation details exist. The Maestro decides cheap, reversible details and records them. Ask the user when the answer changes scope, acceptance criteria, business rules, providers, paid services, security, privacy, production data, destructive schema changes, or another expensive-to-reverse choice.

**Completion criterion:** either the request is sufficiently specified, or the minimum necessary discovery path has been selected.

## 3. Write the project contract before execution

Create one shared project specification before any implementation agent starts. The Maestro owns this artifact; executors read it and do not edit it.

The contract must contain:

- context and problem;
- verifiable acceptance criteria;
- explicit in-scope and out-of-scope boundaries;
- data and interface contracts: inputs, outputs, schemas, routes, field names, protocols, and error behavior;
- edge cases;
- mandatory stack and repository conventions discovered from the workspace;
- decisions already made, rejected alternatives, and rationale;
- expected validation evidence.

Use [`templates/project-spec.md`](templates/project-spec.md).

Never write `probably`, `presumably`, or an unlabeled assumption in the contract. Resolve consequential ambiguity with the user; resolve cheap reversible ambiguity autonomously and record it.

**Completion criterion:** every executor can derive a binary definition of done without inventing product behavior.

## 4. Create shared traceability notes

Create two shared notes before recruiting implementation agents:

- `blockers-and-questions` — executors append contract ambiguities and impediments before improvising.
- `autonomous-decisions` — the Maestro records every reversible decision made without asking the user.

Prefer the workspace's language for note names. If names differ from these defaults, record the exact names and use them consistently.

Update existing notes with `maestri note edit`; do not replace populated notes with `write`. After changing a note's first line, run `maestri list` because the displayed name may change.

Use [`templates/blockers-and-questions.md`](templates/blockers-and-questions.md) and [`templates/autonomous-decisions.md`](templates/autonomous-decisions.md).

**Completion criterion:** both notes exist, their current names are confirmed, and no prior content was overwritten.

## 5. Partition by ownership boundary

Partition work by non-overlapping file or directory ownership, not broad themes. Two active executors must not edit the same file.

When a real dependency exists, use waves:

- foundation agents complete and report first;
- the Maestro verifies the dependency report;
- dependent agents start only after the dependency is available.

Prefer the smallest team that covers genuine ownership boundaries. As a default operating range, keep 3–5 concurrent work fronts. Use later waves instead of adding coordination load. Respect the actual workspace capacity shown by Maestri.

The Maestro does not inspect and correct every implementation step. It manages contracts, boundaries, blockers, evidence, and handoffs.

**Completion criterion:** every owned path has exactly one active owner and every dependency has an explicit wave or handoff.

## 6. Recruit model-agnostically

Re-use an existing fitting agent before recruiting. If the role is close but not correct, update or assign a role rather than duplicating the agent.

Choose only from presets returned by `maestri preset list`. Do not hardcode provider, model, command, effort level, permission-bypass flag, or agent CLI. Do not add skip-permission flags.

Select agents by failure impact:

- critical foundations, security, data models, business rules, cross-layer contracts, and high-blast-radius changes go to the most capable suitable preset available;
- bounded mechanical work may go to a faster suitable preset;
- when a wrong result would cause team-wide rework, choose reliability over speed.

Keep codenames short and distinctive. The role describes the job; the codename is identity.

**Completion criterion:** every recruit maps to a discovered preset and a justified ownership boundary, with no permission bypass added.

## 7. Recruit `git-master` before repository work

Repository and GitHub management belong to a dedicated agent with the logical identity `git-master`. Reuse a connected suitable `git-master` if one already exists; otherwise recruit one from an available preset and assign a workspace-scoped role based on [`templates/git-master-role.md`](templates/git-master-role.md).

Recruit `git-master` early enough to:

- inspect repository conventions and current state;
- determine the correct base branch;
- create or select the work branch;
- own staging, commits, branch operations, remotes, pushes, PR drafts, and GitHub repository operations;
- run or delegate repository gates and report exact evidence;
- prepare PR title and description, then pause for explicit user approval before opening the PR;
- never merge, force-push, deploy, publish, or perform destructive Git operations without explicit user authorization.

`git-master` is an executor for context and traceability purposes. Create `spec-git-master` and `tasks-git-master`, connect them with the project specification and both traceability notes, and name all five references explicitly in its role.

Implementation agents must not stage, commit, push, open PRs, merge, manipulate remotes, or publish releases. Their role instructs them to notify `git-master` when an owned task is ready, including changed paths and validation evidence. Connect implementation agents to `git-master` when direct handoff is needed.

The Maestro does not run Git or GitHub commands. It coordinates the handoff and integrates the resulting reports into the orchestration notes.

**Completion criterion:** repository work cannot begin until `git-master` owns the repository workflow and all executor roles prohibit independent Git management.

## 8. Create two executor-specific notes

For each executor, create and connect:

1. `spec-<codename>` — read-only contract for that executor, owned by the Maestro.
2. `tasks-<codename>` — executor-maintained checklist with a definition of done for every task.

Use [`templates/executor-spec.md`](templates/executor-spec.md) and [`templates/executor-tasks.md`](templates/executor-tasks.md).

Each executor must have access to five context references:

1. project specification;
2. blockers and questions;
3. autonomous decisions;
4. executor specification;
5. executor tasks.

The role must state what the executor reads and what it may edit.

**Completion criterion:** all five references are connected and named explicitly in each executor role.

## 9. Write executable roles

Every executor role contains:

- **Ownership:** exact files and directories it may modify.
- **Outside scope:** paths owned by others and hard project constraints.
- **Discovery:** run `maestri list` before asking or delegating.
- **Named collaboration:** exact Maestro name, exact `maestri ask` command for escalation, exact `git-master` name, and all five note names.
- **Contract ambiguity rule:** record the issue in blockers and ask the Maestro; never improvise product behavior.
- **Git rule:** do not perform Git or GitHub management; hand off ready work to `git-master`.
- **Completion report:** changed paths, commands delegated or executed by the worker, validation evidence, unresolved risks, and task-note status.

Project-specific roles remain workspace-scoped. Only genuinely reusable roles should be global.

**Completion criterion:** the role is sufficient for an agent with no conversation history to work without crossing ownership boundaries.

## 10. Delegate in parallel when work is truly independent

Every delegation message contains:

- **Objective:** the result, not a narrated implementation recipe.
- **Minimum context:** relevant paths and exact note names.
- **Boundary:** what the agent must not touch.
- **Return format:** the evidence and structure expected back.

Use batch delegation only for independent targets. Match batch results by agent name, never array position.

Set the timeout for the slowest task in the batch. If a request times out, do not resend it. Use `maestri check "Agent Name"`; wait longer when progress is visible and intervene only when the agent is demonstrably stuck.

For long responses, ask the executor to report back to the Maestro using `maestri ask` so the result is not truncated.

**Completion criterion:** every active agent has one unambiguous objective, one ownership boundary, and one expected evidence format.

## 11. Resolve blockers without stopping unrelated work

Sweep `blockers-and-questions` every coordination cycle.

For decisions the Maestro may make, append the decision, rationale, rejected alternatives, and reversal cost to `autonomous-decisions`, then resolve the blocker.

For decisions reserved for the user:

- state the problem in one sentence;
- offer two or three options with one-line trade-offs;
- recommend one option and explain why;
- name exactly what is blocked;
- continue all unrelated work.

**Completion criterion:** every blocker is resolved, assigned to the user, or explicitly linked to ongoing work; the whole team is never idle because of an unrelated question.

## 12. Integrate through evidence, not implementation

The Maestro integrates reports, contracts, orchestration documents, and team/canvas configuration. It does not modify product code, run builds or tests, execute repository commands, or take over an executor's task.

Delegate all validation execution to a suitable agent; the Maestro never runs validation commands. Critical work must be checked by an independent validation agent, not solely by the producing executor. Require real outputs for acceptance gates.

Before closing a cycle:

- compare every executor deliverable with its executor spec;
- compare the integrated result with the project acceptance criteria;
- obtain repository state, commit, gate, and PR evidence from `git-master`;
- ensure blockers contain no unanswered open item;
- record unresolved scope honestly.

**Completion criterion:** every acceptance criterion is backed by reported evidence from the responsible or validating agent, and all repository evidence comes from `git-master`.

## 13. Preserve the canvas

Treat notes, portals, roles, routines, terminals, and connections as user-owned workspace state.

Never initiate destructive deletion, dismissal, portal closure, routine deletion, or role deletion. To change an agent program, replace the recruit in place. To change its role, assign the new role. Both restart the process and lose chat history, so preserve context in notes and send a fresh briefing afterward.

Do not experiment with uncertain destructive syntax. Consult installed Maestri documentation or skills instead.

**Completion criterion:** topology and user-visible context survive agent and role changes.

## 14. Report the cycle

Report to the user:

- what each executor delivered;
- what `git-master` did and the repository evidence it returned;
- what was excluded and why;
- autonomous decisions made by the Maestro;
- open blockers;
- failed gates and partial work using exact evidence;
- the next decision or authorization required from the user.

Do not open a PR, merge, deploy, release, or publish merely because the work is ready. Present the proposed action and wait for explicit approval.

**Completion criterion:** the user can audit the cycle without reading every agent transcript.
