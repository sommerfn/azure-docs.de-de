---
title: Erstellen eines virtuellen Windows-Computers mit SQL Server unter Verwendung von Azure PowerShell | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie unter Verwendung von Azure PowerShell einen virtuellen Windows-Computer mit SQL Server 2017 erstellen.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 12/21/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 69781b7c5187bd9166946a96a8b47233d0f77208
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70100674"
---
# <a name="quickstart-create-a-sql-server-windows-virtual-machine-with-azure-powershell"></a>Schnellstart: Erstellen eines virtuellen Windows-Computers mit SQL Server unter Verwendung von Azure PowerShell

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Azure PowerShell einen virtuellen SQL Server-Computer erstellen.

> [!TIP]
> - Sie erfahren, wie Sie schnell einen virtuellen SQL-Computer bereitstellen und eine Verbindung damit herstellen. Weitere Informationen zu anderen Azure PowerShell-Optionen für die Erstellung virtueller SQL-Computer finden Sie im [Bereitstellungshandbuch für virtuelle SQL Server-Computer mit Azure PowerShell](virtual-machines-windows-ps-sql-create.md).
> - Falls Sie Fragen zu SQL Server-VMs haben, finden Sie in den [häufig gestellten Fragen](virtual-machines-windows-sql-server-iaas-faq.md) weitere Informationen.

## <a id="subscription"></a> Einrichten eines Azure-Abonnements

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a id="powershell"></a> Abrufen von Azure PowerShell

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

## <a name="configure-powershell"></a>Konfigurieren von PowerShell

1. Öffnen Sie PowerShell, und führen Sie den Befehl **Connect-AzAccount** aus, um den Zugriff auf Ihr Azure-Konto einzurichten.

   ```powershell
   Connect-AzAccount
   ```

1. Ein Bildschirm zur Eingabe Ihrer Anmeldeinformationen sollte angezeigt werden. Verwenden Sie die E-Mail-Adresse und das Kennwort für die Anmeldung beim Azure-Portal.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

1. Definieren Sie eine Variable mit einem eindeutigen Ressourcengruppennamen. Dieser Name wird der Einfachheit halber bei den übrigen Befehlen in dieser Schnellstartanleitung als Grundlage für andere Ressourcennamen verwendet.

   ```powershell
   $ResourceGroupName = "sqlvm1"
   ```

1. Definieren Sie einen Standort einer Azure-Zielregion für alle VM-Ressourcen.

   ```powershell
   $Location = "East US"
   ```

1. Erstellen Sie die Ressourcengruppe.

   ```powershell
   New-AzResourceGroup -Name $ResourceGroupName -Location $Location
   ```

## <a name="configure-network-settings"></a>Konfigurieren der Netzwerkeinstellungen

1. Erstellen Sie ein virtuelles Netzwerk, ein Subnetz und eine öffentliche IP-Adresse. Diese Ressourcen dienen dazu, dem virtuellen Computer Netzwerkkonnektivität bereitzustellen und ihn mit dem Internet zu verbinden.

   ``` PowerShell
   $SubnetName = $ResourceGroupName + "subnet"
   $VnetName = $ResourceGroupName + "vnet"
   $PipName = $ResourceGroupName + $(Get-Random)

   # Create a subnet configuration
   $SubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix 192.168.1.0/24

   # Create a virtual network
   $Vnet = New-AzVirtualNetwork -ResourceGroupName $ResourceGroupName -Location $Location `
      -Name $VnetName -AddressPrefix 192.168.0.0/16 -Subnet $SubnetConfig

   # Create a public IP address and specify a DNS name
   $Pip = New-AzPublicIpAddress -ResourceGroupName $ResourceGroupName -Location $Location `
      -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name $PipName
   ```

1. Erstellen Sie eine Netzwerksicherheitsgruppe. Konfigurieren Sie Regeln, um Remotedesktop- und SQL Server-Verbindungen zuzulassen.

   ```powershell
   # Rule to allow remote desktop (RDP)
   $NsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow

   #Rule to allow SQL Server connections on port 1433
   $NsgRuleSQL = New-AzNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow

   # Create the network security group
   $NsgName = $ResourceGroupName + "nsg"
   $Nsg = New-AzNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

1. Erstellen Sie die Netzwerkschnittstelle.

   ```powershell
   $InterfaceName = $ResourceGroupName + "int"
   $Interface = New-AzNetworkInterface -Name $InterfaceName `
      -ResourceGroupName $ResourceGroupName -Location $Location `
      -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $Pip.Id `
      -NetworkSecurityGroupId $Nsg.Id
   ```

## <a name="create-the-sql-vm"></a>Erstellen des virtuellen SQL-Computers

