# Microsoft Sentinel SOC Lab

A cloud-native SOC/SIEM lab built using Microsoft Azure, Microsoft Sentinel, Defender for Cloud, Microsoft Entra ID, and Azure Logic Apps.

This project simulates real-world cyber attack scenarios against a monitored Windows honeypot VM and demonstrates:
- SIEM engineering
- Detection engineering
- Incident response
- Threat hunting
- Cloud identity monitoring
- SOAR automation
- MITRE ATT&CK mapping

---

# Project Overview

This lab environment was designed to simulate a real Security Operations Center (SOC) workflow using Microsoft security technologies.

The project includes:
- Windows honeypot VM exposed to the internet
- Microsoft Sentinel SIEM deployment
- Azure Monitor Agent telemetry ingestion
- Microsoft Defender for Cloud integration
- Microsoft Entra ID sign-in monitoring
- Atomic Red Team attack simulations
- KQL analytics rules
- Logic App SOAR automation
- Incident investigation and triage
- Sentinel workbook dashboards

---

# Technologies Used

| Technology | Purpose |
|---|---|
| Microsoft Azure | Cloud infrastructure |
| Microsoft Sentinel | SIEM platform |
| Log Analytics Workspace | Centralized logging |
| Defender for Cloud | Threat detection |
| Microsoft Entra ID | Identity telemetry |
| Azure Logic Apps | SOAR automation |
| Atomic Red Team | Attack simulation |
| Kali Linux | Attack platform |
| Hydra | RDP brute force simulation |
| Kusto Query Language (KQL) | Detection engineering |

---

# Lab Architecture

## Components

- Kali Linux attacker machine
- Windows Server honeypot VM
- Azure Monitor Agent
- Log Analytics Workspace
- Microsoft Sentinel
- Microsoft Defender for Cloud
- Microsoft Entra ID
- Azure Logic Apps

---

# Attack Simulations

## 1. RDP Brute Force Attack

A brute-force attack was simulated from Kali Linux using Hydra against the exposed RDP service.

### Tool
- Hydra

### Command

```bash
hydra -L users.txt -P passwords.txt rdp://<VM_PUBLIC_IP> -t 4
```

### Expected Telemetry
- Event ID 4625
- Failed RDP authentication attempts
- Source IP telemetry

---

## 2. Local Administrator Account Creation

Simulated persistence and privilege escalation activity using Atomic Red Team.

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

## 3. Successful Login After Brute Force

A successful RDP login was performed after generating multiple failed authentication attempts.

### MITRE ATT&CK
- T1110 — Brute Force
- T1078 — Valid Accounts

### Expected Telemetry
- Event ID 4624
- Event ID 4625

---

## 4. Impossible Travel Authentication

Authentication attempts were performed from geographically distant locations using a VPN.

### Data Source
- Microsoft Entra ID Sign-in Logs

### Expected Telemetry
- Multiple geographic login locations
- Suspicious sign-in activity

---

# Detection Engineering

## Brute Force Detection Rule

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

---

## New Local Admin Account Detection Rule

### MITRE ATT&CK
- T1136.001 — Create Local Account

### KQL Query

```kql
SecurityEvent
| where EventID == 4720 or EventID == 4728
| project TimeGenerated, Computer, TargetUserName, SubjectUserName
```

---

## Successful Login After Brute Force Detection

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

---

## Impossible Travel Detection Rule

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

---

# Incident Response & Triage

Incidents generated during the lab were investigated using Microsoft Sentinel incident workflows.

## Incident Types Investigated

- RDP Brute Force Activity
- Local Admin Account Creation
- Successful Login After Brute Force
- Impossible Travel Authentication

## Investigation Activities

- Timeline analysis
- Entity investigation
- MITRE ATT&CK mapping
- Log correlation
- Incident classification
- Evidence review
- Incident closure

---

# SOAR Automation

A Microsoft Sentinel-triggered Azure Logic App playbook was created to automate incident response workflows.

## Playbook Name

```text
disable-compromised-account
```

## Automated Workflow

1. Sentinel incident generated
2. Logic App triggered automatically
3. Incident enrichment performed
4. Automated investigation comments added

---

# Sentinel Workbook Dashboard

A custom Sentinel workbook dashboard was built to visualize:
- Failed RDP login attempts
- Top attacker IP addresses
- Security incident severity
- Authentication trends
- Global login activity

---

# MITRE ATT&CK Mapping

| Detection | MITRE Technique |
|---|---|
| Brute Force Detection | T1110 |
| Local Account Creation | T1136.001 |
| Successful Login After Brute Force | T1078 |
| Impossible Travel Authentication | T1078 |

---

# Skills Demonstrated

- Microsoft Sentinel
- SIEM Engineering
- KQL Detection Engineering
- Threat Hunting
- Incident Response
- SOC Operations
- Windows Event Analysis
- Cloud Security Monitoring
- Microsoft Entra ID Monitoring
- MITRE ATT&CK Mapping
- Azure Logic Apps
- SOAR Automation

---

# Challenges Encountered

| Challenge | Resolution |
|---|---|
| Sentinel UI migration to Defender portal | Adjusted workflow |
| Azure AD renamed to Entra ID | Updated connectors |
| Logic App tenant authorization issues | Resolved via tenant switching |
| Limited remediation permissions | Used enrichment workflow |

---

# Lessons Learned

- Cloud-native SIEM deployment and telemetry ingestion
- KQL-based detection engineering
- Windows authentication event analysis
- Identity threat monitoring
- SOC investigation workflows
- Sentinel incident triage
- Logic App SOAR integration
- MITRE ATT&CK operational mapping

---

# Conclusion

This project successfully simulated a modern cloud-native SOC environment using Microsoft security technologies.

The lab demonstrated:
- attack simulation
- telemetry ingestion
- detection engineering
- incident response
- cloud identity monitoring
- SOAR automation

This project provided hands-on experience with real-world SOC workflows and enterprise cloud security operations.
