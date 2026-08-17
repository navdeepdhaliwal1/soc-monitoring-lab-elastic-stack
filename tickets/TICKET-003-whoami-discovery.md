# TICKET-003: Discovery Command Executed Post-Login

| Field | Value |
|---|---|
| Status | Closed |
| Severity | Low |
| Verdict | True Positive |
| Detection | `detections/T1033-whoami-discovery.md` |
| Host | 192.168.56.104 (Ubuntu) |

## Summary
Auditbeat captured 3 `execve` events for `whoami`, run inside the SSH session documented in TICKET-002.

## Investigation
- Queried `auditbeat-*` for `process.name : "whoami"`
- Confirmed TTY and timing place the command inside the active SSH session, not a separate local login
- Exit code 0 confirms successful execution

## Verdict & Reasoning
True Positive, low severity in isolation. Significant primarily as the third link in the scan → login → enumerate chain (TICKET-001 → TICKET-002 → TICKET-003).

## Actions Taken
- Recommended: correlation rule chaining discovery commands to recent new-source SSH sessions
