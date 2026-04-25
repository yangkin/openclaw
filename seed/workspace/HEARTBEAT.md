# Heartbeat

## Runtime Monitoring

This file defines the instance's self-monitoring and health-checking behavior.

## Built-In Health Endpoints

The gateway exposes the following endpoints for liveness and readiness probes:

| Endpoint | Purpose | Expected Response |
|----------|---------|-------------------|
| `GET /healthz` | Liveness probe | HTTP 200 when gateway process is running |
| `GET /readyz` | Readiness probe | HTTP 200 when gateway is ready to accept traffic |
| `GET /health` | Alias for `/healthz` | Same as above |
| `GET /ready` | Alias for `/readyz` | Same as above |

## Self-Checks

### Periodic Audit

Offer the user periodic security and update audits:

```bash
openclaw security audit        # Fast, non-probing
openclaw security audit --deep # Comprehensive
openclaw update status         # Check for updates
```

### Cron Scheduling

If the user opts in, schedule recurring checks via:

```bash
openclaw cron add --name healthcheck:security-audit --schedule "0 9 * * *" --command "openclaw security audit"
openclaw cron add --name healthcheck:update-status --schedule "0 10 * * 1" --command "openclaw update status"
```

Always use stable job names so updates are deterministic.

## Failure Modes

| Scenario | Behavior |
|----------|----------|
| Gateway crash | Container exits; Pod does not auto-restart (`RestartPolicy: Never`) |
| InitContainer failure | Pod fails; user must inspect logs and fix root cause |
| PVC unavailable | Pod stays in `Pending`; check storage class and volume claims |
| Token mismatch | Control UI and API requests fail with 401; verify `/config/openclaw.json` |

## Observability

- Logs: Standard output (stdout/stderr) captured by the container runtime.
- Metrics: Exposed via gateway HTTP endpoints where configured.
- Session Logs: Stored locally; use `session-logs` skill to search historical conversations.
