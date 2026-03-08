## Overview

The purpose of Lab 2 is to expand the visibility of the SOC environment by enabling resource-level logging within Microsoft Azure. In Lab 1, security telemetry was collected through data connectors such as Microsoft Entra ID and Microsoft Defender XDR. While these connectors provide valuable identity and security signals, many Azure infrastructure services generate logs that are not automatically forwarded to monitoring systems by default. To ingest these logs into Microsoft Sentinel, diagnostic settings must be configured at the resource level.

This lab demonstrates how Azure platform logs are forwarded to a Log Analytics Workspace where they can be analysed and used for detection and Investigation

## Objectives

- Enable diagnostic logging on Azure resources
- Send infrastructure logs to the Log Analytics Workspace
- Verify ingestion using KQL queries
- Expand SOC visibility to include infrastructure activity

## Environment

- Platform: Microsoft Azure
- SIEM: Microsoft Sentinel
- Workspace: LAW-Security-labs
- Region: UK South

## Log Sources Enabled

The following Azure resource logs were configured and forwarded to the Log Analytics Workspace for analysis within Microsoft Sentinel:

Azure Storage Blob service logs (StorageBlobLogs)

These logs capture operations such as:

- blob uploads
- blob downloads
- blob deletions
- client IP addresses
- operation timestamps

This telemetry allows monitoring for suspicious file access patterns such as abnormal download activity or potential data exfiltration.

Additional Azure resources such as Azure Key Vault, Azure Virtual Machines, and Azure Network Security Groups can also be onboarded using diagnostic settings to expand infrastructure monitoring coverage.

## Key Skills Demonstrated

- Azure monitoring configuration
- Security telemetry ingestion
- Log validation using Kusto Query Language
- SOC visibility expansion

