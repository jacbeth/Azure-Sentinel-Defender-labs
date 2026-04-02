## Password Spray

### Attack Method

A password spray attempts one password across multiple accounts.

### Steps

* Navigated to: https://login.microsoftonline.com.
* Attempted signins for lab-user1 and labuser2, using over 5 incorrect passwords, ensuring failures logged.

### Log Analysis in Sentinel

Navigated to Sentinel (via Defender) > Configuration > Analytics Workspace → Logs
Ran a basic password spray KQL

#### KQL Query Used

```SigninLogs
| where ResultType != 0 and UserPrincipalName contains "test-"
| summarize FailedAttempts = count() by UserPrincipalName, IPAddress
| order by FailedAttempts desc```

#### 

Screenshot of query results

!\[passwordspray](./screenshots/1-passwordspray.png)

#### Saved as an analytics rule

