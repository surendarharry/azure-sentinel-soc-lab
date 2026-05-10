# Brute Force Detection Rule

## Description
Detects repeated failed RDP authentication attempts against the Windows honeypot VM.

## MITRE ATT&CK
- T1110 — Brute Force

## Data Source
- SecurityEvent

## Event IDs
- 4625

## KQL Query

```kql
SecurityEvent
| where EventID == 4625
| where TimeGenerated > ago(1h)
| summarize FailCount = count() by IpAddress, Account, Computer
| where FailCount >= 5
