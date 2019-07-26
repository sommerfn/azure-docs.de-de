---
title: Azure PowerShell-Skriptbeispiel – Sichern eines virtuellen Azure-Computers | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Sichern eines virtuellen Azure-Computers
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 03/05/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 84581dd4dfd1cc993476e0e85f804f32d28e8ab4
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68467177"
---
# <a name="back-up-an-encrypted-azure-virtual-machine-with-powershell"></a>Sichern eines verschlüsselten virtuellen Azure-Computers mit PowerShell

Dieses Skript erstellt einen Recovery Services-Tresor mit georedundantem Speicher (Geo-Redundant Storage, GRS) für einen verschlüsselten virtuellen Azure-Computer. Die Standardschutzrichtlinie wird auf den Tresor angewendet. Die Richtlinie generiert täglich eine Sicherung des virtuellen Computers und behält diese für 30 Tage bei. Das Skript löst auch den ersten Wiederherstellungspunkt für den virtuellen Computer aus und behält diesen Wiederherstellungspunkt für 365 Tage bei.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/backup/backup-encrypted-vm/backup-encrypted-vm.ps1 "Back up encrypted virtual machine")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.


| Get-Help | Notizen | 
|---|---| 
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. | 
| [New-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/new-azrecoveryservicesvault) | Erstellt einen Recovery Services-Tresor zum Speichern von Sicherungen | 
| [Set-AzRecoveryServicesBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) | Legt die Sicherungsspeichereigenschaften für den Recovery Services-Tresor fest | 
| [New-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy)| Erstellt eine Schutzrichtlinie mithilfe der Planungsrichtlinie und der Aufbewahrungsrichtlinie im Recovery Services-Tresor. | 
| [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) | Legt Berechtigungen für Key Vault fest, um dem Dienstprinzipal Zugriff auf Verschlüsselungsschlüssel zu gewähren. | 
| [Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection) | Aktiviert die Sicherung für ein Element mit einer bestimmten Azure Backup-Schutzrichtlinie | 
| [Set-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy)| Ändert eine vorhandene Azure Backup-Schutzrichtlinie | 
| [Backup-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem) | Startet eine Sicherung für ein geschütztes Azure Backup-Element, das nicht an den Sicherungszeitplan gebunden ist |
| [Wait-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) | Wartet auf die Fertigstellung eines Azure Backup-Auftrags | 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. | 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

