## Sentinel SIEM Deployment & Azure Logging 

### 1. Log Analytics Workspace Creation
Data connector set up to send logs directly to the Log Analytics Workspace (LAW).

![law](./screenshots/1-log-analytics-workspace.png)
#### Screenshot showing Log Analytics Workspace

### 2. Microsoft Sentinel Deployment

#### Screenshot - Sentinel enabled on LAW
![sentinel](./screenshots/2-Microsoft-Sentinel-enabled.png)

### 3. Data Connector Configuration

#### Screenshot - data connectors configured
![data_connectors](./screenshots/3-Data_connectors.png)

#### Screenshot - Sign‑in and audit logs in LAW 
![signin](./screenshots/4-log-verification-kql.png)
 
### 4. Content Hub Installation
Detection content was installed from the Content Hub, populating the Analytics Rule Templates section.

#### Screenshot - Content Hub Installed
![contenthub](./screenshots/5-content-hub-installed.png)

#### Screenshot -  146 analytics rule templates available
![rulesavailable](./screenshots/6-analytics-rule-templates.png)

### 5. Analytics Rules Configuration
Templates do **not** generate incidents until converted into **active rules**.

#### Screenshot -  Rules Configuration
![rulesconfig](./screenshots/7-Analytic_rule_creation.png)

### 6. Access Control (RBAC)
Role‑based access control was configured to support least privilege principle.

### Roles Assigned: 
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

#### Screenshot -  Assigned roles
![rbac](./screenshots/9-rbac.png)

### 7.Log Ingestion Validation
KQL query used to validate ingestion:

```kql
union SigninLogs, AuditLogs, AzureActivity
| summarize LastEvent = max(TimeGenerated) by Type
```
#### Screenshot -  Log Ingestion Validation
![logingestion](./screenshots/8-kql-results.png)
