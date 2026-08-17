# TICKET-004: Outbound Web Request: False Positive

| Field | Value |
|---|---|
| Status | Closed |
| Severity | Low (as reported) |
| Verdict | False Positive |
| Host | 192.168.56.104 (Ubuntu) |

## Summary
Monitored host generated an outbound `curl http://example.com` request and an `nslookup example.com` DNS resolution  activity that could superficially resemble a C2 check-in.

## Investigation
- Reviewed Packetbeat flow data for the connection
- Confirmed destination (`example.com`) is IANA's public reserved documentation domain
- Traffic was a single request-response pair  no periodicity, unusual port, or abnormal payload size

## Verdict & Reasoning
False Positive. Single benign outbound request to a well-known, non-malicious domain. Does not match the expected signature of beaconing (repeated, periodic connections).

## Actions Taken
- Recommended tuning: don't alert on single outbound requests to arbitrary domains; require repeated periodic connections or low-reputation domain reputation before flagging
- Full analysis: `playbooks/false-positive-analysis-playbook.md`
