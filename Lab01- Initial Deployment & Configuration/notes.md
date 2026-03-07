## Notes

### Microsoft Sentinel is now managed through the Microsoft Defender portal rather than exclusively through the Azure portal.
This reflects Microsoft's transition toward a unified security operations platform.

### Data Connectors and Log Ingestion

Security data ingestion relies on configuring data connectors that send logs to the Log Analytics Workspace.

Identity and security sources such as:

Microsoft Entra ID

Microsoft Defender XDR

Azure Activity

send logs directly to the workspace without requiring resource-level diagnostic settings.

### Diagnostic Settings

Resource-level diagnostic settings were not enabled in this lab because the primary data sources are identity and Defender telemetry. Diagnostic settings are typically required when collecting logs from Azure resources such as: virtual machines, storage accounts, Azure Key Vault and network security devices

These resources send logs to the workspace via diagnostic pipelines.

### Analytics Rule Templates

Installing security solutions from the Content Hub populates analytics rule templates. Templates do not generate alerts until they are converted into active analytics rules.

## RBAC Configuration

Role-based access control was configured to support least-privilege access for SOC analysts.

Roles assigned included:

- Sentinel Contributor

- Log Analytics Reader

- Security Reader

### Elevated Access Warning

During RBAC configuration, Azure displayed a notification indicating that elevated access had been enabled at the tenant level.

This occurs when administrators temporarily enable root-level permissions to manage role assignments. The permission can be disabled once required roles are configured.

## Log Ingestion Validation

Log ingestion was verified using Kusto Query Language queries in the workspace.

Example validation query:

union SigninLogs, AuditLogs, AzureActivity 
| summarize LastEvent=max(TimeGenerated) by Type

This confirmed that multiple data sources were successfully sending logs.