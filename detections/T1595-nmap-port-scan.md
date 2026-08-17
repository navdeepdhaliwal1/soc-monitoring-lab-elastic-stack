# Detection: Network Reconnaissance: Nmap Port Scan

| Field | Value |
|---|---|
| Tactic | Reconnaissance |
| Technique | T1595 – Active Scanning |
| Log Source | Packetbeat (`packetbeat-*`) |
| Severity | Medium |

## Description
Detects a source host generating connection attempts across multiple distinct ports against a monitored host in a short time window — consistent with port scanning tools like Nmap.

## Attack Simulation
From the Kali attacker machine (192.168.56.103) against the victim (192.168.56.104):
```bash
ping -c 4 192.168.56.104
nmap -sT -p 22,80,443 192.168.56.104
```
Result: port 22/tcp (SSH) open; ports 80 and 443 closed.

## Detection Query
```kql
agent.type : "packetbeat"
```
```kql
source.ip : "192.168.56.104" or destination.ip : "192.168.56.104"
```
Reusable copies of these queries are in `queries/packetbeat-network-recon.kql`.

## Why This Works
Isolating traffic to/from the monitored host's IP surfaces the scan's signature: multiple ports probed from a single source within seconds, followed immediately by an established session  a pattern generic traffic monitoring alone would not flag without this filtering.

## Evidence
`screenshots/03-nmap-scan-ssh-login.png`, `screenshots/04-packetbeat-network-overview.png`, `screenshots/05-packetbeat-filtered-host-traffic.png`

## Recommended Response
See `playbooks/alert-triage-playbook.md`. Containment: restrict inbound port 22 to a known management range/VPN, enable SSH rate-limiting, alert on hosts touching >N ports from one source within a short window.
