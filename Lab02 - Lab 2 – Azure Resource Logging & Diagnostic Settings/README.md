## Overview

The purpose of Lab 2 is to expand the visibility of the SOC environment by enabling resource-level logging within Microsoft Azure. In Lab 1, security telemetry was collected through data connectors such as Microsoft Entra ID and Microsoft Defender XDR. While these connectors provide valuable identity and security signals, many Azure infrastructure services generate logs that are not collected by default. To ingest these logs into Microsoft Sentinel, diagnostic settings must be configured at the resource level.

This lab demonstrates how Azure platform logs are forwarded to the Log Analytics Workspace so they can be analysed and used in detection rules.

## Objectives

Enable diagnostic logging on Azure resources
Send infrastructure logs to the Log Analytics Workspace
Verify ingestion using KQL queries
Expand SOC visibility to include infrastructure activity

## Environment

- Platform: Microsoft Azure
- SIEM: Microsoft Sentinel
- Workspace: LAW-Security-labs
- Region: UK South

## Key Skills Demonstrated

- Azure monitoring configuration
- Security telemetry ingestion
- Log validation using Kusto Query Language
- SOC visibility expansion

