# Dev Suite installation and usage

## What this package gives you

This repo now contains three portable building blocks:
- `agents/dev-suite-supervisor/`
- `agents/tech-orchestrator-dev/`
- `skills/gsap-react-motion/`

These are blueprints for a governed multi-agent software-delivery setup, not a one-click full runtime.

## Recommended use case

Use this setup for:
- medium or large software projects
- parallel frontend/backend/db work
- quality-gated delivery
- traceable multi-agent execution

Avoid it for:
- one-off scripts
- tiny repos
- low-complexity maintenance tasks

## Minimum runtime requirements

Before using the Dev Suite on a project, bootstrap the target project with:
- `project_config.yaml`
- `CLAUDE.md`

Recommended for non-trivial projects:
- `AGENTS.md`
- `specs/`
- `patterns/`
- `reports/`
- `session_registry.json`

See:
- `agents/dev-suite-supervisor/protocols/project_bootstrap.md`

## Runtime config wiring

Use the example file:
- `examples/openclaw.dev-suite.example.json`

Merge it into your runtime config and replace:
- `<workspace-root>`
- model aliases if desired
- any workspace names that differ in your installation

## Required routing relationships

The Dev Suite depends on explicit delegate permissions.

Required `subagents.allowAgents`:
- `tech-orchestrator` -> `team_lead`, `research`, `patron_agent`, `agent_sync`
- `team_lead` -> `planner`, `agent_prep`, `frontend`, `backend`, `db`, `devstart`, `tester_back`, `tester_front`, `validator`, `github`, `docs`

Without these, the orchestration collapses and the lead/orchestrator tends to do the work itself.

## Frontend motion setup

`frontend` should load:
- `ui-ux-pro-max`
- `gsap-react-motion`

Use GSAP when:
- timeline orchestration matters
- ScrollTrigger is product-relevant
- SVG morphing or advanced interaction motion is required

Do not use GSAP for trivial motion that CSS can solve cleanly.

## Operating model

### Entrypoint
- `tech-orchestrator`
- does intake, bootstrap, routing, final summary
- does **not** implement code directly for code tasks

### Delivery lead
- `team_lead`
- plans, delegates, enforces quality gates, consolidates execution

### Support and execution agents
- implementers, testers, validator, prep, sync
- close sessions when their task finishes
- may remain open only for:
  - `epic_active`
  - `waiting_user_approval`
  - `waiting_user_clarification`

## Reporting contract

Every implementation should end with:
- what changed
- artefacts delivered
- agents involved
- sessions closed
- sessions still open by allowed exception
- risks or pending follow-ups

See:
- `agents/dev-suite-supervisor/protocols/implementation_reporting.md`

## Suggested rollout path

1. Create or map the required workspaces
2. Merge `examples/openclaw.dev-suite.example.json` into your runtime config
3. Ensure the skill directories are loadable
4. Bootstrap one real project with `project_config.yaml` and `CLAUDE.md`
5. Start with `tech-orchestrator` as entrypoint
6. Validate routing and session closure behavior on one small epic
7. Expand to broader project usage

## Common failure modes

- Missing `subagents.allowAgents`
- Missing skill load directory
- No project bootstrap files
- Frontend using GSAP without reduced-motion fallback
- Open sessions left without allowed waiting reason

## Practical recommendation

Treat this repo as a portable blueprint layer.
Keep project-specific context in each project workspace, not in these reusable agent/skill artifacts.
