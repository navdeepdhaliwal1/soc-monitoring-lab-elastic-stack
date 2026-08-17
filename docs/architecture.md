# Lab Architecture

The environment consists of two virtual machines.

| System | Role | Main Tools |
|---|---|---|
| Kali Linux (192.168.56.103) | Attacker / Security Testing Machine | Nmap, SSH, Metasploit |
| Ubuntu 24 LTS (192.168.56.104) | Victim / Monitored Endpoint / SIEM Server | Elasticsearch, Kibana, Filebeat, Auditbeat, Packetbeat |

**Elastic Stack version:** 8.19.19
**Network setup:** *(add your hypervisor and network mode — e.g. VirtualBox, host-only adapter)*

## Architecture Flow

```text
                    Kali Linux
                 Attacker Machine
                       |
            Controlled Test Activity
                       v
                  Ubuntu 24 LTS
                       |
          +------------+------------+
          |            |            |
      Filebeat     Auditbeat    Packetbeat
          |            |            |
       Auth Logs     Process      Network
       SSH/Sudo      Activity      Traffic
          |            |            |
          +------------+------------+
                       v
                 Elasticsearch
                       v
                    Kibana
                       v
              SIEM Detection Rules
                       v
                    Alerts
                       v
                 SOC L1 Triage
```

Kali generates controlled test activity against the Ubuntu host. Filebeat, Auditbeat, and Packetbeat each ship a distinct telemetry stream into Elasticsearch, where Kibana evaluates queries/rules against the ingested data and routes findings for L1 triage.

## Environment Verification

See `screenshots/01-elasticsearch-setup-verification.png` and `screenshots/02-beats-installation.png` for cluster health and Beats version confirmation.
