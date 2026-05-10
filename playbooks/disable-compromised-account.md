# Disable Compromised Account Playbook

## Overview

This Azure Logic App playbook was created to simulate automated incident response workflows using Microsoft Sentinel and Azure Logic Apps.

The playbook is triggered automatically when a Microsoft Sentinel incident is generated.

---

# Technologies Used

- Microsoft Sentinel
- Azure Logic Apps
- Microsoft Entra ID
- Microsoft Defender Portal

---

# Playbook Name

```text
disable-compromised-account
```

---

# Workflow Objective

The objective of the playbook is to automate incident response and enrichment activities after suspicious authentication activity is detected.

---

# Trigger

## Microsoft Sentinel Incident Trigger

The playbook uses the Microsoft Sentinel incident trigger to automatically initiate the workflow when incidents are created.

---

# Workflow Actions

## 1. Incident Trigger
Sentinel incident generated.

## 2. Logic App Execution
Azure Logic App triggered automatically.

## 3. Incident Enrichment
Automated comments added to the incident.

## 4. Optional Remediation
Attempted automated account disable workflow.

---

# Automated Comment

```text
SOAR playbook executed automatically after incident creation. Investigation workflow initiated for suspicious authentication activity.
```

---

# SOAR Workflow

```text
Microsoft Sentinel Incident
        ↓
Azure Logic App Triggered
        ↓
Incident Enrichment
        ↓
Automated Investigation Actions
```

---

# Security Benefits

- Faster incident response
- Reduced manual analyst effort
- Automated enrichment workflows
- Improved SOC operational efficiency

---

# Challenges Encountered

## Tenant Authorization Issues
Logic App authentication initially failed due to Azure tenant mismatches.

## Permission Restrictions
Automated remediation actions were limited in the free/student Azure environment.

---

# Lessons Learned

This playbook demonstrated:
- Sentinel automation integration
- Azure Logic Apps workflows
- SOAR concepts
- Automated incident handling
- Cloud-native security orchestration

---

# Conclusion

The playbook successfully demonstrated automated incident response workflows using Microsoft Sentinel and Azure Logic Apps.
