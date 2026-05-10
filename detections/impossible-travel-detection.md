
---

# 3. `impossible-travel-detection.md`

## Purpose

Documents:
- suspicious Entra sign-in behavior

---

## Include

```markdown id="md3"
# Impossible Travel Detection Rule

## Description
Detects successful sign-ins from multiple geographic locations within a short period of time.

## MITRE ATT&CK
- T1078 — Valid Accounts

## Data Source
- SigninLogs

## KQL Query

```kql
SigninLogs
| where ResultType == 0
| summarize Locations = make_set(Location)
    by UserPrincipalName
| where array_length(Locations) >= 2
