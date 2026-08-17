# False-Positive Analysis Playbook

## When to Suspect a False Positive
- The triggering activity has a plausible, common, non-malicious explanation
- No corroborating evidence in the other two log sources
- The "suspicious" indicator (e.g. an external domain, a discovery command) is well-known and benign
- The pattern doesn't match the technique's real signature (e.g. a single connection, not the periodicity expected of beaconing)

## Process
1. Identify exactly which field/value caused the rule to fire
2. Research the flagged indicator (domain, IP, process, hash) against known-good references
3. Compare the observed pattern against the technique's expected signature single event vs. repeated/periodic, expected port/protocol vs. anomalous
4. Document the reasoning, not just the conclusion "benign" alone isn't useful to the next analyst

## Worked Example: Outbound Web Request
**Alert:** Outbound request to `example.com` (`curl` + `nslookup`) from the monitored host.

**Why it looked suspicious:** External connection to a domain not on any internal allowlist resembles early-stage C2 or data staging.

**Why it's benign:**
- `example.com` is IANA's public reserved documentation domain, not attacker infrastructure
- Single request-response pair, no periodicity (real beaconing shows a repeating interval)
- Standard HTTP GET and DNS lookup no unusual port, protocol, or payload size

**Tuning recommendation:** Don't alert on single outbound requests to arbitrary domains. Tune for repeated connections at regular intervals (beacon-interval detection) or connections to low-reputation/recently-registered domains instead.

Full evidence: `screenshots/08-outbound-web-request-fp.png`, ticket `tickets/TICKET-004-fp-outbound-web-request.md`.
