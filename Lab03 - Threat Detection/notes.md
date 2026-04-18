

## 4. Detection 2 — Blob Access from Unusual IP Ranges
Detects blob access from non‑private IP ranges. 
Findings:  External IPs 92.40.169.164 and 195.149.13.240 accessed blobs — expected from home network testing.

## Commentary:  
Unexpected IPs may indicate credential compromise or SAS token leakage.

## MITRE ATT&CK:

- Initial Access (TA0001)
- Valid Accounts (T1078)

## 5. Detection 3 — Blob Access Using SAS Tokens
Identifies blob access authenticated using SAS tokens.
Findings:
- SAS activity originated from a single public IP
- Access counts ranged from 1 to 65 per hour
- Container listing operations confirmed SAS enumeration capability

## Commentary:  
SAS tokens are powerful — any unexpected usage should be treated as a potential incident.

## MITRE ATT&CK:

- Defense Evasion (TA0005)
- Use of Credentials (T1550)

## 6. Detection 4 — Blob Deletions
Detects blob deletions, which may indicate destructive behaviour.
Findings:
- DeleteBlob events captured successfully
- Deleted blobs still generated log entries
- SAS URLs returned 404 after deletion

## Commentary:  
All deletions were controlled and expected — no signs of mass deletion or unauthorised access.

## MITRE ATT&CK:

- Impact (TA0040)
- Data Destruction (T1485)

## 7. Summary
This lab demonstrated:

- StorageBlobLogs ingestion
- AzureActivity ingestion
- Behavioural detections for blob access
- MITRE ATT&CK‑aligned threat detection logic


