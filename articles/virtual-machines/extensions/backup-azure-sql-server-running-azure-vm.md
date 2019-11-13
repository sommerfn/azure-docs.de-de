---
title: Azure Backup für auf einer Azure-VM ausgeführten SQL Server
description: In diesem Artikel erfahren Sie, wie Sie Azure Backup in einem auf einem virtuellen Azure-Computer ausgeführten SQL Server registrieren.
services: backup
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: dacurwin
ms.openlocfilehash: 77492454e2519c98cadfb6819c850c4830015b59
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748960"
---
# <a name="azure-backup-for-sql-server-running-in-azure-vm"></a>Azure Backup für auf einer Azure-VM ausgeführten SQL Server

Azure Backup bietet unter anderem Unterstützung für die Sicherung von Workloads wie SQL Server, die auf Azure-VMs ausgeführt werden. Da die SQL-Anwendung in einer Azure-VM ausgeführt wird, muss der Sicherungsdienst über die Berechtigung für den Zugriff auf die Anwendung und das Abrufen der erforderlichen Informationen verfügen.
Dazu installiert Azure Backup während des vom Benutzer ausgelösten Registrierungsprozesses die Erweiterung **AzureBackupWindowsWorkload** auf der VM, auf der der SQL Server ausgeführt wird.

## <a name="prerequisites"></a>Voraussetzungen

