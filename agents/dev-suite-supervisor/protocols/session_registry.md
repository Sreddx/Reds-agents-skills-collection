# protocols/session_registry.md — Mission Control Visibility Contract

## Session entry schema

```json
{
  "agent_id": "string",
  "task_id": "string",
  "status": "spawned | running | waiting | completed | failed",
  "spawned_at": "ISO8601 timestamp",
  "updated_at": "ISO8601 timestamp",
  "model": "string",
  "artefact": "string | null",
  "error": "string | null",
  "waiting_reason": "waiting_user_approval | waiting_user_clarification | epic_active | null",
  "session_state": "open | closed"
}
```

## Rule
Non-leadership execution/support agents close their session when the managed task finishes. Open sessions are valid only under the allowed waiting reasons above.
