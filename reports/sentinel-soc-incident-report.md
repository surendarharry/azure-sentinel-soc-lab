# Microsoft Sentinel SOC Incident Report

**Project Name:** Microsoft Sentinel SOC Lab  
**Environment:** Microsoft Azure / Microsoft Sentinel / Microsoft Defender for Cloud  
**Analyst:** Surendar Fernandez Harry  
**Report Type:** Security Operations Center (SOC) Incident Report  
**Date:** May 2026  

---

# Table of Contents

1. Executive Summary  
2. Project Objectives  
3. Environment Architecture  
4. Technologies Used  
5. Attack Simulations  
6. Detection Engineering  
7. Incident Investigations  
8. SOAR Automation  
9. Workbook Dashboards  
10. MITRE ATT&CK Mapping  
11. Findings & Observations  
12. Challenges Encountered  
13. Lessons Learned  
14. Conclusion  

---

# 1. Executive Summary

A cloud-native SOC environment was built using Microsoft Azure and Microsoft Sentinel to simulate enterprise-level security monitoring, detection engineering, incident response, and SOAR automation workflows.

The environment successfully generated and analyzed telemetry from simulated cyber attacks against a publicly exposed Windows honeypot VM.

The project demonstrated:
- SIEM deployment
- Threat detection engineering
- KQL analytics creation
- Incident response workflows
- Threat hunting
- Cloud identity monitoring
- SOAR automation using Azure Logic Apps

Multiple attack simulations were performed including:
- RDP brute force attacks
- Local administrator account creation
- Successful login after brute force
- Impossible travel authentication activity

Custom KQL detection rules successfully generated incidents which were triaged and investigated using Microsoft Sentinel incident management workflows.

---

# 2. Project Objectives

The objectives of this project were to:

- Deploy a cloud-native SIEM environment
- Centralize Windows and cloud identity telemetry
- Simulate realistic attack activity
- Build custom Sentinel analytics rules
- Investigate security incidents
- Implement SOAR automation workflows
- Visualize telemetry using Sentinel Workbooks
- Map detections to MITRE ATT&CK techniques

---

# 3. Environment Architecture

## Core Components

| Component | Purpose |
|---|---|
| Kali Linux | Attack simulation platform |
| Windows Honeypot VM | Attack target |
| Azure Monitor Agent | Telemetry collection |
| Log Analytics Workspace | Centralized log storage |
| Microsoft Sentinel | SIEM platform |
| Defender for Cloud | Threat detection |
| Microsoft Entra ID | Identity telemetry |
| Azure Logic Apps | SOAR automation |

---

# Telemetry Flow

```text
Windows Honeypot VM
        ↓
Azure Monitor Agent
        ↓
Log Analytics Workspace
        ↓
Microsoft Sentinel
        ↓
Analytics Rules & Incidents
```

---

# Identity Telemetry Flow

```text
Microsoft Entra ID
        ↓
SigninLogs
        ↓
Microsoft Sentinel
        ↓
Impossible Travel Detection
```

---

# SOAR Workflow

```text
Sentinel Incident
        ↓
Azure Logic App Triggered
        ↓
Incident Enrichment
        ↓
Automated Response Workflow
```

---

# 4. Technologies Used

| Technology | Purpose |
|---|---|
| Microsoft Azure | Cloud infrastructure |
| Microsoft Sentinel | SIEM |
| Log Analytics Workspace | Telemetry storage |
| Microsoft Defender for Cloud | Threat alerts |
| Microsoft Entra ID | Identity monitoring |
| Azure Logic Apps | SOAR workflows |
| Kali Linux | Attack platform |
| Hydra | RDP brute force |
| Atomic Red Team | ATT&CK simulations |
| KQL | Detection engineering |

---

# 5. Attack Simulations

## 5.1 RDP Brute Force Attack

### Objective
Simulate external password brute-force activity against an exposed RDP service.

### Tool Used
- Hydra

### Command

```bash
hydra -L users.txt -P passwords.txt rdp://<VM_PUBLIC_IP> -t 4
```

### Expected Telemetry
- Event ID 4625
- Failed authentication logs
- External source IP activity

---

## 5.2 Local Administrator Account Creation

### Objective
Simulate persistence and privilege escalation behavior.

### Tool Used
- Atomic Red Team

### MITRE ATT&CK
- T1136.001 — Create Local Account

### Command

```powershell
Invoke-AtomicTest T1136.001
```

### Expected Telemetry
- Event ID 4720
- Event ID 4728

---

## 5.3 Successful Login After Brute Force

### Objective
Simulate successful access after repeated authentication failures.

### Expected Telemetry
- Event ID 4624
- Event ID 4625

---

## 5.4 Impossible Travel Authentication

### Objective
Simulate suspicious cloud identity behavior using geographically distant login locations.

### Data Source
- Microsoft Entra ID Sign-in Logs

### Expected Telemetry
- Multiple geographic login locations
- Successful authentication events

---

# 6. Detection Engineering

## 6.1 Brute Force Detection Rule

### MITRE ATT&CK
- T1110 — Brute Force

### KQL Query

```kql
SecurityEvent
| where EventID == 4625
| where TimeGenerated > ago(1h)
| summarize FailCount = count() by IpAddress, Account, Computer
| where FailCount >= 5
```

### Severity
Medium

---

## 6.2 Local Admin Account Detection

### MITRE ATT&CK
- T1136.001 — Create Local Account

