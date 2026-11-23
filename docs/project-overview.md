# Project Overview

## Objective

The internship project focused on designing, deploying and configuring a cloud-native SIEM solution based on Microsoft Azure Monitor and Microsoft Sentinel.

The implementation aimed to:
- Centralize security logs from Windows and Linux virtual machines, Microsoft Entra ID and Azure Activity.
- Detect suspicious activity through built-in analytics and custom KQL rules.
- Present security activity through Microsoft Sentinel Workbooks.
- Produce technical documentation supporting maintenance and future extension.

## Implemented scope

The documented solution includes a Log Analytics workspace, Microsoft Sentinel, Windows and Linux virtual machines, AMA, DCRs, Entra ID and Azure Activity data sources, analytics rules and Workbooks.

The report explicitly excludes complex on-premises integrations, advanced SOAR automation and 24/7 operational monitoring.

## Main deliverables

- Centralized SIEM workspace with 90-day retention in the test environment.
- Five enabled built-in analytics rules.
- Two custom scheduled KQL analytics rules.
- An identity Workbook showing daily successful/failed sign-ins and geographic sign-in distribution.
- Technical deployment and operational documentation.

## Environment described in the report

| Component | Documented configuration | Purpose |
|---|---|---|
| Windows VM | Windows Server 2019 Datacenter, Standard_B2s (2 vCPU / 4 GB) | `SecurityEvent` source |
| Linux VM | Ubuntu Server 22.04 LTS, Standard_B2s (2 vCPU / 4 GB) | `Syslog` source |
| Log Analytics | Workspace in East US, 90-day retention | Central log store |
| Collection | Azure Monitor Agent + two DCRs | VM telemetry |
| Identity | Microsoft Entra ID connector | `SigninLogs`, `AuditLogs` |
| Cloud activity | Azure Activity Diagnostic Settings | `AzureActivity` |

Environment-specific names and identifiers should be replaced with generic placeholders in any public copy.
