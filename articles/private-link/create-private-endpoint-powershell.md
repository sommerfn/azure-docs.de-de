---
title: Erstellen eines privaten Azure-Endpunkts mit Azure PowerShell | Microsoft-Dokumentation
description: Erfahren Sie mehr über Azure Private Link.
services: private-link
author: KumudD
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: 569bc021d978714472bf40bcf39f7134fec95970
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71130347"
---
# <a name="create-a-private-endpoint-using-azure-powershell"></a>Erstellen eines privaten Endpunkts mit Azure PowerShell
Ein privater Endpunkt ist der grundlegende Baustein für Private Link in Azure. Mit ihm können Azure-Ressourcen wie virtuelle Computer (VMs) privat mit Private Link-Ressourcen kommunizieren. 

In diesem Schnellstart erfahren Sie, wie Sie einen virtuellen Computer in einem virtuellen Azure-Netzwerk und einen SQL-Datenbank-Server mit einem privaten Azure-Endpunkt unter Verwendung von Azure PowerShell erstellen. Anschließend können Sie vom virtuellen Computer sicher auf den SQL-Datenbank-Server zugreifen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Bevor Sie Ihre Ressourcen erstellen können, müssen Sie eine Ressourcengruppe erstellen, die das virtuelle Netzwerk und den privaten Endpunkt mit [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) hostet. Im folgenden Beispiel wird am Speicherort *WestUS* eine Ressourcengruppe mit dem Namen *myResourceGroup* erstellt:

```azurepowershell

New-AzResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und ein Subnetz. Als nächstes weisen Sie das Subnetz dem virtuellen Netzwerk zu.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie mit [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) ein virtuelles Netzwerk für Ihren privaten Endpunkt. Das folgende Beispiel erstellt ein virtuelles Netzwerk namens *MyVirtualNetwork*:
 
```azurepowershell

$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Hinzufügen eines Subnetzes

Azure stellt Ressourcen innerhalb eines virtuellen Netzwerks in einem Subnetz bereit, daher müssen Sie ein Subnetz erstellen. Erstellen Sie eine Subnetzkonfiguration mit der Bezeichnung  *mySubnet* mit [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig). Im folgenden Beispiel wird ein Subnetz mit dem Namen *mySubnet* erstellt, wobei das Netzwerkrichtlinienflag für private Endpunkte auf **Deaktiviert** festgelegt ist.

```azurepowershell
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 10.0.0.0/24 `
  -PrivateEndpointNetworkPoliciesFlag "Disabled" `
  -VirtualNetwork $virtualNetwork
```

### <a name="associate-the-subnet-to-the-virtual-network"></a>Zuweisen eines Subnetzes zum virtuellen Netzwerk

Schreiben Sie mit  [Set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork) die Subnetzkonfiguration in das virtuelle Netzwerk. Dieser Befehl erstellt das Subnetz:

```azurepowershell
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie mit [New-AzVM](/powershell/module/az.compute/new-azvm) einen virtuellen Computer im virtuellen Netzwerk. Wenn Sie den nächsten Befehl ausführen, werden Sie zur Eingabe von Anmeldeinformationen aufgefordert. Geben Sie einen Benutzernamen und ein Kennwort für den virtuellen Computer ein:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "westcentralus" `
    -VirtualNetworkName "MyVirtualNetwork" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80,3389 `
    -AsJob  
```

Die `-AsJob`-Option erstellt den virtuellen Computer im Hintergrund. Sie können mit dem nächsten Schritt fortfahren.

Wenn Azure beginnt, den virtuellen Computer im Hintergrund zu erstellen, wird in etwas folgendes zurückgegeben:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM
```

## <a name="create-a-sql-database-server"></a>Erstellen eines SQL-Datenbank-Servers 

Erstellen Sie einen SQL-Datenbank-Server mit dem Befehl New-AzSqlServer. Denken Sie daran, dass der Name Ihres SQL-Datenbank-Servers innerhalb von Azure eindeutig sein muss, ersetzen Sie daher den Platzhalterwert in Klammern durch Ihren eigenen eindeutigen Wert:

```azurepowershell-interactive
$adminSqlLogin = "SqlAdmin"
$password = "ChangeYourAdminPassword1"

$server = New-AzSqlServer -ResourceGroupName "myResourceGroup" `
    -ServerName "myserver" `
    -Location "WestCentralUS" `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

New-AzSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName "myserver"`
    -DatabaseName "myda"`
    -RequestedServiceObjectiveName "S0" `
    -SampleName "AdventureWorksLT"
