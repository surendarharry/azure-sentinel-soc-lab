# Successful Login After Brute Force

## Incident Summary

A successful login event was detected after repeated failed authentication attempts, indicating potential credential compromise.

## Severity
High

## MITRE ATT&CK
- T1110 — Brute Force
- T1078 — Valid Accounts

## Event IDs
- 4624
- 4625

## Investigation Actions

- Correlated failed and successful login events
- Identified source IP
- Reviewed authentication sequence
- Confirmed successful access

## Evidence

- Login telemetry
- SecurityEvent logs
- Sentinel investigation data

## Outcome

Incident classified as:
- True Positive

## Lessons Learned

Successful authentication after repeated failures may indicate password compromise.
