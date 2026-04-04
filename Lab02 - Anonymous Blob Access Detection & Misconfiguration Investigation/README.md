## 📘 Lab 2 — Anonymous Blob Access Detection & Misconfiguration Analysis

### 🔍 Overview
The lab demonstrates how anonymous public access to Azure Blob Storage can be detected. A storage container was misconfigured with unathenticated blob access, to generate logs for analysis in Microsoft Defender and Log Analytics.

### 🎯 Learning Objectives
•   Understand how anonymous blob access works and why it’s risky
• 	Identify misconfigurations in Azure Storage security settings
• 	Detect unauthenticated access attempts 
• 	Build a detection query for public blob enumeration or download
• 	Capture evidence for portfolio documentation

### 🏗️ Lab Architecture
• 	Azure Storage Account with a container configured for anonymous blob access
• 	Log Analytics Workspace for ingestion
• 	Microsoft Defender for Cloud for alerts
• 	Attacker simulation using browser to access blobs anonymously

### 🧪 Key Steps
1. 	Create a Storage Account and enable blob unauthenticated access
2. 	Configure a container with unauthenticated access
3. 	Upload test file
4. 	Access blobs anonymously to simulate exploitation
5. 	Validate logs 
6. 	Build KQL queries to detect anonymous access
7. 	Disable public access and validate remediation

### 🛡️ Detection Queries (KQL)
#### Anonymous Access Detection
```
StorageBlobLogs
| where AuthenticationType == "Anonymous"
| summarize AccessCount = count(), Blobs = make_set(Uri, 10)
    by CallerIpAddress, bin(TimeGenerated, 1h)
```

#### Screenshot of Anonymous access KQL query and results
![anonymousaccess](./screenshots/filename.png)

#### Container Enumeration Detection  
```
StorageBlobLogs
| where Uri contains "comp=list"
| summarize ListOperations = count()
    by CallerIpAddress, bin(TimeGenerated, 1h)
```

#### Screenshot of Container listing KQL query and results
![containerlisting](./screenshots/filename.png)

#### High‑Volume Anonymous Reads
```
StorageBlobLogs
| where AuthenticationType == "Anonymous"
| summarize TotalReads = count()
    by bin(TimeGenerated, 1h)
| where TotalReads > 20
```

#### Screenshot of High‑volume access detection results
![highvolumeaccess](./screenshots/filename.png)

### ✅ Findings
• Anonymous blob access was successfully logged.
• All access originated from the expected test IP address.
• Container listing operations (comp=list) were captured, showing that public access allowed enumeration of blob contents.
• High volume reads were detected during testing, confirming that repeated anonymous access is fully logged.
• No unexpected IPs or suspicious access patterns were observed.

The findings confirm that Azure logs provide full visibility into anonymous access events.

### 🧠 Conclusion:
Azure logs anonymous access, including the caller IP, timestamp, and blob URI, which is essential for investigations dealing with misconfigurations that expose data publicly. The behaviour observed in this lab matches real world cloud incidents where public containers lead to data exposure. This demonstrates that Azure provides sufficient telemetry to detect misconfigurations, provided diagnostic settings are correctly configured. 