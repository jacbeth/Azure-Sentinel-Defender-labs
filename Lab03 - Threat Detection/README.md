## Lab Azure Threat Detection (StorageBlobLogs + AzureActivity)

### 📝 Overview
Azure Storage is a high value target for attackers due to its use for backups, logs, application data, and sensitive files.  
The goal of the lab is to simulate threat detection by analysing blob access logs, identifying anomalies, and mapping detections to MITRE ATT&CK.

### 📡 Data Sources
#### StorageBlobLogs
- Blob downloads (GetBlob)  
- Blob deletions (DeleteBlob)  
- Authentication type (Key, OAuth, SAS)  
- Caller IP address  
- URI and container information  
#### AzureActivity
- SAS token creation  
- Storage account key regeneration  
- Network rule changes  
- Role assignments and permission changes  

### Detection 1 — Repeated Blob Downloads from the Same IP
```kql
StorageBlobLogs
| where OperationName == "GetBlob"
| summarize DownloadCount = count() by CallerIpAddress, bin(TimeGenerated, 15m)
| where DownloadCount > 25
| sort by DownloadCount desc
```
- Repeated downloads from a single IP could indicate: automated scripts, credential misuse or early stage data exfiltration
### MITRE ATT&CK MAPPING:
- Exfiltration (TA0010) and Exfiltration Over Web Services (T1567)
### Detection 2 — Blob Access from Unusual or Non‑Corporate IP Ranges
```kql
StorageBlobLogs
| where OperationName == "GetBlob"
| where CallerIpAddress !startswith "192.168."
    and CallerIpAddress !startswith "10."
    and CallerIpAddress !startswith "172.16."
| summarize AccessCount = count() by CallerIpAddress, bin(TimeGenerated, 1h)
| where AccessCount > 0
``` 
- Unexpected IPs may indicate credential compromise, SAS token leakage or external reconnaissance
### MITRE ATT&CK MAPPING:
- Initial Access (TA0001) and Valid Accounts (T1078)
### Detection 3 — Blob Access Using SAS TokensStorageBlobLogs
```kql 
StorageAzureBlobbs
| where AuthenticationType == "SAS"
| summarize SASAccessCount = count(), Blobs = make_set(Uri, 5)
    by CallerIpAddress, bin(TimeGenerated, 1h)
```
- SAS tokens are powerful because: they bypass credentials, they grant scoped access and if leaked, they enable silent data access
### MITRE ATT&CK MAPPING:
- Defense Evasion (TA0005) and Use of Credentials (T1550)
### 🔍 Detection 4 — Blob Deletions
```kql 
StorageBlobLogs
| where OperationName == "DeleteBlob"
| summarize DeleteCount = count(), DeletedBlobs = make_set(Uri, 10)
    by CallerIpAddress, bin(TimeGenerated, 1h)
```
- Blob deletions may indicate: Cleanup after data theft, malicious tampering or attempts to hide activity
## MITRE ATT&CK MAPPING:
- Impact (TA0040) and Data Destruction (T1485)
### Findings
- Repeated downloads from a single IP
- SAS token usage from a public IP
- Blob deletions performed by a test account
These behaviours clearly distinguish normal vs suspicious access patterns
### 🪞 Conclusion
This lab strengthened my understanding of cloud storage attack surfaces and the importance of monitoring both data‑plane and control‑plane activity. Even small anomalies — such as repeated downloads or unexpected IPs — can be early indicators of compromise.