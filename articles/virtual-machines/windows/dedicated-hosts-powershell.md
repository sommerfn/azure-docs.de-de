---
title: Bereitstellen von dedizierten Azure-Hosts über Azure PowerShell | Microsoft-Dokumentation
description: Stellen Sie VMs auf dedizierten Hosts über Azure PowerShell bereit.
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/01/2019
ms.author: cynthn
ms.openlocfilehash: 2482fb4ab74a3c1e032a32890c3dc2c3920b5e6b
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68725628"
---
# <a name="preview-deploy-vms-to-dedicated-hosts-using-the-azure-powershell"></a>Vorschau: Bereitstellen von VMs auf dedizierten Hosts über Azure PowerShell

Dieser Artikel führt Sie durch die Erstellung eines [dedizierten Azure-Hosts](dedicated-hosts.md) zum Hosten Ihrer virtuellen Computer (VMs). 

Vergewissern Sie sich, dass die Azure PowerShell-Version 2.4.2 oder höher installiert ist und Sie mithilfe von `Connect-AzAccount` bei einem Azure-Konto angemeldet wurden. Öffnen Sie zum Installieren von Version 2.4.2 eine PowerShell-Eingabeaufforderung, und geben Sie Folgendes ein:

```powershell
Install-Module -Name Az.Compute -Repository PSGallery -RequiredVersion 2.4.2-preview -AllowPrelease
```

Sie benötigen mindestens Version 1.6.0 des PowerShellGet-Moduls, um die Funktionalität des Vorschaumoduls in PowerShell zu aktivieren. In den neuesten Versionen von PowerShell Core ist diese automatisch integriert. Für ältere Versionen von PowerShell können Sie den folgenden Befehl ausführen, um ein Update auf die neueste Version durchzuführen:

```powershell
Install-Module -Name PowerShellGet -Repository PSGallery -Force
```


> [!IMPORTANT]
> Azure Dedicated Host – der Dienst für dedizierte Azure-Hosts – ist derzeit als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> **Bekannte Einschränkungen der Vorschau**
> - VM-Skalierungsgruppen werden zurzeit auf dedizierten Hosts nicht unterstützt.
> - Das erste Vorschaurelease unterstützt die folgende VM-Serien: DSv3 und ESv3. 



## <a name="create-a-host-group"></a>Erstellen einer Hostgruppe

Eine **Hostgruppe** ist eine Ressource, die eine Sammlung dedizierter Hosts darstellt. Sie erstellen eine Hostgruppe in einer Region und einer Verfügbarkeitszone und fügen ihr Hosts hinzu. Bei der Planung für Hochverfügbarkeit stehen zusätzliche Optionen zur Verfügung. Sie können eine oder beide der folgenden Optionen mit Ihren dedizierten Hosts verwenden: 
- Ausdehnen über mehrere Verfügbarkeitszonen. In diesem Fall muss in jeder Zone, die Sie verwenden möchten, eine Hostgruppe vorhanden sein.
- Ausdehnen über mehrere Fehlerdomänen, die physischen Racks zugeordnet sind. 
 
In beiden Fällen müssen Sie die Anzahl der Fehlerdomänen für Ihre Hostgruppe angeben. Wenn Ihre Gruppe keine Fehlerdomänen umfassen soll, verwenden Sie die Anzahl 1 von Fehlerdomänen. 

Sie können auch sowohl Verfügbarkeitszonen als auch Fehlerdomänen verwenden. In diesem Beispiel wird eine Hostgruppe in Zone 1 mit zwei Fehlerdomänen erstellt. 


```powershell
$rgName = "myDHResourceGroup"
$location = "East US"

New-AzResourceGroup -Location $location -Name $rgName
$hostGroup = New-AzHostGroup `
   -Location $location `
   -Name myHostGroup `
   -PlatformFaultDomain 2 `
   -ResourceGroupName $rgName `
   -Zone 1
```

## <a name="create-a-host"></a>Erstellen eines Hosts

Nun erstellen Sie einen dedizierten Host in der Hostgruppe. Zusätzlich zu einem Namen müssen Sie die SKU für den Host angeben. Die Host-SKU erfasst die unterstützte VM-Serie sowie die Hardwaregenerierung für Ihren dedizierten Host.  Während der Vorschau werden die folgenden Host-SKU-Werte unterstützt: „DSv3_Type1“ und „ESv3_Type1“.


Weitere Informationen zu den Host-SKUs und Preisen finden Sie unter [Azure Dedicated Host – Preise](https://aka.ms/ADHPricing).

Beim Festlegen der Anzahl der Fehlerdomänen für die Hostgruppe werden Sie aufgefordert, die Fehlerdomäne für den Host anzugeben. In diesem Beispiel wird die Fehlerdomäne für den Host auf „1“ festgelegt.


```powershell
$dHost = New-AzHost `
   -HostGroupName $hostGroup.Name `
   -Location $location -Name myHost `
   -ResourceGroupName $rgName `
   -Sku DSv3-Type1 `
   -AutoReplaceOnFailure 1 `
   -PlatformFaultDomain 1
