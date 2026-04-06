
# 🌐 Sentinel Deployment & Configuration


## 📝 Overview
The goal of this lab was to deploy Microsoft Sentinel, configure log ingestion, enable detection logic, in order to continue with further labs including threat hunting and incident response. 
Sentinel was deployed and managed through the Microsoft Defender portal (security.microsoft.com), (Microsoft is currently unifying it's security operations platform).

---

## 🧪 Environment Architecture
- **Platform:** Microsoft Azure  
- **SIEM:** Microsoft Sentinel  
- **Portal:** Microsoft Defender  
- **Log Analytics Workspace:** LAW‑Security‑labs  
- **Region:** UK South  
- **Retention:** 30 days  

---

## 📝 Data Connectors & Log Ingestion

The Log Analytics Workspace acts as the data repository for Sentinel.

LAW supports:
- Log ingestion  
- KQL querying  
- Analytics rule evaluation  
- Threat hunting  

![law](../../SOC_screenshots/Lab01- Sentinel SIEM Deployment/1-log-analytics-workspace.png)
##### Screenshot showing Log Analytics Workspace

### Identity & Security Connectors Used:

- Azure Activity  
- Microsoft Entra ID   
- Microsoft Defender for Office 365  

---

## 📡 Diagnostic Settings
Diagnostic settings were not enabled because the lab focused on identity and Defender telemetry, which send logs natively to the LAW. 

Diagnostic settings are required for Azure resource logs, such as:
Virtual Machines, Apps and Azure Key Vault. These logs flow through the Azure Monitor diagnostic pipeline into LAW.

#### Key takeaway:  
- Identity logs ≠ diagnostic settings required
- Resource logs = diagnostic settings required

---

## 📦 Analytics Rule Templates
Installing security solutions from the Content Hub populates the Analytics Rule Templates library.

### Key Takeaway 
Content Hub installation is required before templates appear and templates do not generate alerts until converted into active analytics rules. 

--- 

## 🔐 RBAC Configuration
Role‑based access control was configured to support least privilege principle.

Roles Assigned: 
- Sentinel Contributor, Log Analytics Reader, Security Reader

---

## 📊 Log Ingestion Validation
Log ingestion was validated using KQL queries in the Log Analytics Workspace.
Outcome:  All configured data sources successfully ingested into LAW.

---

## 🧠 Lessons learnt
- Sentinel is now part of the Microsoft Defender unified SOC platform.
- Identity and Defender logs ingest natively without diagnostic settings.
- Data connectors must be configured before rule templates appear.
- Content Hub must be installed before analytics rules appear.
- Diagnostic settings are required for Azure resource logs.
- Analytics rule templates must be activated to generate incidents.
- RBAC must follow least privilege principles.
- KQL validation confirms log ingestion.