1. Definieren Sie Ihre Anmeldeinformationen für den virtuellen Computer. Der Benutzername lautet „azureadmin“. Denken Sie daran, „\<password>“ zu ändern, bevor Sie den Befehl ausführen.

   ``` PowerShell
   # Define a credential object
   $SecurePassword = ConvertTo-SecureString '<password>' `
      -AsPlainText -Force
   $Cred = New-Object System.Management.Automation.PSCredential ("azureadmin", $securePassword)
   ```

1. Erstellen Sie ein VM-Konfigurationsobjekt, und erstellen Sie dann den virtuellen Computer. Der folgende Befehl erstellt einen virtuellen Computer mit SQL Server 2017 Developer Edition unter Windows Server 2016:

   ```powershell
   # Create a virtual machine configuration
   $VMName = $ResourceGroupName + "VM"
   $VMConfig = New-AzVMConfig -VMName $VMName -VMSize Standard_DS13_V2 |
      Set-AzVMOperatingSystem -Windows -ComputerName $VMName -Credential $Cred -ProvisionVMAgent -EnableAutoUpdate |
      Set-AzVMSourceImage -PublisherName "MicrosoftSQLServer" -Offer "SQL2017-WS2016" -Skus "SQLDEV" -Version "latest" |
      Add-AzVMNetworkInterface -Id $Interface.Id
   
   # Create the VM
   New-AzVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VMConfig
   ```

   > [!TIP]
   > Die Erstellung des virtuellen Computers dauert einige Minuten.

## <a name="install-the-sql-iaas-agent"></a>Installieren des SQL-IaaS-Agents

Um die Portalintegration und die SQL-VM-Features nutzen zu können, müssen Sie die [Erweiterung für SQL Server-IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md) installieren. Führen Sie nach der Erstellung des virtuellen Computers den folgenden Befehl aus, um den Agent auf dem neuen virtuellen Computer zu installieren:

   ```powershell
   Set-AzVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="remote-desktop-into-the-vm"></a>Herstellen einer Remotedesktopverbindung mit dem virtuellen Computer

1. Rufen Sie mithilfe des folgenden Befehls die öffentliche IP-Adresse für den neuen virtuellen Computer ab:

   ```powershell
   Get-AzPublicIpAddress -ResourceGroupName $ResourceGroupName | Select IpAddress
   ```

1. Übergeben Sie die zurückgegebene IP-Adresse als Befehlszeilenparameter an **mstsc**, um eine Remotedesktopsitzung mit dem neuen virtuellen Computer zu starten:

   ```
   mstsc /v:<publicIpAddress>
   ```

1. Wenn Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, entscheiden Sie sich für die Eingabe von Anmeldeinformationen für ein anderes Konto. Geben Sie den Benutzernamen mit einem vorangestellten umgekehrten Schrägstrich (Beispiel: `\azureadmin`) sowie das Kennwort ein, das Sie zuvor in dieser Schnellstartanleitung festgelegt haben.

## <a name="connect-to-sql-server"></a>Herstellen einer Verbindung mit SQL Server

1. Starten Sie nach der Anmeldung bei der Remotedesktopsitzung **SQL Server Management Studio 2017** über das Startmenü.

1. Behalten Sie im Dialogfeld **Mit Server verbinden** die Standardeinstellungen bei. Der Servername ist der Name des virtuellen Computers. Die Authentifizierung ist auf **Windows-Authentifizierung** festgelegt. Wählen Sie **Verbinden**aus.

Sie sind nun lokal mit SQL Server verbunden. Falls Sie eine Remoteverbindung herstellen möchten, müssen Sie die [Konnektivität konfigurieren](virtual-machines-windows-sql-connect.md) (entweder über das Portal oder manuell).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn der virtuelle Computer nicht kontinuierlich ausgeführt werden muss, können Sie ihn beenden, wenn er nicht verwendet wird, um unnötige Kosten zu vermeiden. Mit dem folgenden Befehl wird der virtuelle Computer beendet, bleibt aber für eine spätere Verwendung verfügbar.

```powershell
Stop-AzVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

Mit dem Befehl **Remove-AzResourceGroup** können Sie alle Ressourcen, die der VM zugeordnet sind, endgültig entfernen. Dadurch wird auch der virtuelle Computer endgültig gelöscht. Seien Sie daher vorsichtig, wenn Sie diesen Befehl verwenden.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie unter Verwendung von Azure PowerShell einen virtuellen Computer mit SQL Server 2017 erstellt. Weitere Informationen zum Migrieren Ihrer Daten zu dem neuen SQL Server-Computer finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Migrieren einer SQL Server-Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md)
