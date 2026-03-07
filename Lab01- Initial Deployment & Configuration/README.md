# Azure Sentinel SOC Lab – Initial Deployment \& Configuration

## Overview

The goal of Lab 1 is to establish a functional SIEM environment capable of ingesting logs, enabling detection logic, and preparing the foundation for threat hunting and incident response.

The Microsoft Defender portal (security.microsoft.com), where Sentinel now resides, was used.



## Environment Architecture

Platform: Microsoft Azure

SIEM: Microsoft Sentinel

Portal: Microsoft Defender (security.microsoft.com)

Log Analytics Workspace : LAW-Security-labs

Region: UK South

Retention: 30 days

## Log Flow

Security Data Sources
(Microsoft Entra ID, Defender XDR, Azure Activity)
↓
Log Analytics Workspace (LAW-Security-labs)
↓
Microsoft Sentinel
↓
Analytics Rules → Incidents → SOC Investigation



## Step 1 – Log Analytics Workspace Creation

Log Analytics Workspace is the central data repository for Sentinel.

LAW supports:

• 	Log ingestion

• 	KQL querying

• 	Analytics rule evaluation

• 	Threat hunting

## Step 2 – Microsoft Sentinel Deployment

Microsoft Sentinel was enabled on the Log Analytics Workspace, creating a SIEM that can accomplish the following: ingest logs, threat detection, threat hunting, incident management, visualisation and automation (SOAR).

## Step 3 – Data Connector Configuration

The following data connectors were successfully connected and verified:

* Azure Activity
* Microsoft Entra ID
* Microsoft Defender for Endpoint
* Microsoft Defender for Identity
* Microsoft Defender XDR
* Microsoft Defender for Cloud Apps
* Microsoft Defender for Office 365

## Verification

* All connectors show as Connected
* Sign‑in logs and audit logs confirmed in Log Analytics
* Azure Activity logs successfully ingested

## Step 4 – Content Hub Installation

Detection content was installed from the Content Hub in the Defender portal, which populated the Analytics Rule Templates section.

Installed solutions include:

* Microsoft Entra ID
* Microsoft Defender for Identity
* Microsoft Defender XDR
* Microsoft 365 security content
* Azure Activity content

### Detection Coverage

146 Analytics Rule Templates available

## Step 5 -Analytical rules configuration

NB: Templates do not generate incidents until they are converted into active rules.

### Rules enabled

* Multiple failed sign in attempts
* Identity-based detections
* Azure role monitoring
* MFA anomaly detection
* Impossible travel detection
* Suspicious sign-in behaviour rules
* Threat intelligence-based rules



(Currently  resource level diagnostic settings disabled as they are mainly used for Azure resource logs (VMs, Key Vault, Storage, etc.).  Log Analytics Workspace is receiving logs from data connectors e.g. Azure Activity which go to Sentinel.)



## Step 6 — Access Control (RBAC)

Access Control (IAM) was configured on the Log Analytics Workspace
to assign least-privilege permissions for  analysts.

Roles assigned:

Sentinel Contributor – manage detection and incident workflows
Log Analytics Reader – query and analyse security logs
Security Reader – view security alerts and posture

## Step 7 — Workspace Health Validation

KQL queries were used to confirm ingestion health:

union SigninLogs, AuditLogs, AzureActivity
| summarize LastEvent=max(TimeGenerated) by Type


This verifies that all connected data sources are actively sending logs.


## Lessons Learned

* Data connectors must be configured before rule templates appear.
* Content Hub installation is required to populate detection logic.
* Microsoft Sentinel has transitioned from Azure Portal to Microsoft Defender portal.



This lab establishes the foundational SIEM infrastructure.
Subsequent labs will focus on detection engineering, incident investigation, threat hunting, and automated response using Microsoft Sentinel.