```

## <a name="create-a-vm"></a>Erstellen einer VM

Erstellen Sie einen virtuellen Computer auf dem dedizierten Host. 

Wenn Sie beim Erstellen der Hostgruppe eine Verfügbarkeitszone angegeben haben, müssen Sie beim Erstellen des virtuellen Computers dieselbe Zone verwenden. Da sich in diesem Beispiel die Hostgruppe in Zone 1 befindet, müssen wir die VM in Zone 1 erstellen.  


```powershell
$cred = Get-Credential
New-AzVM `
   -Credential $cred `
   -ResourceGroupName $rgName `
   -Location $location `
   -Name myVM `
   -HostId $dhost.Id `
   -Image Win2016Datacenter `
   -Zone 1 `
   -Size Standard_D4s_v3
```

> [!WARNING]
> Wenn Sie einen virtuellen Computer auf einem Host erstellen, der nicht über ausreichende Ressourcen verfügt, wird der virtuelle Computer im Zustand „Fehler“ erstellt. 

## <a name="check-the-status-of-the-host"></a>Überprüfen des Status des Hosts

Sie können mit [GetAzHost](/powershell/module/az.compute/get-azhost) mit dem Parameter `-InstanceView` den Integritätsstatus des Hosts sowie die Anzahl von virtuellen Computern überprüfen, die Sie noch auf dem Host bereitstellen können.

```
Get-AzHost `
   -ResourceGroupName $rgName `
   -Name myHost `
   -HostGroupName $hostGroup.Name `
   -InstanceView
```

Die Ausgabe sieht etwa wie folgt aus:

```
ResourceGroupName      : myDHResourceGroup
PlatformFaultDomain    : 1
AutoReplaceOnFailure   : True
HostId                 : 12345678-1234-1234-abcd-abc123456789
ProvisioningTime       : 7/28/2019 5:31:01 PM
ProvisioningState      : Succeeded
InstanceView           : 
  AssetId              : abc45678-abcd-1234-abcd-123456789abc
  AvailableCapacity    : 
    AllocatableVMs[0]  : 
      VmSize           : Standard_D2s_v3
      Count            : 32
    AllocatableVMs[1]  : 
      VmSize           : Standard_D4s_v3
      Count            : 16
    AllocatableVMs[2]  : 
      VmSize           : Standard_D8s_v3
      Count            : 8
    AllocatableVMs[3]  : 
      VmSize           : Standard_D16s_v3
      Count            : 4
    AllocatableVMs[4]  : 
      VmSize           : Standard_D32-8s_v3
      Count            : 2
    AllocatableVMs[5]  : 
      VmSize           : Standard_D32-16s_v3
      Count            : 2
    AllocatableVMs[6]  : 
      VmSize           : Standard_D32s_v3
      Count            : 2
    AllocatableVMs[7]  : 
      VmSize           : Standard_D64-16s_v3
      Count            : 1
    AllocatableVMs[8]  : 
      VmSize           : Standard_D64-32s_v3
      Count            : 1
    AllocatableVMs[9]  : 
      VmSize           : Standard_D64s_v3
      Count            : 1
  Statuses[0]          : 
    Code               : ProvisioningState/succeeded
    Level              : Info
    DisplayStatus      : Provisioning succeeded
    Time               : 7/28/2019 5:31:01 PM
  Statuses[1]          : 
    Code               : HealthState/available
    Level              : Info
    DisplayStatus      : Host available
Sku                    : 
  Name                 : DSv3-Type1
Id                     : /subscriptions/10101010-1010-1010-1010-101010101010/re
sourceGroups/myDHResourceGroup/providers/Microsoft.Compute/hostGroups/myHostGroup/hosts
/myHost
Name                   : myHost
Location               : eastus
Tags                   : {}
```

## <a name="clean-up"></a>Bereinigen

Ihre dedizierten Hosts werden Ihnen auch dann in Rechnung gestellt, wenn keine virtuellen Computer bereitgestellt sind. Sie sollten, um Kosten zu sparen, alle Hosts löschen, die Sie zurzeit nicht verwenden.  

Ein Host kann nur gelöscht werden, wenn er nicht mehr von virtuellen Computern verwendet wird. Löschen Sie die VMs mit [Remove-AzVM](/powershell/module/az.compute/remove-azvm).

```powershell
Remove-AzVM -ResourceGroupName $rgName -Name myVM
```

Nachdem Sie die VMs gelöscht haben, können Sie den Host mithilfe von [Remove-AzHost](/powershell/module/az.compute/remove-azhost) löschen.

```powershell
Remove-AzHost -ResourceGroupName $rgName -Name myHost
```

Wenn Sie alle Hosts gelöscht haben, können Sie die Hostgruppe mit [Remove-AzHostGroup](/powershell/module/az.compute/remove-azhostgroup) löschen. 

```powershell
Remove-AzHost -ResourceGroupName $rgName -Name myHost
```

Sie können mithilfe von [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) auch die gesamte Ressourcengruppe mit einem einzigen Befehl löschen. Dabei werden alle in der Gruppe erstellten Ressourcen gelöscht, einschließlich aller VMs, Hosts und Hostgruppen.
 
```azurepowershell-interactive
Remove-AzResourceGroup -Name $rgName
```


## <a name="next-steps"></a>Nächste Schritte

- [Hier](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) finden Sie eine Beispielvorlage, die sowohl Zonen als auch Fehlerdomänen für maximale Resilienz in einer Region verwendet.

- Sie können dedizierte Hosts auch über das [Azure-Portal](dedicated-hosts-portal.md) bereitstellen.
