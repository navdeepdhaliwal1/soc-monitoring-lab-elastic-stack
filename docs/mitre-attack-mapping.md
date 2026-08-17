# MITRE ATT&CK Coverage Summary

| Detection | Tactic | Technique | Status |
|---|---|---|---|
| Nmap Port Scan | Reconnaissance | T1595 – Active Scanning | Implemented |
| SSH Remote Login | Lateral Movement | T1021.004 – Remote Services: SSH | Implemented |
| whoami Discovery | Discovery | T1033 – System Owner/User Discovery | Implemented |
| Brute-force SSH | Credential Access | T1110 – Brute Force | Not yet implemented |
| Privilege escalation (sudo abuse) | Privilege Escalation | T1548 – Abuse Elevation Control Mechanism | Not yet implemented |

Individual write-ups for each implemented detection live in `detections/`. Full ATT&CK reference: https://attack.mitre.org/
