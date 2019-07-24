---
title: Markieren einer Windows-VM-Ressource in Azure | Microsoft-Dokumentation
description: Informieren Sie sich über das Markieren eines virtuellen Windows-Computers, der in Azure mithilfe des Resource Manager-Bereitstellungsmodells erstellt wurde.
services: virtual-machines-windows
documentationcenter: ''
author: mmccrory
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: 56d17f45-e4a7-4d84-8022-b40334ae49d2
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
ms.openlocfilehash: 26ee777f7db05ca1850e2a01c1716810624906c0
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709838"
---
# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>So markieren Sie einen virtuellen Windows-Computer in Azure
In diesem Artikel werden verschiedene Methoden zum Markieren eines virtuellen Windows-Computers in Azure mithilfe des Resource Manager-Bereitstellungsmodells beschrieben. Tags sind benutzerdefinierte Schlüssel-Wert-Paare, die direkt auf einer Ressource oder einer Ressourcengruppe platziert werden können. Azure unterstützt derzeit bis zu 15 Tags pro Ressource und Ressourcengruppe. Tags können zum Zeitpunkt der Erstellung auf einer Ressource platziert werden oder zu einer vorhandenen Ressource hinzugefügt werden. Beachten Sie, dass Tags nur für Ressourcen unterstützt werden, die über das Resource Manager-Bereitstellungsmodell erstellt wurden. Wenn Sie einen virtuellen Linux-Computer markieren möchten, finden Sie weitere Informationen unter [How to tag a Linux virtual machine in Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)(So markieren Sie einen virtuellen Linux-Computer in Azure).

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Markieren mit PowerShell
Zum Erstellen, Hinzufügen und Löschen von Tags über PowerShell müssen Sie zunächst Ihre [PowerShell-Umgebung mit dem Azure-Ressourcen-Manager][PowerShell environment with Azure Resource Manager]einrichten. Sobald Sie die Einrichtung abgeschlossen haben, können Sie die Tags auf Compute-, Netzwerk- und Speicherressourcen platzieren, entweder während der Erstellung oder nachdem die Ressource mithilfe von PowerShell erstellt wurde. Dieser Artikel konzentriert sich auf das Anzeigen und Bearbeiten von Tags, die auf virtuellen Computern platziert sind.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

Navigieren Sie zuerst über das `Get-AzVM` -Cmdlet zu einem virtuellen Computer.

        PS C:\> Get-AzVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Wenn der virtuelle Computer bereits Tags enthält, werden auf der Ressource anschließend alle Tags angezeigt:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Wenn Sie Tags über PowerShell hinzufügen möchten, können Sie den Befehl `Set-AzResource` verwenden. Beachten Sie, dass beim Aktualisieren von Tags über PowerShell die Tags als Ganzes aktualisiert werden. Wenn Sie ein Tag auf eine Ressource hinzufügen, die bereits über Tags verfügt, müssen Sie alle Tags einschließen, die auf der Ressource platziert werden sollen. Es folgt ein Beispiel für das Hinzufügen zusätzlicher Tags zu einer Ressource über PowerShell-Cmdlets.

Das erste Cmdlet legt alle Tags, die auf *MyTestVM* platziert sind, mithilfe der Eigenschaften `Get-AzResource` und `Tags` auf die *$tags*-Variable fest.

        PS C:\> $tags = (Get-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

Der zweite Befehl zeigt die Tags für die angegebene Variable an.

```
    PS C:\> $tags
    
    Key           Value
    ----          -----
    Department    MyDepartment
    Application   MyApp1
    Created By    MyName
    Environment   Production
```

Der dritte Befehl fügt ein zusätzliches Tag für die *$tags* -Variable hinzu. Beachten Sie, wie **+=** verwendet wird, um das neue Schlüssel-Wert-Paar an die *$tags* -Liste anzufügen.

        PS C:\> $tags += @{Location="MyLocation"}

Der vierte Befehl legt alle Tags, die gemäß der *$tags* -Variablen definiert sind, auf die angegebene Ressource fest. In diesem Fall ist dies „MyTestVM“.

        PS C:\> Set-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Der fünfte Befehl zeigt alle Tags auf der Ressource an. Wie Sie sehen können, wird der *Speicherort* jetzt als Tag mit *MeinStandort* als Wert definiert.

```
    PS C:\> (Get-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

    Key           Value
    ----          -----
    Department    MyDepartment
    Application   MyApp1
    Created By    MyName
    Environment   Production
    Location      MyLocation
```

Weitere Informationen zum Festlegen von Tags über PowerShell finden Sie in den [Azure Resource-Cmdlets][Azure Resource Cmdlets].

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Taggen Ihrer Azure-Ressourcen finden Sie in der [Übersicht über Azure Resource Manager][Azure Resource Manager Overview] sowie unter „Verwenden von Tags zum Organisieren von Azure-Ressourcen“.and [Using Tags to organize your Azure Resources][Using Tags to organize your Azure Resources]
* Unter [Grundlegendes zu Ihrer Microsoft Azure-Rechnung][Understanding your Azure Bill] und „Verwenden der Azure-Abrechnungs-APIs, um programmgesteuerte Einblicke in die Nutzung Ihrer Azure-Ressourcen zu erlangen“ erfahren Sie, wie Tags Sie bei der Verwaltung Ihrer Azure-Ressourcenverwendung unterstützen können.and [Gain insights into your Microsoft Azure resource consumption][Gain insights into your Microsoft Azure resource consumption]

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/manage-resources-powershell.md
[Azure Resource Cmdlets]: https://docs.microsoft.com/powershell/module/az.resources/
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
