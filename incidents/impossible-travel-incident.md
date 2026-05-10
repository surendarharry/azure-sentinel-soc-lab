# Impossible Travel Authentication Incident

## Incident Summary

Suspicious authentication activity was detected after successful sign-ins were observed from geographically distant locations within a short period of time.

## Severity
High

## MITRE ATT&CK
- T1078 — Valid Accounts

## Detection Rule
Impossible Travel Detection

## Data Source
- SigninLogs

## Investigation Actions

- Reviewed Entra ID sign-in logs
- Compared geographic locations
- Investigated IP addresses
- Validated authentication success events

## Evidence

- Sign-in telemetry
- Geographic login locations
- Sentinel incident screenshots

## Outcome

Incident classified as:
- True Positive

## Lessons Learned

Impossible travel events may indicate compromised credentials or unauthorized cloud account access.
