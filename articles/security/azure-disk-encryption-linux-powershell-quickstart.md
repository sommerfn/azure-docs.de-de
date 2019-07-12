---
title: Erstellen und Verschlüsseln eines virtuellen Linux-Computers mit Azure PowerShell
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure PowerShell einen virtuellen Linux-Computer erstellen und verschlüsseln.
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: 123d3e6ad0312a76540b68a28abf13008d419ca7
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331414"
---
# <a name="quickstart-create-and-encrypt-a-linux-virtual-machine-in-azure-with-powershell"></a>Schnellstart: Erstellen und Verschlüsseln einer Linux-VM mit PowerShell in Azure

Das Azure PowerShell-Modul dient zum Erstellen und Verwalten von Azure-Ressourcen über die PowerShell-Befehlszeile oder mit Skripts. In dieser Schnellstartanleitung erfahren Sie, wie Sie mit dem Azure PowerShell-Modul einen virtuellen Linux-Computer (VM) und einen Schlüsseltresor zum Speichern von Verschlüsselungsschlüsseln erstellen und den virtuellen Computer verschlüsseln. In dieser Schnellstartanleitung werden das Ubuntu 16.04 LTS-Marketplace-Image von Canonical und eine VM der Größe Standard_D2S_V3 verwendet. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden:

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie eine Azure-VM mit [New-AzVM](/powershell/module/az.compute/new-azvm), und übergeben Sie sie dem VM-Konfigurationsobjekt, das Sie oben erstellt haben.

```powershell
$securePassword = ConvertTo-SecureString 'AZUREuserPA$$W0RD' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

New-AzVM -Name MyVm -Credential $cred -ResourceGroupName MyResourceGroup -Image Canonical:UbuntuServer:16.04-LTS:latest -Size Standard_D2S_V3
```

Die Bereitstellung des virtuellen Computers dauert ein paar Minuten. 

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Erstellen eines für Verschlüsselungsschlüssel konfigurierten Schlüsseltresors

Bei der Azure-Datenträgerverschlüsselung werden die Verschlüsselungsschlüssel in Azure Key Vault gespeichert. Erstellen Sie mit [New-AzKeyvault](/powershell/module/az.keyvault/new-azkeyvault) einen Schlüsseltresor. Um dem Schlüsseltresor die Speicherung von Verschlüsselungsschlüsseln zu ermöglichen, verwenden Sie den Parameter „-EnabledForDiskEncryption“.

> [!Important]
> Jeder Schlüsseltresor muss einen eindeutigen Namen haben. Das folgende Beispiel erstellt einen Schlüsseltresor mit dem Namen *myKV*. Ihren eigenen müssen Sie jedoch mit einem anderen Namen versehen.

```powershell
New-AzKeyvault -name MyKV -ResourceGroupName myResourceGroup -Location EastUS -EnabledForDiskEncryption
```

## <a name="encrypt-the-virtual-machine"></a>Verschlüsseln des virtuellen Computers

Verschlüsseln Sie Ihre VM mit [Set-AzVmDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension). 

Set-AzVmDiskEncryptionExtension benötigt einige Werte aus Ihrem Schlüsseltresorobjekt. Sie können diese Werte erhalten, indem Sie den eindeutigen Namen Ihres Schlüsseltresors an [Get-AzKeyvault](/powershell/module/az.keyvault/get-azkeyvault) übergeben.

```powershell
$KeyVault = Get-AzKeyVault -VaultName MyKV -ResourceGroupName MyResourceGroup

Set-AzVMDiskEncryptionExtension -ResourceGroupName MyResourceGroup -VMName MyVM -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId -SkipVmBackup -VolumeType All
```

Nach einigen Minuten gibt der Prozess Folgendes zurück:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK
```

Sie können den Verschlüsselungsvorgang durch Ausführung von [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/Get-AzVMDiskEncryptionStatus) überprüfen.

```powershell
Get-AzVmDiskEncryptionStatus -VMName MyVM -ResourceGroupName MyResourceGroup
```

Wenn die Verschlüsselung aktiviert ist, wird in der zurückgegebenen Ausgabe Folgendes angezeigt:

```
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : NotMounted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe, die VM und alle dazugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Cmdlet [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) entfernen:

```powershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen virtuellen Computer und einen Schlüsseltresor erstellt, der für Verschlüsselungsschlüssel aktiviert wurde, und die VM verschlüsselt.  Fahren Sie mit dem nächsten Artikel fort, um weitere Informationen zu den Voraussetzungen für Azure Disk Encryption und virtuelle IaaS-Computer zu erhalten.

> [!div class="nextstepaction"]
> [Azure Disk Encryption – Voraussetzungen](azure-security-disk-encryption-prerequisites.md)