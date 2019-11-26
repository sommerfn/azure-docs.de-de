---
title: 'Tutorial: Erstellen virtueller Computer mit ausgeführtem SQL-/IIS-/.NET-Stapel in Azure'
description: In diesem Tutorial erfahren Sie, wie Sie den Azure SQL-/IIS-/.NET-Stapel auf einem virtuellen Windows-Computer in Azure installieren.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 286d00ce718a34dea15a075984cc3b2a5e4cfec3
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74064659"
---
# <a name="tutorial-install-the-sql-iis-net-stack-in-a-windows-vm-with-azure-powershell"></a>Tutorial: Installieren des SQL-/IIS-/.NET-Stapels auf einem virtuellen Windows-Computer mit Azure PowerShell

In diesem Tutorial installieren Sie einen SQL-/IIS-/.NET-Stapel mithilfe von Azure PowerShell. Dieser Stapel besteht aus zwei virtuellen Computern unter Windows Server 2016: einer mit IIS und .NET und der andere mit SQL Server.

> [!div class="checklist"]
> * Erstellen einer VM 
> * Installieren von IIS und .NET Core SDK auf dem virtuellen Computer
> * Erstellen eines virtuellen Computers, auf dem SQL Server ausgeführt wird
> * Installieren der SQL Server-Erweiterung

## <a name="launch-azure-cloud-shell"></a>Starten von Azure Cloud Shell

Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. 

Wählen Sie zum Öffnen von Cloud Shell oben rechts in einem Codeblock einfach die Option **Ausprobieren**. Sie können Cloud Shell auch auf einer separaten Browserregisterkarte starten, indem Sie zu [https://shell.azure.com/powershell](https://shell.azure.com/powershell) navigieren. Wählen Sie **Kopieren**, um die Blöcke mit dem Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen.

## <a name="create-an-iis-vm"></a>Erstellen einer IIS-VM 

In diesem Beispiel verwenden wir das Cmdlet [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) in der PowerShell-Cloud Shell, um einen virtuellen Computer unter Windows Server 2016 zu erstellen und dann IIS und .NET Framework zu installieren. Der virtuelle IIS- und der virtuelle SQL-Computer gehören derselben Ressourcengruppe und demselben virtuellen Netzwerk an, daher erstellen wir Variablen für diese Namen.


```azurepowershell-interactive
$vmName = "IISVM"
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzVm `
    -ResourceGroupName $resourceGroup `
    -Name $vmName `
    -Location "East US" `
    -VirtualNetworkName $vNetName `
    -SubnetName "myIISSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -AddressPrefix 192.168.0.0/16 `
    -PublicIpAddressName "myIISPublicIpAddress" `
    -OpenPorts 80,3389 
```

Installieren Sie IIS und .NET Framework mit dem Cmdlet [Set-AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) unter Verwendung der benutzerdefinierten Skripterweiterung.

```azurepowershell-interactive
Set-AzVMExtension `
    -ResourceGroupName $resourceGroup `
    -ExtensionName IIS `
    -VMName $vmName `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features"}' `
    -Location EastUS
```

## <a name="create-another-subnet"></a>Erstellen eines anderen Subnetzes

Erstellen Sie ein zweites Subnetz für die SQL-VM. Rufen Sie das VNET über [Get-AzVirtualNetwork]{/powershell/module/az.network/get-azvirtualnetwork} ab.

```azurepowershell-interactive
$vNet = Get-AzVirtualNetwork `
   -Name $vNetName `
   -ResourceGroupName $resourceGroup
```

Erstellen Sie mit [Add-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/add-azvirtualnetworksubnetconfig) eine Konfiguration für das Subnetz.


```azurepowershell-interactive
Add-AzVirtualNetworkSubnetConfig `
   -AddressPrefix 192.168.0.0/24 `
   -Name mySQLSubnet `
   -VirtualNetwork $vNet `
   -ServiceEndpoint Microsoft.Sql
```

Verwenden Sie [Set-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/set-azvirtualnetwork), um das VNET mit den neuen Subnetzinformationen zu aktualisieren.
   
```azurepowershell-interactive   
$vNet | Set-AzVirtualNetwork
```

## <a name="azure-sql-vm"></a>Virtueller Azure SQL-Computer

Verwenden Sie ein vorkonfiguriertes Azure Marketplace-Image einer SQL Server-Instanz zum Erstellen des virtuellen SQL-Computers. Zunächst erstellen wir den virtuellen Computer, und danach installieren wir die SQL Server-Erweiterung auf dem virtuellen Computer. 


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName $resourceGroup `
    -Name "mySQLVM" `
    -ImageName "MicrosoftSQLServer:SQL2016SP1-WS2016:Enterprise:latest" `
    -Location eastus `
    -VirtualNetworkName $vNetName `
    -SubnetName "mySQLSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "mySQLPublicIpAddress" `
    -OpenPorts 3389,1401 
```

Verwenden Sie [Set-AzVMSqlServerExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsqlserverextension), um dem virtuellen SQL-Computer die [SQL Server-Erweiterung](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) hinzuzufügen.

```azurepowershell-interactive
Set-AzVMSqlServerExtension `
   -ResourceGroupName $resourceGroup  `
   -VMName mySQLVM `
   -Name "SQLExtension" `
   -Location "EastUS"
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen SQL-/IIS-/.NET-Stapel mithilfe von Azure PowerShell installiert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer VM 
> * Installieren von IIS und .NET Core SDK auf dem virtuellen Computer
> * Erstellen eines virtuellen Computers, auf dem SQL Server ausgeführt wird
> * Installieren der SQL Server-Erweiterung

Im nächsten Tutorial erfahren Sie, wie Sie einen IIS-Webserver mit SSL-Zertifikaten schützen.

> [!div class="nextstepaction"]
> [Schützen eines IIS-Webservers mit SSL-Zertifikaten](tutorial-secure-web-server.md)

