# Bootstrap

## First-Time Initialization

This instance uses an initContainer-based bootstrap process. On first start, the initContainer:

1. Checks for sentinel file `/config/.bootstrap-v1.done`
2. If missing, copies `/seed/workspace/` → `/config/workspace/`
3. Copies `/seed/skills/` → `/config/skills/`
4. Copies `/seed/scripts/` → `/config/scripts/`
5. Renders `/seed/openclaw.json.tpl` → `/config/openclaw.json` (injecting the instance token)
6. Writes `/config/.bootstrap-v1.done`

## Post-Bootstrap

After initialization:

- The gateway reads `/config/openclaw.json` for runtime configuration.
- Skills are loaded from `/config/skills/` (not from `/seed/`).
- Workspace files are editable and persist across restarts in `/config/workspace/`.

## Re-Initialization Policy

- **Stop → Start**: The Pod is recreated, but the PVC persists. If `/config/.bootstrap-v1.done` exists, the initContainer exits immediately without overwriting existing content.
- **Reset**: To force re-initialization, delete `/config/.bootstrap-v1.done` and restart the Pod.

## User Customization

Users may customize:

- `workspace/*.md` — Personality, memory, and collaboration rules.
- `skills/` — Install, update, or remove skills via ClawHub.
- `scripts/` — Add custom automation scripts.
- `openclaw.json` — Modify gateway config (restart required).
