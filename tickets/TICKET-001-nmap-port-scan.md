# TICKET-001: Network Reconnaissance: Port Scan Detected

| Field | Value |
|---|---|
| Status | Closed |
| Severity | Medium |
| Verdict | True Positive |
| Detection | `detections/T1595-nmap-port-scan.md` |
| Source | 192.168.56.103 (Kali) |
| Target | 192.168.56.104 (Ubuntu) |
| Analyst | L1 SOC (lab exercise) |

## Summary
Packetbeat captured a burst of connection attempts from 192.168.56.103 against 192.168.56.104 across ports 22, 80, and 443 within seconds, immediately followed by an established session on port 22.

## Investigation
- Filtered `packetbeat-*` to traffic involving the victim host IP (56 documents)
- Confirmed multi-port probing pattern typical of Nmap TCP connect scans
- Cross-referenced against `system.auth` (see TICKET-002)  scan was immediately followed by a successful SSH login from the same source

## Verdict & Reasoning
True Positive. Deliberate reconnaissance immediately preceding access  even though the follow-on login used valid credentials, the scan-then-login sequence is a pattern worth flagging in a real environment.

## Actions Taken
- Documented as a chained event with TICKET-002 and TICKET-003
- Recommended: restrict inbound port 22 to a known management range, alert on multi-port probing from a single source
