# Installation Guide

This guide summarizes the sequence documented in the internship report. Azure portal labels can change over time; verify the current portal experience and organization policies before deployment.

> Use a lab subscription and least-privilege access. Do not reuse production credentials or publish tenant-specific identifiers.

## 1. Create a Log Analytics workspace

1. In the Azure portal, open **Log Analytics workspaces**.
2. Select **Create**.
3. Choose the subscription, resource group, region and workspace name.
4. Configure the required retention period. The documented lab used 90 days.
5. Review and create the workspace.

## 2. Enable Microsoft Sentinel

1. Open **Microsoft Sentinel** in the Azure portal.
2. Select **Add**.
3. Choose the Log Analytics workspace created in step 1.
4. Add Sentinel to that workspace.

## 3. Provision the virtual machines

The documented lab used:
- Windows Server 2019 Datacenter VM.
- Ubuntu Server 22.04 LTS VM.

The report lists Standard_B2s (2 vCPU / 4 GB) for both. Choose VM sizing according to your own lab requirements and budget.

## 4. Install Azure Monitor Agent

Associate each VM with its corresponding Data Collection Rule. The report describes the AMA extensions:
- `AzureMonitorWindowsAgent` for Windows.
- `AzureMonitorLinuxAgent` for Linux.

Verify the extension provisioning status in **Extensions + applications**. The expected successful state is `Provisioning succeeded`.

## 5. Configure Data Collection Rules

Create separate DCRs for Windows and Linux.

### Windows DCR

Collect Windows Security Events via AMA and filter for:
- Event ID `4624`: successful logon.
- Event ID `4625`: failed logon.

The report's event-selection expression is:

```text
Security!*[System[(EventID=4624 or EventID=4625)]]
```

### Linux DCR

Configure Syslog collection via AMA. The report lists the facilities:
`auth`, `authpriv`, `cron`, `daemon`, `kern`, `syslog` and `user`, with `LOG_INFO` severity.

Verify ingestion in the `SecurityEvent` and `Syslog` tables.

## 6. Connect Microsoft Entra ID

1. In Microsoft Sentinel, open **Data connectors**.
2. Select **Microsoft Entra ID**.
3. Enable **Sign-In Logs** and **Audit Logs**.
4. Apply the changes.

## 7. Connect Azure Activity

The report configures Azure Activity through subscription Diagnostic Settings:

1. Open **Subscriptions** and select the target subscription.
2. Open **Activity log** → **Diagnostic settings**.
3. Add a diagnostic setting.
4. Select the categories documented in the report: Administrative, Security, ServiceHealth and Alert.
5. Send the selected data to the Log Analytics workspace.
6. Save.

## 8. Validate ingestion

Run the sample queries in [`../kql/validation-queries.kql`](../kql/validation-queries.kql). Confirm that the expected tables contain records before enabling detections.

## 9. Configure detections and Workbooks

- Enable the selected built-in analytics rules appropriate to the environment.
- Create the two custom scheduled analytics rules from the files in `../kql/`.
- Create an identity Workbook using the sign-in trend and geographic queries.

## Validation checklist

- [ ] Workspace exists and retention is configured.
- [ ] Sentinel is connected to the workspace.
- [ ] AMA is provisioned on both VMs.
- [ ] Windows and Linux DCRs are associated with the correct VMs.
- [ ] `SecurityEvent` and `Syslog` data are arriving.
- [ ] Entra ID Sign-In Logs and Audit Logs are arriving.
- [ ] Azure Activity records are arriving.
- [ ] KQL validation queries return expected records.
- [ ] Analytics rules and Workbook queries have been tested.
