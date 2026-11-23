# Data Sources

| Source | Collection method | Log Analytics table | Use in this project |
|---|---|---|---|
| Windows Server | AMA + Windows DCR | `SecurityEvent` | Event IDs 4624 and 4625 |
| Ubuntu Linux | AMA + Syslog DCR | `Syslog` | Authentication-related facilities such as `auth` and `authpriv` |
| Microsoft Entra ID | Sentinel data connector | `SigninLogs`, `AuditLogs` | Authentication and directory changes |
| Azure subscription | Diagnostic Settings | `AzureActivity` | Resource operations, including deletion events |

## Windows

- `4624`: successful logon.
- `4625`: failed logon.

The DCR limits collection to these two event IDs in the documented lab.

## Linux

The DCR includes selected Syslog facilities. The report specifically discusses `auth` and `authpriv` as useful for SSH, PAM and sudo-related activity.

## Microsoft Entra ID

- `SigninLogs`: authentication activity, result, application and source IP metadata.
- `AuditLogs`: administrative directory operations such as user lifecycle and role changes.

## Azure Activity

Records operations performed against resources in the Azure subscription. The custom mass-deletion detection uses this source.
