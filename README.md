# Reds-agents-skills-collection

Colección de **agentes** y **skills** para OpenClaw.

Estructura:

- `skills/<skill-name>/SKILL.md`
- `agents/<agent-name>/` (SOUL.md, USER.md, AGENTS.md, etc.)
- `examples/` para templates de runtime config
- `docs/` para instalación y uso

## Nuevos artefactos de Dev Suite

- `agents/dev-suite-supervisor/`
- `agents/tech-orchestrator-dev/`
- `skills/gsap-react-motion/`
- `examples/openclaw.dev-suite.example.json`
- `docs/dev-suite-installation.md`

## Quick start

1. Revisa `docs/dev-suite-installation.md`
2. Usa `examples/openclaw.dev-suite.example.json` como base para tu runtime
3. Ajusta paths, skills y `subagents.allowAgents`
4. Bootstrappea el proyecto objetivo antes de ejecutar el suite

Smoke checks (desde el workspace donde vive Skillforge):

```bash
node skillforge/validators/validate-repo.mjs /ruta/a/Reds-agents-skills-collection
```
