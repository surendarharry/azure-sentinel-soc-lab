# New Local Admin Account Created

## Incident Summary

A high-severity alert was generated after a new local administrator account was created on the Windows honeypot VM using Atomic Red Team simulation activity.

## Severity
High

## MITRE ATT&CK
- T1136.001 — Create Local Account

## Detection Rule
Local Admin Account Detection

## Event IDs
- 4720
- 4728

## Investigation Timeline

| Time | Event |
|---|---|
| 10:30 | Atomic Red Team executed |
| 10:31 | Event ID 4720 generated |
| 10:32 | Sentinel incident triggered |

## Investigation Actions

- Reviewed account creation logs
- Confirmed privileged account activity
- Investigated affected device
- Reviewed related events

## Evidence

- Event ID 4720 logs
- Sentinel incident data
- Investigation screenshots

## Outcome

Incident classified as:
- True Positive

## Lessons Learned

Unauthorized local account creation is a common persistence technique used by attackers after initial compromise.
