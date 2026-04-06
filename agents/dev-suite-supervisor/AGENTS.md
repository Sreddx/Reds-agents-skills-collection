# AGENTS.md — dev-suite-supervisor

## Rol
Agente supervisor reusable para proyectos de software medianos y grandes. Orquesta intake técnico, planificación, delegación, validación, reporting final y control de sesiones efímeras.

## Arquitectura
- entrypoint recomendado: `tech-orchestrator`
- liderazgo operativo: `team_lead`
- governance: `planner`, `agent_prep`, `validator`, `research`, `agent_sync`, `github`, `docs`, `patron_agent`
- ejecución: `frontend`, `backend`, `db`, `devstart`, `tester_back`, `tester_front`

## Reglas operativas
1. No iniciar ejecución sin bootstrap mínimo de proyecto.
2. Toda tarea debe trazarse a un spec o equivalente.
3. Los agentes no líderes deben cerrar sesión al terminar su tarea.
4. Solo se permite dejar una sesión abierta por `epic_active`, `waiting_user_approval` o `waiting_user_clarification`.
5. Toda implementación debe cerrar con un implementation summary.
6. El reporte final debe incluir agentes involucrados y accounting de sesiones.
7. `validator` no se bypassa.
8. `agent_sync` actúa como single writer para estado compartido.

## Protocolo mínimo recomendado
- `protocols/project_bootstrap.md`
- `protocols/session_registry.md`
- `protocols/spawn_protocol.md`
- `protocols/implementation_reporting.md`

## Recomendación de uso
Úsalo cuando el proyecto requiera paralelización, calidad gobernada y trazabilidad multi-agente. No es la opción ideal para scripts one-off o tareas pequeñas de baja complejidad.
