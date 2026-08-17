# SOC L1 Alert Triage Playbook

General procedure followed for every alert investigated in this lab, regardless of which detection fired.

## 1. Initial Review
- What fired: rule/query name, data source, severity
- When: timestamp, timezone
- Who/what: source and destination IP, host, user, process

## 2. Investigation
- Pivot into Kibana Discover using the alerting query as a starting filter
- Widen or narrow the time range to see what happened immediately before/after
- Cross-reference against the *other* two log sources (auth, process, network) for the same host/timeframe — single-source alerts are weaker evidence than a corroborated chain
- Check whether the source IP/user has any prior history in the environment

## 3. Evidence Collection
- Screenshot the Discover view showing the relevant events
- Note exact document counts, timestamps, and any fields that support or undermine the alert

## 4. Verdict
- **True Positive**: malicious or policy-violating activity confirmed
- **False Positive**: benign activity that matched the detection pattern (see `playbooks/false-positive-analysis-playbook.md`)
- **Benign Positive**: activity is real but expected/authorized (e.g. scheduled scan, known admin)

## 5. Documentation
- Log the investigation as a ticket in `tickets/` using the standard template
- Update the relevant file in `detections/` if the investigation reveals the query needs tuning

## 6. Containment (if True Positive)
- Recommend the narrowest effective action first (block IP, disable account, isolate host)
- Note any follow-up hardening (e.g. key-based auth, rate limiting) separately from immediate containment
