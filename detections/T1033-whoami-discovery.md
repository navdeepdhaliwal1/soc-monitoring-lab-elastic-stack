# Detection: Post-Login Discovery: whoami Execution

| Field | Value |
|---|---|
| Tactic | Discovery |
| Technique | T1033  System Owner/User Discovery |
| Log Source | Auditbeat (`auditbeat-*`) |
| Severity | Low (higher when chained with prior events) |

## Description
Flags execution of common user/context enumeration commands, which are typically among the first actions taken after gaining access.

## Attack Simulation
Run inside the SSH session established in the SSH login scenario:
```bash
whoami
```

## Detection Query
```kql
process.name : "whoami"
```
Reusable copy in `queries/auditbeat-process-whoami.kql`.

## Why This Works
Auditbeat captures every `execve` syscall; filtering on `process.name` isolates specific binary executions regardless of which shell or session spawned them.

## Evidence
`screenshots/07-whoami-process-detection.png`  3 `execve` events, TTY, and exit code 0.

## Recommended Response
Low severity in isolation. Correlate: alert when discovery commands (`whoami`, `id`, `uname -a`, `hostname`) run within minutes of a new SSH session from a previously-unseen source.
