---
title: Erstellen und Verschlüsseln eines virtuellen Linux-Computers mit Azure PowerShell
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure PowerShell einen virtuellen Linux-Computer erstellen und verschlüsseln.
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: 14cdd1e15212141e8c631b589c9c470bbdfe38a9
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71829023"
---
# <a name="quickstart-create-and-encrypt-a-linux-vm-in-azure-with-azure-powershell"></a>Schnellstart: Erstellen und Verschlüsseln eines virtuellen Linux-Computers in Azure mit Azure PowerShell

Das Azure PowerShell-Modul dient zum Erstellen und Verwalten von Azure-Ressourcen über die PowerShell-Befehlszeile oder mit Skripts. In dieser Schnellstartanleitung erfahren Sie, wie Sie mit dem Azure PowerShell-Modul einen virtuellen Linux-Computer (VM) und einen Schlüsseltresor zum Speichern von Verschlüsselungsschlüsseln erstellen und den virtuellen Computer verschlüsseln. In dieser Schnellstartanleitung werden das Ubuntu 16.04 LTS-Marketplace-Image von Canonical und eine VM der Größe Standard_D2S_V3 verwendet. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden:

```azurepowershell-interactive
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie eine Azure-VM mit [New-AzVM](/powershell/module/az.compute/new-azvm), und übergeben Sie sie dem VM-Konfigurationsobjekt, das Sie oben erstellt haben.

```powershell-interactive
$cred = = Get-Credential

New-AzVM -Name MyVm -Credential $cred -ResourceGroupName MyResourceGroup -Image Canonical:UbuntuServer:16.04-LTS:latest -Size Standard_D2S_V3
```

Die Bereitstellung des virtuellen Computers dauert ein paar Minuten. 

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Erstellen eines für Verschlüsselungsschlüssel konfigurierten Schlüsseltresors

Bei der Azure-Datenträgerverschlüsselung werden die Verschlüsselungsschlüssel in Azure Key Vault gespeichert. Erstellen Sie mit [New-AzKeyvault](/powershell/module/az.keyvault/new-azkeyvault) einen Schlüsseltresor. Um dem Schlüsseltresor die Speicherung von Verschlüsselungsschlüsseln zu ermöglichen, verwenden Sie den Parameter „-EnabledForDiskEncryption“.

> [!Important]
> Jeder Schlüsseltresor muss einen Namen aufweisen, der in Azure eindeutig ist. Ersetzen Sie in den Beispielen unten <your-unique-keyvault-name> durch einen von Ihnen gewählten Namen.

```azurepowershell-interactive
New-AzKeyvault -name "<your-unique-keyvault-name>" -ResourceGroupName "myResourceGroup" -Location EastUS -EnabledForDiskEncryption
```

## <a name="encrypt-the-virtual-machine"></a>Verschlüsseln des virtuellen Computers

Verschlüsseln Sie Ihre VM mit [Set-AzVmDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension). 

Set-AzVmDiskEncryptionExtension benötigt einige Werte aus Ihrem Schlüsseltresorobjekt. Sie können diese Werte erhalten, indem Sie den eindeutigen Namen Ihres Schlüsseltresors an [Get-AzKeyvault](/powershell/module/az.keyvault/get-azkeyvault) übergeben.

```azurepowershell-interactive
$KeyVault = Get-AzKeyVault -VaultName "<your-unique-keyvault-name>" -ResourceGroupName "MyResourceGroup"

Set-AzVMDiskEncryptionExtension -ResourceGroupName MyResourceGroup -VMName "MyVM" -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId -SkipVmBackup -VolumeType All
```

Nach einigen Minuten gibt der Prozess Folgendes zurück:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK
```

Sie können den Verschlüsselungsvorgang durch Ausführung von [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/Get-AzVMDiskEncryptionStatus) überprüfen.

```azurepowershell-interactive
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

```azurepowershell-interactive
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen virtuellen Computer und einen Schlüsseltresor erstellt, der für Verschlüsselungsschlüssel aktiviert wurde, und die VM verschlüsselt.  Fahren Sie mit dem nächsten Artikel fort, um weitere Informationen zu Azure Disk Encryption für virtuelle Linux-Computer zu erhalten.

> [!div class="nextstepaction"]
> [Übersicht über Azure Disk Encryption](disk-encryption-overview.md)