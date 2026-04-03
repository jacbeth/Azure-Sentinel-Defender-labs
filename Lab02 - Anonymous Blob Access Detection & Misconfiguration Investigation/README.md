## 📦 Lab 2 — Anonymous Blob Access Detection & Misconfiguration Analysis

### 1. Objective
Demonstrate how Azure logs unauthenticated blob access when a storage container is misconfigured with public access enabled, and that Azure can detect exposure events.

### 2. Environment Setup & Misconfiguration
#### Container Configuration
• 	Set the Public Access Level of the blob container to allow anonymous read access
• 	Uploaded a test file () to generate activity

#### Why this matters:
Public blob access is a common misconfiguration that exposes sensitive data without authentication. 

### 3. Anonymous Access Testing
Actions Performed
• 	Copied the blob URL directly from the Azure Portal
• 	Opened a private/incognito browser window
• 	Accessed the blob without authentication
• 	Refreshed the blob repeatedly to generate multiple anonymous read events

#### Key Observations
• 	Anonymous access requires no SAS token, no keys, and no Azure AD credentials
• 	Each access attempt generated a corresponding log entry in 

### 4. Log Ingestion Verification
Diagnostic settings were confirmed to be sending:
• 	StorageBlobLogs
• 	StorageRead
• 	StorageWrite

Waited 5 minutes for ingestion.

Fields of Interest in Logs
• 	AuthenticationType == "Anonymous"
• 	OperationName == "GetBlob"
• 	CallerIAddress
• 	Uri


### 5. Detection Queries (KQL)
#### 5.1 Anonymous Access Detection
```
StorageBlobLogs
| where AuthenticationType == "Anonymous"
| summarize AccessCount = count(), Blobs = make_set(Uri, 10)
    by CallerIpAddress, bin(TimeGenerated, 1h)
```

#### 5.2 Container Enumeration Detection  
```
StorageBlobLogs
| where Uri contains "comp=list"
| summarize ListOperations = count()
    by CallerIpAddress, bin(TimeGenerated, 1h)
```

#### 5.3 High‑Volume Anonymous Reads
```
StorageBlobLogs
| where AuthenticationType == "Anonymous"
| summarize TotalReads = count()
    by bin(TimeGenerated, 1h)
| where TotalReads > 20
```

### 6. Findings
- Anonymous blob access was successfully logged
- All access originated from the expected test IP address
- High‑volume reads were detected during testing
- No unexpected IPs or suspicious access patterns were observed

This confirms that Azure logs provide full visibility into anonymous access events.

### 7. Commentary
Azure logs anonymous access clearly, including:
- Caller IP
- Timestamp
- Blob URI
- Operation type

The lab mirrors real world cloud incidents where public access leads to data exposure. Anonymous reads, container listings, and repeated access attempts were all captured as expected.
#### Conclusion:
Azure provides sufficient telemetry to detect misconfigurations — as long as diagnostic settings are correctly configured.