
# 🌐 Sentinel Deployment & Configuration


## 📝 Overview
The goal of this lab is to deploy Microsoft Sentinel, configure log ingestion, enable detection logic, and establish the foundation for SOC operations including threat hunting and incident response.

Sentinel was deployed and managed through the **Microsoft Defender portal (security.microsoft.com)**, reflecting Microsoft’s unified security operations platform.

---

## 🧪 Environment Architecture
- **Platform:** Microsoft Azure  
- **SIEM:** Microsoft Sentinel  
- **Portal:** Microsoft Defender  
- **Log Analytics Workspace:** LAW‑Security‑labs  
- **Region:** UK South  
- **Retention:** 30 days  

### Log Flow
Security Data Sources  
(Microsoft Entra ID, Defender XDR, Azure Activity)  
  ↓  
**Log Analytics Workspace (LAW‑Security‑labs)**  
  ↓  
**Microsoft Sentinel**  
  ↓  
Analytics Rules → Incidents → SOC Investigation  

---

## 📝 Data Connectors & Log Ingestion

The Log Analytics Workspace serves as the central data repository for Sentinel.

LAW supports:
- Log ingestion  
- KQL querying  
- Analytics rule evaluation  
- Threat hunting  

### Identity & Security Connectors Used:

- Azure Activity  
- Microsoft Entra ID   
- Microsoft Defender for Office 365  

### Important 
These sources send logs natively to LAW. They do not require diagnostic settings. Identity and Defender telemetry is high‑value SOC data and forms the backbone of most detection logic.

## 📡 Diagnostic Settings
Diagnostic settings were not enabled in this initial deployment because the lab focused on identity and Defender telemetry. Diagnostic settings are required for Azure resource logs, such as:
Virtual Machines, Apps and Azure Key Vault

These logs flow through the Azure Monitor diagnostic pipeline into LAW.

Key takeaway:  
Identity logs ≠ diagnostic settings
Resource logs = diagnostic settings required

## 📦 Analytics Rule Templates
Installing security solutions from the Content Hub populates the Analytics Rule Templates library.

### Important 
Templates do not generate alerts until converted into active analytics rules
Content Hub installation is required before templates appear

146 rule templates available for activation.

## 🔐 RBAC Configuration
Role‑based access control was configured to support least‑privilege SOC operations.

### Roles Assigned
- Sentinel Contributor – manage analytics rules, incidents, automation
- Log Analytics Reader – query and analyse logs
- Security Reader – view alerts and security posture

### Elevated Access Warning
Azure displayed a notification indicating that elevated access had been temporarily enabled at the tenant level.

This occurs when:

- Administrators elevate permissions to assign roles
- Privileged Identity Management (PIM) activates a role
- Tenant‑wide changes require higher privileges

NB:  Elevated access can be disabled once configuration is complete.

## 📊 Log Ingestion Validation
Log ingestion was validated using KQL queries in the Log Analytics Workspace.
Outcome:  All configured data sources successfully ingested into LAW.

## 🧠 Lessons learnt
- Sentinel is now part of the Microsoft Defender unified SOC platform
- Identity and Defender logs ingest natively without diagnostic settings
- Data connectors must be configured before rule templates appear
- Content Hub must be installed before analytics rules appear
- Diagnostic settings are required for Azure resource logs
- Analytics rule templates must be activated to generate incidents
- RBAC must follow least‑privilege principles
- KQL validation is essential to confirm ingestion

