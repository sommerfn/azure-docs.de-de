---
title: Azure PowerShell-Skriptbeispiel – Verschlüsseln einer Windows-VM | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Verschlüsseln einer Windows-VM
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ms.openlocfilehash: b97292fb0a832f6eec1096d2274556d162e67ca7
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748646"
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a>Verschlüsseln eines virtuellen Windows-Computers mit Azure PowerShell

Dieses Skript erstellt eine sichere Azure Key Vault-Lösung, Verschlüsselungsschlüssel, ein Azure Active Directory-Dienstprinzipal und einen virtuellen Windows-Computer (Virtual Machine, VM). Die VM wird anschließend mithilfe des Verschlüsselungsschlüssels aus Key Vault und Dienstprinzipal-Anmeldeinformationen verschlüsselt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/encrypt-vm/encrypt-windows-vm.ps1 "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault) | Erstellt Azure Key Vault, um sichere Daten wie Verschlüsselungsschlüssel zu speichern. |
| [Add-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey) | Erstellt einen Verschlüsselungsschlüssel in Key Vault. |
| [New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal) | Erstellt ein Azure Active Directory-Dienstprinzipal für die sichere Authentifizierung und Steuerung des Zugriffs auf die Verschlüsselungsschlüssel. |
| [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) | Legt Berechtigungen für Key Vault fest, um dem Dienstprinzipal Zugriff auf Verschlüsselungsschlüssel zu gewähren. |
| [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der Netzwerksicherheitsgruppe. Mit diesem Befehl werden außerdem Port 80 geöffnet und die Administratoranmeldeinformationen festgelegt. |
| [Get-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/get-azkeyvault) | Ruft die erforderlichen Informationen aus Key Vault ab. |
| [Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) | Aktiviert die Verschlüsselung mithilfe der Dienstprinzipal-Anmeldeinformationen und Verschlüsselungsschlüssel auf einem virtuellen Computer. |
| [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiskencryptionstatus) | Zeigt den Status des Verschlüsselungsprozesses des virtuellen Computers an. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
