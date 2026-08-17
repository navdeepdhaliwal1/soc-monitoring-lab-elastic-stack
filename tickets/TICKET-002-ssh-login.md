# TICKET-002: Successful SSH Login Following Port Scan

| Field | Value |
|---|---|
| Status | Closed |
| Severity | Medium |
| Verdict | True Positive |
| Detection | `detections/T1021.004-ssh-remote-login.md` |
| Source | 192.168.56.103 (Kali) |
| Target | 192.168.56.104 (Ubuntu) |
| User | vboxuser |

## Summary
19 `system.auth` events captured over a 15-minute Discover window, timestamp-correlated with an SSH session originating from the host that performed the port scan in TICKET-001.

## Investigation
- Queried `filebeat-*` for `event.dataset : "system.auth"`
- Confirmed event timing aligns with the network-layer session seen in Packetbeat
- Credentials used were valid (`vboxuser`)  not a brute-force or credential-stuffing scenario

## Verdict & Reasoning
True Positive. Legitimate credentials were used, but the access pattern (scan then login from the same source within minutes) still warrants review  this is exactly the kind of low-and-slow sequence a single-event alert would miss.

## Actions Taken
- Recommended: SSH key-based auth, disable root login, alert on logins from IPs seen scanning minutes prior
