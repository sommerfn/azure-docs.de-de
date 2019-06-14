---
title: Erstellen virtueller Windows-Computer mithilfe des vereinfachten Cmdlets „New-AzVM“ in Azure Cloud Shell | Microsoft-Dokumentation
description: In diesem kurzen Artikel erfahren Sie, wie Sie virtuelle Windows-Computer in Azure Cloud Shell mithilfe des vereinfachten Cmdlets „New-AzVM“ erstellen.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: cdf7385b3686c5c3f91e66c67ab5f0758935b39f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66730130"
---
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azvm-cmdlet-in-cloud-shell"></a>Erstellen eines virtuellen Windows-Computers mithilfe des vereinfachten Cmdlets „New-AzVM“ in Cloud Shell 

Das Cmdlet [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) verfügt über einen vereinfachten Parametersatz für die Erstellung eines neuen virtuellen Computers mithilfe von PowerShell. In diesem Thema erfahren Sie, wie Sie mithilfe von PowerShell in Azure Cloud Shell und der vorinstallierten neuesten Version des Cmdlets „New-AzureVM“ einen neuen virtuellen Computer erstellen. Wir verwenden einen vereinfachten Parametersatz, der auf der Grundlage intelligenter Standardwerte automatisch alle erforderlichen Ressourcen erstellt. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]


## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Mithilfe des Cmdlets [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) können Sie einen virtuellen Computer mit intelligenten Standardwerten erstellen, die unter anderem die Verwendung des Windows Server 2016 Datacenter-Images aus dem Azure Marketplace umfassen. Sie können „New-AzVM“ mit lediglich dem Parameter **-Name** verwenden, und dieser Wert wird dann für alle Ressourcennamen herangezogen. In diesem Beispiel legen wir den Parameter **-Name** auf *myVM* fest. 

Vergewissern Sie sich in Cloud Shell, dass **PowerShell** ausgewählt ist, und geben Sie Folgendes ein:

```azurepowershell-interactive
New-AzVm -Name myVM
```

Sie werden zur Erstellung eines Benutzernamens und eines Kennworts für den virtuellen Computer aufgefordert. Diese werden später zum Herstellen der Verbindung mit dem virtuellen Computer benötigt. Das Kennwort muss 12 bis 123 Zeichen lang sein und zur Erfüllung der Komplexitätsanforderungen drei der folgenden vier Elemente enthalten: einen Kleinbuchstaben, einen Großbuchstaben, eine Zahl und ein Sonderzeichen.

Die Erstellung des virtuellen Computers und der dazugehörigen Ressourcen dauert einen Moment. Nach Abschluss des Vorgangs können Sie die erstellten Ressourcen mithilfe des Cmdlets [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) anzeigen.

```azurepowershell-interactive
Get-AzResource `
    -ResourceGroupName myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

Nachdem die Bereitstellung abgeschlossen ist, stellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer her.

Geben Sie mit dem Befehl [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) die öffentliche IP-Adresse des virtuellen Computers zurück. Notieren Sie sich diese IP-Adresse.

```azurepowershell-interactive
Get-AzPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Öffnen Sie auf Ihrem lokalen Computer eine Eingabeaufforderung, und starten Sie mithilfe des Befehls **mstsc** eine Remotedesktopsitzung mit Ihrem neuen virtuellen Computer. Ersetzen Sie &lt;publicIPAddress&gt; durch die IP-Adresse Ihres virtuellen Computers. Geben Sie den bei der Erstellung des virtuellen Computers angegebenen Benutzernamen und das dazugehörige Kennwort ein, wenn Sie dazu aufgefordert werden.

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>Angeben unterschiedlicher Ressourcennamen

Sie können auch aussagekräftigere Namen für die Ressourcen angeben und sie trotzdem automatisch erstellen lassen. Im folgenden Beispiel haben wir mehrere Ressourcen für den neuen virtuellen Computer benannt (einschließlich einer neuen Ressourcengruppe):

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 3389
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) entfernen.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myVM
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Thema haben Sie mithilfe von „New-AzVM“ einen einfachen virtuellen Computer bereitgestellt und anschließend eine RDP-Verbindung damit hergestellt. Fahren Sie mit dem Tutorial für virtuelle Windows-Computer fort, um weitere Informationen zu virtuellen Azure-Computern zu erhalten.

> [!div class="nextstepaction"]
> [Azure-Tutorials zu virtuellen Windows-Computern](./tutorial-manage-vm.md)
