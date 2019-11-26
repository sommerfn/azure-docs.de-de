---
title: 'Tutorial: Hochverfügbarkeit für virtuelle Windows-Computer in Azure'
description: In diesem Tutorial erfahren Sie, wie Sie Azure PowerShell zum Bereitstellen hoch verfügbarer virtueller Computer in Verfügbarkeitsgruppen verwenden.
documentationcenter: ''
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: tutorial
ms.date: 11/30/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 0f94f4d312cefec80a0f294e256ee1ad908b903c
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74068134"
---
# <a name="tutorial-create-and-deploy-highly-available-virtual-machines-with-azure-powershell"></a>Tutorial: Erstellen und Bereitstellen hoch verfügbarer virtueller Computer mit Azure PowerShell

In diesem Tutorial erfahren Sie, wie Sie die Verfügbarkeit und Zuverlässigkeit Ihrer virtuellen Computer mithilfe von Verfügbarkeitsgruppen erhöhen. Verfügbarkeitsgruppen sorgen dafür, dass die von Ihnen in Azure bereitgestellten virtuellen Computer auf mehrere isolierte Hardwareknoten in einem Cluster verteilt werden. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verfügbarkeitsgruppe erstellen
> * Erstellen eines virtuellen Computers in einer Verfügbarkeitsgruppe
> * Überprüfen der verfügbaren VM-Größen
> * Überprüfen des Azure Advisor


## <a name="availability-set-overview"></a>Übersicht über Verfügbarkeitsgruppen

Eine Verfügbarkeitsgruppe ist eine Funktion zur logischen Gruppierung, um VM-Ressourcen bei ihrer Bereitstellung voneinander zu isolieren. Azure stellt sicher, dass die virtuellen Computer innerhalb einer Verfügbarkeitsgruppe auf mehrere physische Server, Computeracks, Speichereinheiten und Netzwerkswitches verteilt werden. Bei einem Hardware- oder Softwarefehler ist nur ein Teil der virtuellen Computer betroffen, sodass Ihre Lösung weiterhin verwendet werden kann. Verfügbarkeitsgruppen sind für das Erstellen zuverlässiger Cloudlösungen unverzichtbar.

In einer typischen VM-basierten Lösung gibt es unter Umständen vier Front-End-Webserver und zwei Back-End-VMs. Sie können in Azure zwei Verfügbarkeitsgruppen definieren, bevor Sie Ihre virtuellen Computer bereitstellen: eine für die Webebene und eine für die Back-End-Ebene. Wenn Sie einen neuen virtuellen Computer erstellen, geben Sie die Verfügbarkeitsgruppe als Parameter an. Azure stellt sicher, dass die virtuellen Computer über mehrere physische Hardwareressourcen isoliert werden. Wenn bei der physischen Hardware, auf der einer Ihrer Server ausgeführt wird, ein Problem auftritt, können Sie darauf vertrauen, dass die anderen Instanzen Ihrer Server weiterhin einwandfrei ausgeführt werden, da sie sich auf anderer Hardware befinden.

Verwenden Sie Verfügbarkeitsgruppen, wenn Sie zuverlässige VM-basierte Lösungen in Azure bereitstellen möchten.

## <a name="launch-azure-cloud-shell"></a>Starten von Azure Cloud Shell

Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. 

