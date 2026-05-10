
---

# 4. `successful-login-after-bruteforce.md`

## Purpose

Documents:
- successful authentication after repeated failures

---

## Include

```markdown id="md4"
# Successful Login After Brute Force

## Description
Detects successful authentication activity following repeated failed login attempts.

## MITRE ATT&CK
- T1110 — Brute Force
- T1078 — Valid Accounts

## Data Source
- SecurityEvent

## Event IDs
- 4624
- 4625

## KQL Query

```kql
let FailedLogins =
SecurityEvent
| where EventID == 4625
| summarize FailCount=count() by IpAddress, Account
| where FailCount >= 5;

SecurityEvent
| where EventID == 4624
| join kind=inner FailedLogins on Account
