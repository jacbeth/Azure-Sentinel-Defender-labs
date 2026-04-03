## 📦 Lab 2 — Anonymous Blob Access Detection & Misconfiguration Analysis

### 1. Objective
Demonstrate how Azure logs unauthenticated blob access when a storage container is misconfigured with public access enabled, and that Azure can detect exposure events.

### 2. Environment Setup & Misconfiguration
Public Access Level of the blob container was set to allow anonymous read access and a test file was uploaded to generate activity.

#### Why this matters:
Public blob access is a common misconfiguration that exposes sensitive data without authentication. 

### 3. Anonymous Access Testing
Accessed the blob without authentication and refreshed the blob repeatedly to generate multiple anonymous read events.

#### Key Observations
• 	Anonymous access requires no SAS token, no keys, and no Azure AD credentials
• 	Each access attempt generated a corresponding log entry.

### 4. Log Ingestion Verification
Diagnostic settings were confirmed to be sending: StorageBlobLogs, StorageRead and StorageWrite

### 5. Findings
 Anonymous blob access was successfully logged.
- All access originated from the expected test IP address.
- Container listing operations (comp=list) were captured, showing that public access allowed enumeration of blob contents.
- High volume reads were detected during testing, confirming that repeated anonymous access is fully logged.
- No unexpected IPs or suspicious access patterns were observed.

This confirms that Azure logs provide full visibility into anonymous access events.

### 6. Commentary
Azure logs anonymous access clearly, including:
- Caller IP
- Timestamp
- Blob URI
- Operation type

#### 7. Conclusion:
Azure logs anonymous access, including the caller IP, timestamp, and blob URI, which is essential for investigations dealing with misconfigurations that expose data publicly. The behaviour observed in this lab matches real world cloud incidents where public containers lead to data exposure. This demonstrates that Azure provides sufficient telemetry to detect misconfigurations, provided diagnostic settings are correctly configured. 