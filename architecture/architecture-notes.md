# Architecture Overview

This project simulates a cloud-native Security Operations Center (SOC) environment using Microsoft Azure, Microsoft Sentinel, Microsoft Defender for Cloud, Microsoft Entra ID, and Azure Logic Apps.

The lab was designed to emulate real-world enterprise security monitoring, threat detection, incident response, and SOAR automation workflows.

---

# Architecture Objectives

The primary objectives of this lab were to:

- Deploy a cloud-native SIEM environment
- Centralize Windows and identity telemetry
- Simulate realistic cyber attack scenarios
- Build custom KQL analytics detections
- Generate and triage security incidents
- Implement SOAR automation workflows
- Map detections to MITRE ATT&CK techniques

---

# High-Level Architecture

The environment consists of:

1. Kali Linux attacker machine
2. Publicly exposed Windows honeypot VM
3. Azure Monitor Agent (AMA)
4. Log Analytics Workspace
5. Microsoft Sentinel SIEM
6. Microsoft Defender for Cloud
7. Microsoft Entra ID
8. Azure Logic Apps SOAR playbook

---

# Core Components

## 1. Kali Linux Attacker Machine

The attacker system was used to simulate malicious activity against the Azure-hosted Windows VM.

### Attack Tools Used
- Hydra
- Atomic Red Team
- VPN service for impossible travel simulation

### Attack Simulations
- RDP brute force attacks
- Persistence activity
- Credential access simulations
- Impossible travel authentication

---

## 2. Windows Honeypot VM

A publicly accessible Windows Server VM was deployed inside Microsoft Azure to act as the primary attack target.

### Configuration
- Public IP enabled
- RDP exposed on port 3389
- Windows Security Event logging enabled
- Azure Monitor Agent installed

### Purpose
- Generate realistic Windows telemetry
- Simulate enterprise endpoint monitoring
- Produce authentication and persistence events

---

## 3. Azure Monitor Agent (AMA)

The Azure Monitor Agent was installed on the Windows VM to collect and forward security telemetry to the Log Analytics Workspace.

### Collected Data
- SecurityEvent logs
- Authentication events
- Account management events
- Scheduled task activity

### Function
- Centralized telemetry ingestion
- Windows event forwarding
- Sentinel integration

---

## 4. Log Analytics Workspace

The Log Analytics Workspace acted as the centralized data repository for all collected security telemetry.

### Data Sources
- Windows Security Events
- Microsoft Entra ID Sign-in Logs
- Defender for Cloud Alerts

### Purpose
- Queryable telemetry storage
- KQL analytics backend
- SIEM data lake

---

## 5. Microsoft Sentinel

Microsoft Sentinel served as the primary SIEM platform for:
- threat detection
- incident generation
- investigation workflows
- workbook dashboards
- analytics rule creation

### Key Functions
- KQL-based detection engineering
- Incident management
- Entity correlation
- Threat hunting
- MITRE ATT&CK mapping

---

# Detection Engineering

Custom KQL analytics rules were created to detect simulated attack activity.

## Detection Rules Implemented

| Detection | MITRE Technique |
|---|---|
| Brute Force Detection | T1110 |
| Local Account Creation | T1136.001 |
| Successful Login After Brute Force | T1078 |
| Impossible Travel Authentication | T1078 |

---

# Telemetry Flow

## Windows Telemetry Pipeline

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

## Identity Telemetry Pipeline

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

## Defender Alert Pipeline

```text
Microsoft Defender for Cloud
        ↓
Security Alerts
        ↓
Microsoft Sentinel
        ↓
Incident Correlation
```

---

# Attack Flow

The simulated attack sequence followed this workflow:

```text
Kali Linux Attacker
        ↓
Hydra RDP Brute Force
        ↓
Windows Security Events Generated
        ↓
Telemetry Sent to Sentinel
        ↓
KQL Analytics Rules Triggered
        ↓
Incidents Generated
        ↓
SOC Investigation Performed
        ↓
SOAR Automation Triggered
```

---

# Incident Response Workflow

Generated incidents were investigated using Microsoft Sentinel incident management features.

## Investigation Activities
- Alert review
- Timeline analysis
- Entity investigation
- MITRE ATT&CK mapping
- IOC identification
- Incident classification
- Incident closure

---

# SOAR Automation

Azure Logic Apps were integrated with Sentinel to simulate automated incident response workflows.

## Playbook Name
disable-compromised-account

## Workflow
1. Sentinel incident generated
2. Logic App triggered automatically
3. Incident enrichment performed
4. Automated comments added to incident

---

# Workbook Dashboard

Custom Sentinel workbook dashboards were created to visualize:

- Failed RDP login trends
- Top attacker IP addresses
- Incident severity metrics
- Authentication statistics
- Global sign-in activity

---

# Security Events Monitored

| Event ID | Description |
|---|---|
| 4625 | Failed login |
| 4624 | Successful login |
| 4720 | User account creation |
| 4728 | Privileged group membership |
| 4698 | Scheduled task creation |

---

# MITRE ATT&CK Coverage

| Technique ID | Description |
|---|---|
| T1110 | Brute Force |
| T1136.001 | Create Local Account |
| T1059.001 | PowerShell |
| T1053.005 | Scheduled Task |
| T1003.001 | LSASS Credential Dumping |
| T1078 | Valid Accounts |

---

# Challenges Encountered

## Azure Portal Migration
Microsoft Sentinel analytics and incidents were migrated into the Microsoft Defender portal, requiring updated navigation and workflow adjustments.

## Tenant Authorization Issues
Logic App authentication initially failed due to tenant mismatches and Azure session conflicts.

## Limited Remediation Permissions
Some automated remediation actions were restricted in the free/student Azure environment.

---

# Lessons Learned

This project provided practical experience with:

- Cloud-native SIEM architecture
- Windows event analysis
- KQL detection engineering
- Threat hunting workflows
- Incident response operations
- Sentinel workbook development
- Microsoft Entra ID monitoring
- SOAR automation concepts
- MITRE ATT&CK operational mapping

---

# Conclusion

This lab successfully simulated a modern cloud SOC environment using Microsoft Azure security technologies.

The project demonstrated:
- realistic attack simulation
- centralized telemetry ingestion
- detection engineering
- incident investigation
- cloud identity monitoring
- SIEM operations
- SOAR integration

The environment provided hands-on experience with enterprise-level security operations workflows and cloud security monitoring techniques.
