## Lab 2 - Notes

### Purpose of Diagnostic Settings

Many Azure services do not automatically send logs to Microsoft Sentinel. Diagnostic settings act as the pipeline that forwards resource logs into the monitoring environment. Without diagnostic settings enabled, infrastructure activity may not be visible to security analysts.

### Types of Logs Collected

Diagnostic settings collect platform logs, which include:

* Administrative operations
* Policy changes
* Resource configuration modifications
* Security-related events


These logs help identify suspicious administrative activity or misconfigurations.

NB $logs is reserved for system log files

### SOC Visibility Improvement

After enabling diagnostic settings, the SIEM environment now ingests:

* Identity logs
* Azure Activity logs
* Infrastructure resource logs

This significantly improves the detection capabilities of the SOC environment.

## Example Threat Hunting Query

The following Kusto Query Language query was used to detect potential abnormal download activity from Azure Storage.
StorageBlobLogs
| where OperationName == "GetBlob"
| summarize DownloadCount = count() by CallerIpAddress, bin(TimeGenerated, 15m)
| where DownloadCount > 5
| sort by DownloadCount desc

This query identifies possible data exfiltration attempts by detecting large numbers of file downloads from a single IP address.

### Operational Considerations

Diagnostic logging increases data ingestion into the Log Analytics Workspace, which can affect cost and retention settings in production environments. For lab environments, log volume is usually minimal.

### Result

The SOC environment now provides monitoring  across both identity services and Azure infrastructure resources. This allows Microsoft Sentinel to detect potential threats affecting cloud resources as well as user identities.

