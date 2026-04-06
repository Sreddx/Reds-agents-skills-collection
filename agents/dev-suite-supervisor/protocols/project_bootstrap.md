# protocols/project_bootstrap.md — Reusable Project Bootstrap Contract

## Purpose

Define the minimum bootstrap required before the Dev Suite executes against a new or existing project. This keeps the suite portable, predictable, and scalable across projects.

## Required bootstrap

These files or directories MUST exist before full Dev Suite execution:
- `project_config.yaml`
- `CLAUDE.md`

## Recommended bootstrap for non-trivial projects

Add these when the project has domain complexity, parallel work, or multi-agent execution:
- `AGENTS.md` for project-specific context and rules
- `specs/`
- `patterns/`
- `reports/`
- `session_registry.json` when ephemeral implementer visibility is required
