# Tools

## Overview

This instance has access to the OpenClaw tool ecosystem. Tools are exposed through skills, channels, and the OpenClaw CLI.

## Native CLI Tools

| Tool | Purpose | Safety Notes |
|------|---------|--------------|
| `openclaw gateway` | Start/stop the gateway server | Binds to `0.0.0.0:18789` in this config |
| `openclaw security audit` | Audit host and OpenClaw posture | Read-only by default; `--fix` requires confirmation |
| `openclaw update status` | Check for OpenClaw updates | Safe |
| `openclaw cron` | Schedule periodic tasks | Requires explicit user opt-in |
| `openclaw doctor` | Diagnose configuration issues | Safe |

## Denied Commands

The following commands are blocked by default in this instance's gateway configuration:

- `camera.snap`
- `camera.clip`
- `screen.record`
- `contacts.add`
- `calendar.add`
- `reminders.add`
- `sms.send`

## Skill-Based Tools

Skills extend capabilities dynamically. Built-in skills at first boot:

- `skill-creator` — Create, edit, and package new skills.
- `healthcheck` — Audit and harden host security posture.

Install more via ClawHub: `clawhub search <term>` and `clawhub install <skill>`.

## Tool Use Guidelines

1. **Prefer Native** — Use OpenClaw-native tools before calling external binaries.
2. **Validate Existence** — Check that required binaries exist before invoking them.
3. **Escape Safely** — Sanitize user input before passing to shell commands.
4. **No Secret Leaks** — Never include tokens or credentials in command arguments that might be logged.
