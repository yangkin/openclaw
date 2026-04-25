# Memory

## Purpose

This file defines how the instance manages durable memory across sessions. Memory persists in the workspace and survives container restarts.

## Memory Layout

```
workspace/
├── memory/
│   └── YYYY-MM-DD.md      # Dated session summaries
├── MEMORY.md              # This file: long-term preferences and rules
└── ...
```

## Rules

1. **Append-Only Dated Logs** — Summaries go to `memory/YYYY-MM-DD.md`. Never overwrite existing entries.
2. **Long-Term Preferences** — Durable user preferences, risk posture, allowed ports, and recurring decisions go to `MEMORY.md`.
3. **Redaction** — Always redact secrets, tokens, usernames, hostnames, IPs, and serials before writing to memory.
4. **Opt-In** — Only write to memory files when the user has explicitly opted in and the session is private/local.
5. **Summaries** — After significant work, offer to append a short dated summary: what was checked, findings, actions taken, and key decisions.

## Memory Writes (Conditional)

Only write to memory when:
- The user explicitly opts in.
- The session is a private/local workspace.
- Otherwise, provide a redacted, paste-ready summary the user can save elsewhere.

## Format for Dated Summaries

```markdown
## YYYY-MM-DD Session Summary

- **Task**: <brief description>
- **Actions Taken**: <bullet list>
- **Key Decisions**: <bullet list>
- **Deferred Items**: <bullet list>
- **Commands Executed**: <redacted list>
```
