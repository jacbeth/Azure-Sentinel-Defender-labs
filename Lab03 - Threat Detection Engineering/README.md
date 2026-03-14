\# Lab 3 – Azure Threat Detection (StorageBlobLogs + AzureActivity)



<!-- jacbeth Security Labs Branding -->

!\[jacbeth Labs](https://img.shields.io/badge/jacbeth%20Labs-Cybersecurity-0A0A0A)

!\[Status: Completed](https://img.shields.io/badge/Status-Completed-2ECC71)

!\[Category: SOC Lab](https://img.shields.io/badge/Category-SOC%20Lab-0078D6)

!\[Platform: Microsoft%20Azure](https://img.shields.io/badge/Platform-Microsoft%20Azure-0078D6)

!\[Tool: Sentinel](https://img.shields.io/badge/Tool-Sentinel-8A2BE2)

!\[Detection: Threat%20Hunting](https://img.shields.io/badge/Detection-Threat%20Hunting-FF8800)

!\[MITRE: Multi](https://img.shields.io/badge/MITRE-Multi-C0392B)



\---



\## 📝 Overview

Azure Storage is a high‑value target for attackers due to its use for backups, logs, application data, and sensitive files.  

This lab focuses on identifying suspicious access patterns within Azure Storage accounts using \*\*Microsoft Sentinel\*\* and \*\*KQL\*\*.



The goal is to simulate threat detection work by analysing blob access logs, identifying anomalies, and mapping detections to MITRE ATT\&CK.



\---



\## 📡 Data Sources

This lab primarily uses:



\### \*\*StorageBlobLogs\*\*

Operational logs for blob access, including:

\- Blob downloads (GetBlob)  

\- Blob deletions (DeleteBlob)  

\- Authentication type (Key, OAuth, SAS)  

\- Caller IP address  

\- URI and container information  



\### \*\*AzureActivity\*\*

Control‑plane operations such as:

\- SAS token creation  

\- Storage account key regeneration  

\- Network rule changes  

\- Role assignments and permission changes  



These events are essential for detecting attacker attempts to weaken storage security or establish persistence.



\---



\## 🔍 Detection 1 — Repeated Blob Downloads from the Same IP

```kql

StorageBlobLogs

| where OperationName == "GetBlob"

| summarize DownloadCount = count() by CallerIpAddress, bin(TimeGenerated, 15m)

| where DownloadCount > 25

| sort by DownloadCount desc



