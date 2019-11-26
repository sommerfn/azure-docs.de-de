---
title: Skriptbeispiel von Azure PowerShell – Hinzufügen eines Anwendungszertifikats zu einem Cluster | Microsoft-Dokumentation
description: Skriptbeispiel von Azure PowerShell – Hinzufügen eines Anwendungszertifikats zu einem Cluster
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 04cd13efd198f0a4875c0ede525d10cf45220989
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73901504"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Fügen Sie ein Anwendungszertifikat zu einem Service Fabric-Cluster hinzu

Dieses Beispielskript veranschaulicht Schritt für Schritt, wie Sie ein Zertifikat in Key Vault erstellen und dieses anschließend für eine der VM-Skalierungsgruppen bereitstellen, auf denen Ihr Cluster basiert. In diesem Szenario wird Service Fabric nicht direkt verwendet. Stattdessen werden Key Vault und VM-Skalierungsgruppen verwendet.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Installieren Sie bei Bedarf Azure PowerShell mithilfe der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview), und führen Sie dann `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen. 

## <a name="create-a-certificate-in-key-vault"></a>Erstellen eines Zertifikats in Key Vault

```powershell
$VaultName = ""
$CertName = ""
$SubjectName = "CN="

$policy = New-AzKeyVaultCertificatePolicy -SubjectName $SubjectName -IssuerName Self -ValidityInMonths 12
Add-AzKeyVaultCertificate -VaultName $VaultName -Name $CertName -CertificatePolicy $policy
```

## <a name="update-virtual-machine-scale-sets-profile-with-certificate"></a>Aktualisieren des Profils von VM-Skalierungsgruppen mit dem Zertifikat

```powershell
$ResourceGroupName = ""
$VMSSName = ""
$CertStore = "My" # Update this with the store you want your certificate placed in, this is LocalMachine\My

$CertConfig = New-AzVmssVaultCertificateConfig -CertificateUrl (Get-AzKeyVaultCertificate -VaultName $VaultName -Name $CertName).SecretId -CertificateStore $CertStore
$VMSS = Get-AzVmss -ResourceGroupName $ResourceGroupName -VMScaleSetName $VMSSName

# If this KeyVault is already known by the virtual machine scale set, for example if the cluster certificate is deployed from this keyvault, use
$VMSS.virtualmachineprofile.osProfile.secrets[0].vaultCertificates.Add($certConfig)

# Otherwise use
$VMSS = Add-AzVmssSecret -VirtualMachineScaleSet $VMSS -SourceVaultId (Get-AzKeyVault -VaultName $VaultName).ResourceId  -VaultCertificate $CertConfig
```

## <a name="update-the-virtual-machine-scale-set"></a>Aktualisieren der VM-Skalierungsgruppe
```powershell
Update-AzVmss -ResourceGroupName $ResourceGroupName -VirtualMachineScaleSet $VMSS -VMScaleSetName $VMSSName
```

> Wenn Sie das Zertifikat auf mehreren Knotentypen in Ihrem Cluster platzieren möchten, müssen der zweite und dritte Teil dieses Skripts für jeden Knotentyp wiederholt werden, der über das Zertifikat verfügen soll.

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzKeyVaultCertificatePolicy](/powershell/module/az.keyvault/New-AzKeyVaultCertificatePolicy) | Erstellt eine In-Memory-Richtlinie, die das Zertifikat darstellt. |
| [Add-AzKeyVaultCertificate](/powershell/module/az.keyvault/Add-AzKeyVaultCertificate)| Stellt die Richtlinie für Key Vault bereit. |
| [New-AzVmssVaultCertificateConfig](/powershell/module/az.compute/New-AzVmssVaultCertificateConfig) | Erstellt eine In-Memory-Konfiguration, die das Zertifikat auf einem virtuellen Computer darstellt. |
| [Get-AzVmss](/powershell/module/az.compute/Get-AzVmss) |  |
| [Add-AzVmssSecret](/powershell/module/az.compute/Add-AzVmssSecret) | Fügt das Zertifikat der In-Memory-Definition der VM-Skalierungsgruppe hinzu. |
| [Update-AzVmss](/powershell/module/az.compute/Update-AzVmss) | Stellt die neue Definition der VM-Skalierungsgruppe bereit. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).
