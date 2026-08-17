# SIEM Use Case & Alert Triage: SOC Monitoring Lab

A small-scale SOC monitoring environment built on the Elastic Stack, organized as a working detection-engineering content library: individual detection rules, analyst playbooks, investigation tickets, reusable queries, and sanitised configs — not just a single writeup.

## Objective

Simulate common security events from an attacker machine against a monitored Linux host, ingest the resulting telemetry into a SIEM, write detection queries, investigate the resulting activity, and document the analyst triage process — demonstrating practical SOC Level 1 skills.

**Skills demonstrated:** log collection & SIEM onboarding, authentication monitoring, endpoint process monitoring, network traffic monitoring, detection engineering (KQL), MITRE ATT&CK mapping, alert triage, false-positive analysis, incident documentation, containment recommendations.

##  Environment

| System | Role | Main Tools |
|---|---|---|
| Kali Linux (192.168.56.103) | Attacker / Security Testing | Nmap, SSH, Metasploit |
| Ubuntu 24 LTS (192.168.56.104) | Victim / Monitored Endpoint / SIEM Server | Elasticsearch, Kibana, Filebeat, Auditbeat, Packetbeat |

Elastic Stack version 8.19.19. Full architecture and flow diagram: [`docs/architecture.md`](./docs/architecture.md).

##  Scenarios Summary

| # | Scenario | Technique | Verdict | Detection | Ticket |
|---|---|---|---|---|---|
| 1 | Nmap Port Scan | T1595  Active Scanning | True Positive | [`detections/T1595-nmap-port-scan.md`](./detections/T1595-nmap-port-scan.md) | [`TICKET-001`](./tickets/TICKET-001-nmap-port-scan.md) |
| 2 | SSH Remote Login | T1021.004  Remote Services: SSH | True Positive | [`detections/T1021.004-ssh-remote-login.md`](./detections/T1021.004-ssh-remote-login.md) | [`TICKET-002`](./tickets/TICKET-002-ssh-login.md) |
| 3 | whoami Discovery | T1033  System Owner/User Discovery | True Positive | [`detections/T1033-whoami-discovery.md`](./detections/T1033-whoami-discovery.md) | [`TICKET-003`](./tickets/TICKET-003-whoami-discovery.md) |
| 4 | Outbound Web Request |  | False Positive | [`playbooks/false-positive-analysis-playbook.md`](./playbooks/false-positive-analysis-playbook.md) | [`TICKET-004`](./tickets/TICKET-004-fp-outbound-web-request.md) |

These four events form one narrative: reconnaissance → authenticated access → post-access enumeration, plus a separate benign case used to demonstrate false-positive judgment. Full MITRE coverage table: [`docs/mitre-attack-mapping.md`](./docs/mitre-attack-mapping.md).

## Repository Structure

```
SIEM-Use-Case-Alert-Triage/
├── README.md
├── docs/              → architecture, MITRE ATT&CK coverage summary
├── detections/        → one file per detection: query, logic, MITRE mapping, evidence
├── playbooks/         → general triage procedure + false-positive analysis method
├── tickets/           → per-investigation records (like real SOC case tickets)
├── screenshots/       → visual evidence from Kibana and the lab
├── queries/           → reusable, standalone .kql files
└── config-examples/   → sanitised Filebeat/Auditbeat/Packetbeat configs
```

##  Reproducing This Lab
1. Two VMs: an attacker (Kali) and a victim/SIEM host (Ubuntu) on an isolated/host-only network
2. Install Elasticsearch + Kibana on the Ubuntu host, verify with the cluster health check in `screenshots/01-elasticsearch-setup-verification.png`
3. Install Filebeat, Auditbeat, and Packetbeat — starter configs in `config-examples/`
4. Run the attack simulations described in each `detections/*.md` file
5. Investigate using the queries in `queries/`, following the process in `playbooks/alert-triage-playbook.md`

##  References
- [Elastic Security Documentation](https://www.elastic.co/guide/en/security/current/index.html)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)