Eine Liste der unterstützten Szenarien finden Sie in der von Azure Backup unterstützten [Unterstützungsmatrix](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#scenario-support).

## <a name="network-connectivity"></a>Netzwerkverbindung

Azure Backup unterstützt NSG-Tags, die Bereitstellung eines Proxyservers oder aufgelistete IP-Bereiche; Details zu den einzelnen Methoden finden Sie in diesem [Artikel](https://docs.microsoft.com/azure/backup/backup-sql-server-database-azure-vms#establish-network-connectivity).

## <a name="extension-schema"></a>Erweiterungsschema

Die Werte für das Erweiterungsschema und die Eigenschaften sind die Konfigurationswerte (Laufzeiteinstellungen), die der Dienst an die CRP-API übergibt. Diese Konfigurationswerte werden während der Registrierung und beim Upgrade verwendet. Die Erweiterung **AzureBackupWindowsWorkload** verwendet ebenfalls dieses Schema. Das Schema wird vorab festgelegt; ein neuer Parameter kann im Feld „ObjectStr“ hinzugefügt werden.

  ```json
      "runtimeSettings": [{
      "handlerSettings": {
      "protectedSettingsCertThumbprint": "",
      "protectedSettings": {
      "objectStr": "",
      "logsBlobUri": "",
      "statusBlobUri": ""
      }
      },
      "publicSettings": {
      "locale": "en-us",
      "taskId": "1c0ae461-9d3b-418c-a505-bb31dfe2095d",
      "objectStr": "",
      "commandStartTimeUTCTicks": "636295005824665976",
      "vmType": "vmType"
      }
      }]
      }
  ```

Der folgende JSON-Code zeigt das Schema für die Erweiterung „WorkloadBackup“.  

  ```json
  {
    "type": "extensions",
    "name": "WorkloadBackup",
    "location":"<myLocation>",
    "properties": {
      "publisher": "Microsoft.RecoveryServices",
      "type": "AzureBackupWindowsWorkload",
      "typeHandlerVersion": "1.1",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "locale":"<location>",
        "taskId":"<TaskId used by Azure Backup service to communicate with extension>",

        "objectStr": "<The configuration passed by Azure Backup service to extension>",

        "commandStartTimeUTCTicks": "<Scheduled start time of registration or upgrade task>",
        "vmType": "<Type of VM where registration got triggered Eg. Compute or ClassicCompute>"
      },
      "protectedSettings": {
        "objectStr": "<The sensitive configuration passed by Azure Backup service to extension>",
        "logsBlobUri": "<blob uri where logs of command execution by extension are written to>",
        "statusBlobUri": "<blob uri where status of the command executed by extension is written>"
      }
    }
  }
  ```

### <a name="property-values"></a>Eigenschaftswerte

NAME | Wert/Beispiel | Datentyp
 --- | --- | ---
locale | en-us  |  Zeichenfolge
taskId | „1c0ae461-9d3b-418c-a505-bb31dfe2095d“  | Zeichenfolge
objectStr <br/> (publicSettings)  | „eyJjb250YWluZXJQcm9wZXJ0aWVzIjp7IkNvbnRhaW5lcklEIjoiMzVjMjQxYTItOGRjNy00ZGE5LWI4NTMtMjdjYTJhNDZlM2ZkIiwiSWRNZ210Q29udGFpbmVySWQiOjM0NTY3ODg5LCJSZXNvdXJjZUlkIjoiMDU5NWIwOGEtYzI4Zi00ZmFlLWE5ODItOTkwOWMyMGVjNjVhIiwiU3Vic2NyaXB0aW9uSWQiOiJkNGEzOTliNy1iYjAyLTQ2MWMtODdmYS1jNTM5ODI3ZTgzNTQiLCJVbmlxdWVDb250YWluZXJOYW1lIjoiODM4MDZjODUtNTQ4OS00NmNhLWEyZTctNWMzNzNhYjg3OTcyIn0sInN0YW1wTGlzdCI6W3siU2VydmljZU5hbWUiOjUsIlNlcnZpY2VTdGFtcFVybCI6Imh0dHA6XC9cL015V0xGYWJTdmMuY29tIn1dfQ==“ | Zeichenfolge
commandStartTimeUTCTicks | „636967192566036845“  | Zeichenfolge
vmType  | „microsoft.compute/virtualmachines“  | Zeichenfolge
objectStr <br/> (protectedSettings) | „eyJjb250YWluZXJQcm9wZXJ0aWVzIjp7IkNvbnRhaW5lcklEIjoiMzVjMjQxYTItOGRjNy00ZGE5LWI4NTMtMjdjYTJhNDZlM2ZkIiwiSWRNZ210Q29udGFpbmVySWQiOjM0NTY3ODg5LCJSZXNvdXJjZUlkIjoiMDU5NWIwOGEtYzI4Zi00ZmFlLWE5ODItOTkwOWMyMGVjNjVhIiwiU3Vic2NyaXB0aW9uSWQiOiJkNGEzOTliNy1iYjAyLTQ2MWMtODdmYS1jNTM5ODI3ZTgzNTQiLCJVbmlxdWVDb250YWluZXJOYW1lIjoiODM4MDZjODUtNTQ4OS00NmNhLWEyZTctNWMzNzNhYjg3OTcyIn0sInN0YW1wTGlzdCI6W3siU2VydmljZU5hbWUiOjUsIlNlcnZpY2VTdGFtcFVybCI6Imh0dHA6XC9cL015V0xGYWJTdmMuY29tIn1dfQ==“ | Zeichenfolge
logsBlobUri | <https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Logs.txt?sv=2014-02-14&sr=b&sig=DbwYhwfeAC5YJzISgxoKk%2FEWQq2AO1vS1E0rDW%2FlsBw%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw> | Zeichenfolge
statusBlobUri | <https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Status.txt?sv=2014-02-14&sr=b&sig=96RZBpTKCjmV7QFeXm5IduB%2FILktwGbLwbWg6Ih96Ao%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw> | Zeichenfolge

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Wir empfehlen, die Erweiterung AzureBackupWindowsWorkload zu einem virtuellen Computer hinzuzufügen, indem Sie die SQL Server-Sicherung auf dem virtuellen Computer aktivieren. Dafür verwenden Sie die [Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vm-workload-backup),die für die Automatisierung der Sicherung auf einer SQL Server-VM entwickelt wurde.

## <a name="powershell-deployment"></a>PowerShell-Bereitstellung

Sie müssen die Azure-VM, die die SQL-Anwendung enthält, bei einem Recovery Services-Tresor „registrieren“. Während der Registrierung wird die Erweiterung „AzureBackupWindowsWorkload“ auf der VM installiert. Verwenden Sie das Cmdlet  [Register-AzRecoveryServicesBackupContainerPS](https://docs.microsoft.com/powershell/module/az.recoveryservices/Register-AzRecoveryServicesBackupContainer?view=azps-1.5.0), um die VM zu registrieren.

```powershell
$myVM = Get-AzVM -ResourceGroupName <VMRG Name> -Name <VMName>
Register-AzRecoveryServicesBackupContainer -ResourceId $myVM.ID -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetVault.ID -Force
```

Der Befehl gibt einen **Sicherungscontainer** dieser Ressource zurück, und der Status lautet **registriert**.

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr](https://docs.microsoft.com/azure/backup/backup-sql-server-azure-troubleshoot) zu den Richtlinien für die Problembehandlung für die Azure SQL Server-VM-Sicherung.
- [Häufige Fragen](https://docs.microsoft.com/azure/backup/faq-backup-sql-server) zum Sichern von SQL Server-Datenbanken, die auf virtuellen Azure-Computern (VMs) ausgeführt werden, und für die der Dienst Azure Backup genutzt wird.
