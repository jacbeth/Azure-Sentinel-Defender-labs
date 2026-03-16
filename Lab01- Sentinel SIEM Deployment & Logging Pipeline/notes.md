## Sentinel SIEM Deployment & Azure Logging Pipeline

### 🔧 Log Analytics Workspace Creation
Sentinel ingests security telemetry through data connectors that send logs directly to the Log Analytics Workspace (LAW).

#### Screenshot showing Log Analytics Workspace
![law](./screenshots/1-log-analytics-workspace.png)

---

### 🛡️ Microsoft Sentinel Deployment

#### Screenshot - Sentinel enabled on LAW
![sentinel](./screenshots/2-Microsoft-Sentinel-enabled.png)

---

### 🔌 Data Connector Configuration

#### Screenshot - data connectors configured
![data_connectors](./screenshots/3-Data_connectors.png)

#### Screenshot - Sign‑in and audit logs in LAW 
![signin](./screenshots/4-log-verification-kql.png)
 
---

## 📦 Content Hub Installation
Detection content was installed from the Content Hub, populating the **Analytics Rule Templates** section.
#### Screenshot - Content Hub Installed
![contenthub](./screenshots/5-content-hub-installed.png)

#### Screenshot -  146 analytics rule templates available
![rulesavailable](./screenshots/6-analytics-rule-templates.png)

---

### ⚠️ Analytics Rules Configuration
Templates do **not** generate incidents until converted into **active rules**.


---

### 🔐 Access Control (RBAC)
Least‑privilege access was configured on the workspace.

Roles assigned:
- **Sentinel Contributor**  
- **Log Analytics Reader**  
- **Security Reader**  

#### Screenshot -  Assigned roles
![rbac](./screenshots/9-rbac.png)

---

## 📊 Log Ingestion Validation
KQL query used to validate ingestion:

```kql
union SigninLogs, AuditLogs, AzureActivity
| summarize LastEvent = max(TimeGenerated) by Type
```
#### Screenshot -  Log Ingestion Validation
![logingestion](./screenshots/8-kql-results.png)
