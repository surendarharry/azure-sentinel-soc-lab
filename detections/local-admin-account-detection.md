
---

# 2. `local-admin-account-detection.md`

## Purpose

Documents:
- suspicious local admin creation

---

## Include

```markdown id="md2"
# Local Admin Account Detection Rule

## Description
Detects creation of local user accounts and privileged group membership activity.

## MITRE ATT&CK
- T1136.001 — Create Local Account

## Data Source
- SecurityEvent

## Event IDs
- 4720
- 4728

## KQL Query

```kql
SecurityEvent
| where EventID == 4720 or EventID == 4728
| project TimeGenerated, Computer, TargetUserName, SubjectUserName