```

## <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts

Privater Endpunkt für den SQL-Datenbank-Server in Ihrem virtuellen Netzwerk mit [New-AzPrivateLinkServiceConnection](/powershell/module/az.network/New-AzPrivateLinkServiceConnection): 

```azurepowershell

$privateEndpointConnection = New-AzPrivateLinkServiceConnection -Name "myConnection" `
  -PrivateLinkServiceId $server.ResourceId `
  -GroupId "sqlServer" 
 
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  "myResourceGroup" -Name "MyVirtualNetwork"  
 
$subnet = $virtualNetwork `
  | Select -ExpandProperty subnets `
  | Where-Object  {$_.Name -eq 'mysubnet'}  
 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName "myResourceGroup" `
  -Name "myPrivateEndpoint" `
  -Location "westcentralus" `
  -Subnet  $subnet`
  -PrivateLinkServiceConnection $privateEndpointConnection
``` 

## <a name="configure-the-private-dns-zone"></a>Konfigurieren der privaten DNS Zone 
Erstellen Sie eine private DNS-Zone für die SQL-Datenbank-Server-Domäne, und erstellen Sie eine Zuordnungsverknüpfung mit dem virtuellen Netzwerk: 

```azurepowershell

$zone = New-AzPrivateDnsZone -ResourceGroupName "myResourceGroup" `
  -Name "privatelink.database.windows.net" 
 
$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName "myResourceGroup" `
  -ZoneName "privatelink.database.windows.net"`
  -Name "mylink" `
  -VirtualNetworkId $virtualNetwork.Id  
 
$networkInterface = Get-AzResource -ResourceId $privateEndpoint.NetworkInterfaces[0].Id -ApiVersion "2019-04-01" 
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
$recordName = $fqdn.split('.',2)[0] 
$dnsZone = $fqdn.split('.',2)[1] 
New-AzPrivateDnsRecordSet -Name $recordName -RecordType A -ZoneName "privatelink.database.windows.net"  `
-ResourceGroupName "myResourceGroup" -Ttl 600 `
-PrivateDnsRecords (New-AzPrivateDnsRecordConfig -IPv4Address $ipconfig.properties.privateIPAddress)  
} 
} 
``` 
  
## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Geben Sie mit  [Get-AzPublicIpAddress](/powershell/module/az.network/Get-AzPublicIpAddress) die öffentliche IP-Adresse eines virtuellen Computers zurück. Das Beispiel gibt die öffentliche IP-Adresse des virtuellen Computers  *myVm1* zurück:

```azurepowershell
Get-AzPublicIpAddress `
  -Name myPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress 
```  
Öffnen Sie eine Eingabeaufforderung auf dem lokalen Computer. Führen Sie den Befehl „mstsc“ aus. Ersetzen Sie  <publicIpAddress> durch die im vorherigen Schritt zurückgegebene öffentliche IP-Adresse: 


> [!NOTE]
> Wenn Sie diese Befehle über eine PowerShell-Eingabeaufforderung auf Ihrem lokalen Computer ausgeführt haben und mindestens die Azure PowerShell-Modulversion 1.0 verwenden, können Sie mit dieser Schnittstelle fortfahren.
```
mstsc /v:<publicIpAddress>
```

1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus. 
2. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben.
  > [!NOTE]
  > Unter Umständen müssen Sie „Weitere Optionen > Anderes Konto verwenden“ auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben. 
  
3. Wählen Sie  **OK** aus. 
4. Sie erhalten möglicherweise eine Zertifikatwarnung. Wenn dies der Fall ist, wählen Sie **Ja** oder **Weiter** aus. 

## <a name="access-sql-database-server-privately-from-the-vm"></a>Privates Zugreifen auf den SQL-Datenbank-Server vom virtuellen Computer

1. Öffnen Sie auf dem Remote Desktop von myVM PowerShell.
2. Geben Sie `nslookup myserver.database.windows.net` ein. 

    Sie erhalten eine Meldung wie die folgende:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:   myserver.database.windows.net
3. Install SQL Server Management Studio
4. In Connect to server, enter or select this information:
    Setting Value
      Server type   Select Database Engine.
      Server name   Select myserver.database.windows.net
      Username  Enter a username provided during creation.
      Password  Enter a password provided during creation.
      Remember password Select Yes.
5. Select Connect.
6. Browse Databases from left menu. 
7. (Optionally) Create or query information from mydatabase
8. Close the remote desktop connection to *myVM*. 

## Clean up resources 
When you're done using the private endpoint, SQL Database server and the VM, use [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) to remove the resource group and all the resources it has:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Azure Private Link](private-link-overview.md).