### KQL Query

```kql
SecurityEvent
| where EventID == 4720 or EventID == 4728
| project TimeGenerated, Computer, TargetUserName, SubjectUserName
```

### Severity
High

---

## 6.3 Successful Login After Brute Force

### MITRE ATT&CK
- T1110 — Brute Force
- T1078 — Valid Accounts

### KQL Query

```kql
let FailedLogins =
SecurityEvent
| where EventID == 4625
| summarize FailCount=count() by IpAddress, Account
| where FailCount >= 5;

SecurityEvent
| where EventID == 4624
| join kind=inner FailedLogins on Account
```

### Severity
High

---

## 6.4 Impossible Travel Detection

### MITRE ATT&CK
- T1078 — Valid Accounts

### KQL Query

```kql
SigninLogs
| where ResultType == 0
| summarize Locations = make_set(Location)
    by UserPrincipalName
| where array_length(Locations) >= 2
```

### Severity
High

---

# 7. Incident Investigations

## 7.1 RDP Brute Force Incident

### Summary
Multiple failed RDP authentication attempts were detected from an external IP address.

### Event IDs
- 4625

### Investigation Actions
- Reviewed failed login telemetry
- Investigated source IP
- Correlated authentication events

### Outcome
- True Positive

---

## 7.2 Local Admin Account Creation Incident

### Summary
A new local administrator account was created on the Windows honeypot VM.

### Event IDs
- 4720
- 4728

### Investigation Actions
- Reviewed account creation events
- Investigated privileged group activity
- Confirmed persistence behavior

### Outcome
- True Positive

---

## 7.3 Successful Login After Brute Force

### Summary
Successful authentication occurred after repeated failed login attempts.

### Investigation Actions
- Correlated successful and failed logins
- Identified authentication sequence
- Investigated source IP

### Outcome
- True Positive

---

## 7.4 Impossible Travel Authentication

### Summary
Successful sign-ins occurred from geographically distant locations within a short period of time.

### Investigation Actions
- Reviewed Entra ID sign-in telemetry
- Compared geographic login locations
- Validated successful authentication activity

### Outcome
- True Positive

---

# 8. SOAR Automation

## Playbook Name

```text
disable-compromised-account
```

---

# Workflow Summary

1. Sentinel incident generated
2. Azure Logic App triggered automatically
3. Incident enrichment performed
4. Automated comments added to incident

---

# Technologies Used

- Microsoft Sentinel
- Azure Logic Apps
- Microsoft Entra ID

---

# Automated Comment

```text
SOAR playbook executed automatically after incident creation. Investigation workflow initiated for suspicious authentication activity.
```

---

# Security Benefits

- Faster incident response
- Automated enrichment workflows
- Reduced manual analyst effort
- Improved SOC operational efficiency

---

# 9. Workbook Dashboards

Custom Sentinel Workbooks were created to visualize:

- Failed RDP login trends
- Top attacker IP addresses
- Security incident severity
- Authentication trends
- Global sign-in activity

---

# Dashboard Metrics

| Visualization | Purpose |
|---|---|
| Failed Login Timechart | Authentication monitoring |
| Top Attacker IPs | Threat visibility |
| Incident Severity Pie Chart | SOC metrics |
| Global Login Map | Identity monitoring |

---

# 10. MITRE ATT&CK Mapping

| Detection | MITRE Technique |
|---|---|
| Brute Force Detection | T1110 |
| Local Account Creation | T1136.001 |
| Successful Login After Brute Force | T1078 |
| Impossible Travel Authentication | T1078 |
| PowerShell Execution | T1059.001 |
| Scheduled Task Creation | T1053.005 |

---

# 11. Findings & Observations

## Key Findings

| Finding | Impact |
|---|---|
| Exposed RDP service received brute-force attempts | High risk |
| Windows telemetry ingestion successful | Positive |
| Sentinel incidents generated correctly | Positive |
| Atomic Red Team simulations produced expected telemetry | Positive |
| Cloud identity telemetry successfully monitored | Positive |
| SOAR workflows integrated successfully | Positive |

---

# 12. Challenges Encountered

| Challenge | Resolution |
|---|---|
| Sentinel portal migration | Updated workflow navigation |
| Azure AD renamed to Entra ID | Updated connector configuration |
| Logic App tenant authorization issues | Resolved via tenant switching |
| Remediation permission restrictions | Used enrichment-only workflow |

---

# 13. Lessons Learned

## Technical Skills Developed

- Microsoft Sentinel deployment
- KQL analytics engineering
- Windows event analysis
- Microsoft Entra ID monitoring
- Logic Apps automation
- SIEM dashboard creation
- SOC investigation workflows
- MITRE ATT&CK operational mapping

---

# Operational Lessons

- Cloud-native SIEM environments differ significantly from traditional on-prem SIEM deployments.
- Identity telemetry is critical for modern SOC operations.
- KQL provides flexible and powerful detection engineering capabilities.
- SOAR automation improves response efficiency and incident handling.

---

# 14. Conclusion

This project successfully simulated a modern cloud-native SOC environment using Microsoft Azure and Microsoft Sentinel.

The lab demonstrated:
- attack simulation
- centralized telemetry ingestion
- SIEM operations
- detection engineering
- incident response
- cloud identity monitoring
- workbook visualization
- SOAR automation

The environment provided practical hands-on experience with enterprise-level cloud security operations and modern SOC workflows.
