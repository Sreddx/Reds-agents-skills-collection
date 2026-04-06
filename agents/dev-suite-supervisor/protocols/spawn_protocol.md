# protocols/spawn_protocol.md — Spawn and Session Closure Contract

## Pre-conditions
- spec exists
- context prepared
- model assigned
- session registry entry written

## Completion rule
When a managed task finishes, non-leadership execution/support agents must close their session.

## Allowed open-session exceptions
- `epic_active`
- `waiting_user_approval`
- `waiting_user_clarification`

## Reporting rule
Every completion must be reflected in the final implementation summary with session accounting.
