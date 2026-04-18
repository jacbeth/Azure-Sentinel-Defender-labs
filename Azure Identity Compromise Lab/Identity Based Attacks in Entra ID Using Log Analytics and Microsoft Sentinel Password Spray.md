### Password Spray Attack
This lab simulates suspicious authentication behaviour, collects identity logs, analyses them using KQL, and builds detections in Microsoft Sentinel.
#### Lab Architecture
Azure Components
•	Entra ID tenant
•	Test users
•	Conditional Access policies
•	Sign in logs and Audit logs
•	Log Analytics Workspace (LAW)
•	Microsoft Sentinel
•	Workbook and Analytics Rules
#### Local Components
•	Browser (Chrome)
•	VPN for generating varied IP sign ins  
#### Lab Setup
- Created Test Users: test-user1, test-user2 and labadmin(Global Administrator)
- Configured Diagnostic Settings and enabled forwarding to LAW for: SignInLogs, AuditLogs, NonInteractiveUserSignInLogs and ServicePrincipalSignInLogs
- Enabled Microsoft Sentinel: Attached Sentinel to the LAW and enabled the following data connectors:Azure Active Directory, Azure Active Directory Sign in Logs and Azure Active Directory Audit Logs
#### 1 —  Created Authentication Failure Logs 
- Attempted a few incorrect logins from different user accounts and the same IP address and used a VPN for foreign IP
#### 2 — Generated Logs in Entra ID
- Signinlogs, Result code: 50126 (Invalid username or password), Multiple users, IP address, Location
#### 3 — Sent Logs to Log Analytics
Ensured diagnostic settings are enabled: AuditLogs, SignInLogs and NonInteractiveUserSignInLogs
#### 4 — Reviewed Signinlogs in Entra ID
Searched for: Status: Failure, Code: 50126 (Invalid username or password), Multiple users, Same IP address and Short timeframe
#### - Built KQL Detection Queries
##### Basic failed sign ins
```
SigninLogs 
| where ResultType == "50126" 
| project TimeGenerated, UserPrincipalName, IPAddress, Location 
```
##### Password spray 
```
SigninLogs 
| where ResultType == "50126" 
| summarize FailedAttempts = count(), Users = make_set(UserPrincipalName) by IPAddress 
| where FailedAttempts > 5 and array_length(Users) > 2 
```
##### Timeline
```
SigninLogs 
| where ResultType == "50126" 
| summarize count() by bin(TimeGenerated, 5m), IPAddress 
```
