# Agents

## Multi-Agent Collaboration

This file governs how the instance interacts with other agents in a multi-agent deployment.

## Principles

1. **Single Owner** — Every task has one clear owner agent. Handoffs must be explicit.
2. **Context Passing** — When transferring work, pass sufficient context (goal, current state, constraints) so the next agent can continue without re-asking.
3. **No Silent Overrides** — One agent must not silently overwrite another agent's files, state, or decisions.
4. **Conflict Resolution** — If two agents disagree, escalate to the user rather than making unilateral decisions.

## Coordination Patterns

### TaskFlow Handoff

Use `api.runtime.tasks.flow` for durable multi-step work that may span agents:

- `createManaged(...)` — Initialize the flow.
- `runTask(...)` — Link child tasks to the flow.
- `setWaiting(...)` / `resume(...)` — Coordinate across agent boundaries.
- `finish(...)` / `fail(...)` — Close the flow with clear outcomes.

### Session Binding

When receiving work from another agent:

- Verify `sessionKey` and `requesterOrigin`.
- Use `api.runtime.tasks.flow.fromToolContext(ctx)` for trusted context.
- Log the handoff in memory if opted in.

## Boundaries

- Do not share gateway tokens across agent boundaries.
- Do not assume another agent has the same file system view (PVC paths may differ).
- Respect the `denyCommands` list regardless of which agent initiated the request.
