# RDP Brute Force Incident

## Incident Summary

A brute-force attack was detected against the Windows honeypot VM after multiple failed RDP authentication attempts were observed from a single external IP address.

## Severity
Medium

## MITRE ATT&CK
- T1110 — Brute Force

## Detection Rule
Brute Force Detection Rule

## Event IDs
- 4625

## Investigation Timeline

| Time | Event |
|---|---|
| 10:15 | Hydra attack initiated |
| 10:18 | Multiple Event ID 4625 logs generated |
| 10:20 | Sentinel analytics rule triggered |
| 10:21 | Incident generated |

## Investigation Actions

- Reviewed failed authentication logs
- Identified source IP address
- Confirmed repeated login failures
- Correlated authentication activity

## Evidence

- SecurityEvent logs
- Failed login telemetry
- Sentinel incident screenshots

## Outcome

Incident classified as:
- True Positive

## Lessons Learned

Repeated failed RDP authentication attempts can indicate active password spraying or brute-force activity.