Wählen Sie zum Öffnen von Cloud Shell oben rechts in einem Codeblock einfach die Option **Ausprobieren**. Sie können Cloud Shell auch auf einer separaten Browserregisterkarte starten, indem Sie zu [https://shell.azure.com/powershell](https://shell.azure.com/powershell) navigieren. Wählen Sie **Kopieren**, um die Blöcke mit dem Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen.

## <a name="create-an-availability-set"></a>Verfügbarkeitsgruppe erstellen

Die Hardware an einem Standort ist in mehrere Updatedomänen und Fehlerdomänen unterteilt. Eine **Updatedomäne** ist eine Gruppe von virtuellen Computern und der zugrunde liegenden physischen Hardwarekomponenten, die gleichzeitig neu gestartet werden können. Virtuelle Computer in ein und derselben **Fehlerdomäne** nutzen denselben Speicher sowie eine Stromquelle und einen Netzwerkswitch gemeinsam.  

Sie können mithilfe von [New-AzAvailabilitySet](https://docs.microsoft.com/powershell/module/az.compute/new-azavailabilityset) eine Verfügbarkeitsgruppe erstellen. In diesem Beispiel beträgt die Anzahl von Update- und Fehlerdomänen *2*, und die Verfügbarkeitsgruppe hat den Namen*myAvailabilitySet*.

Erstellen Sie eine Ressourcengruppe.

```azurepowershell-interactive
New-AzResourceGroup `
   -Name myResourceGroupAvailability `
   -Location EastUS
```

Erstellen Sie mit [New-AzAvailabilitySet](https://docs.microsoft.com/powershell/module/az.compute/new-azavailabilityset) mit dem Parameter `-sku aligned` eine verwaltete Verfügbarkeitsgruppe.

```azurepowershell-interactive
New-AzAvailabilitySet `
   -Location "EastUS" `
   -Name "myAvailabilitySet" `
   -ResourceGroupName "myResourceGroupAvailability" `
   -Sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Erstellen von virtuellen Computern in einer Verfügbarkeitsgruppe
Virtuelle Computer müssen in der Verfügbarkeitsgruppe erstellt werden, um sicherzustellen, dass sie ordnungsgemäß auf die Hardwarekomponenten verteilt werden. Nach der Erstellung kann einer Verfügbarkeitsgruppe kein vorhandener virtueller Computer mehr hinzugefügt werden. 


Verwenden Sie beim Erstellen eines virtuellen Computers mit [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) den Parameter `-AvailabilitySetName`, um den Namen der Verfügbarkeitsgruppe anzugeben.

Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) zuerst den Benutzernamen und das Kennwort des VM-Administrators fest:

```azurepowershell-interactive
$cred = Get-Credential
```

Erstellen Sie nun mit [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) zwei virtuelle Computer in der Verfügbarkeitsgruppe.

```azurepowershell-interactive
for ($i=1; $i -le 2; $i++)
{
    New-AzVm `
        -ResourceGroupName "myResourceGroupAvailability" `
        -Name "myVM$i" `
        -Location "East US" `
        -VirtualNetworkName "myVnet" `
        -SubnetName "mySubnet" `
        -SecurityGroupName "myNetworkSecurityGroup" `
        -PublicIpAddressName "myPublicIpAddress$i" `
        -AvailabilitySetName "myAvailabilitySet" `
        -Credential $cred
}
```

Die Erstellung und Konfiguration der beiden virtuellen Computer dauert einige Minuten. Anschließend verfügen Sie über zwei virtuelle Computer, die auf die zugrunde liegenden Hardwarekomponenten verteilt sind. 

Im Portal sollten Sie bei der Verfügbarkeitsgruppe unter **Ressourcengruppen** > **myResourceGroupAvailability** > **myAvailabilitySet** sehen können, dass die virtuellen Computer auf zwei Fehler- und Updatedomänen verteilt sind.

![Verfügbarkeitsgruppe im Portal](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Prüfen der verfügbaren VM-Größen 

Sie können der Verfügbarkeitsgruppe später weitere virtuelle Computer hinzufügen. Dazu müssen Sie jedoch wissen, welche VM-Größen in der Hardware verfügbar sind. Verwenden Sie [Get-AzVMSize](https://docs.microsoft.com/powershell/module/az.compute/get-azvmsize), um alle verfügbaren Größen im Hardwarecluster für die Verfügbarkeitsgruppe aufzulisten.

```azurepowershell-interactive
Get-AzVMSize `
   -ResourceGroupName "myResourceGroupAvailability" `
   -AvailabilitySetName "myAvailabilitySet"
```

## <a name="check-azure-advisor"></a>Überprüfen des Azure Advisor 

Sie können mit dem Azure Advisor auch weitere Informationen zu Methoden zur Verbesserung der Verfügbarkeit Ihrer virtuellen Computer abrufen. Der Azure Advisor analysiert Ihre Konfiguration und Nutzungstelemetrie und macht anschließend Vorschläge zu Lösungen, die die Wirtschaftlichkeit, Leistung, Verfügbarkeit und Sicherheit Ihrer Azure-Ressourcen steigern können.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, wählen Sie die Option **Alle Dienste** aus, und geben Sie **Advisor** ein. Auf dem Advisor-Dashboard werden personalisierte Empfehlungen für das ausgewählte Abonnement angezeigt. Weitere Informationen finden Sie unter [Erste Schritte mit dem Azure Advisor](../../advisor/advisor-get-started.md).


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Verfügbarkeitsgruppe erstellen
> * Erstellen eines virtuellen Computers in einer Verfügbarkeitsgruppe
> * Überprüfen der verfügbaren VM-Größen
> * Überprüfen des Azure Advisor

Im nächsten Tutorial erhalten Sie Informationen zu VM-Skalierungsgruppen.

> [!div class="nextstepaction"]
> [Erstellen einer VM-Skalierungsgruppe](tutorial-create-vmss.md)


