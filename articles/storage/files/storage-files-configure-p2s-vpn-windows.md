---
title: Konfigurieren eines P2S-VPN (Point-to-Site) unter Windows zur Verwendung mit Azure Files | Microsoft-Dokumentation
description: Konfigurieren eines P2S-VPN (Point-to-Site) unter Windows zur Verwendung mit Azure Files
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 10/19/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 527ab905997d18433d1dba5c16ee67c8146f5afa
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73126456"
---
# <a name="configure-a-point-to-site-p2s-vpn-on-windows-for-use-with-azure-files"></a>Konfigurieren eines P2S-VPN (Point-to-Site) unter Windows zur Verwendung mit Azure Files
Sie können eine P2S-VPN-Verbindung (Point-to-Site) verwenden, um Ihre Azure-Dateifreigaben außerhalb von Azure über SMB einzubinden, ohne Port 445 zu öffnen. Eine P2S-VPN-Verbindung ist eine VPN-Verbindung zwischen Azure und einem einzelnen Client. Um eine P2S-VPN-Verbindung mit Azure Files zu verwenden, muss für jeden Client, der eine Verbindung herstellen möchte, eine P2S-VPN-Verbindung konfiguriert werden. Wenn Sie über viele Clients verfügen, die sich über Ihr lokales Netzwerk mit Ihren Azure-Dateifreigaben verbinden müssen, können Sie anstelle einer P2S-Verbindung für jeden Client eine S2S-VPN-Verbindung (Site-to-Site) verwenden. Weitere Informationen finden Sie unter [Konfigurieren eines S2S-VPN (Site-to-Site) zur Verwendung mit Azure Files](storage-files-configure-s2s-vpn.md).

Es wird dringend empfohlen, vor der Lektüre des vorliegenden Artikels den Artikel [Netzwerküberlegungen für den direkten Zugriff auf Azure-Dateifreigaben](storage-files-networking-overview.md) zu lesen, der eine umfassende Erläuterung der für Azure Files verfügbaren Netzwerkoptionen enthält.

Der Artikel beschreibt die Schritte zur Konfiguration eines P2S-VPN unter Windows (Windows-Client und Windows-Server), um Azure-Dateifreigaben direkt lokal einzubinden. Wenn Sie den Datenverkehr zur Azure-Dateisynchronisierung über ein VPN leiten möchten, lesen Sie den Artikel [Konfigurieren der Proxy- und Firewalleinstellungen der Dateisynchronisierung](storage-sync-files-firewall-and-proxy.md).

## <a name="prerequisites"></a>Voraussetzungen
- Die neueste Version des Azure PowerShell-Moduls. Informationen zum Installieren des Azure PowerShell-Moduls finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps) im Abschnitt zu Ihrem Betriebssystem. Wenn Sie lieber mit der Azure CLI unter Windows arbeiten möchten, können Sie dies tun, die folgenden Anweisungen beziehen sich jedoch auf Azure PowerShell.

- Das PowerShell-Modul für privates Azure-DNS. Dieses wird aktuell nicht als Bestandteil des Azure PowerShell-Moduls bereitgestellt und muss deshalb mit der folgenden Methode installiert werden:
    ```PowerShell
    if ($PSVersionTable.PSVersion -ge [System.Version]::new(6, 0)) {
        Install-Module -Name Az.PrivateDns -AllowClobber -AllowPrerelease
    } else {
        Install-Module -Name Az.PrivateDns -RequiredVersion "0.1.3"
    }

    Import-Module -Name Az.PrivateDns
    ```  

- Eine Azure-Dateifreigabe, die Sie lokal einbinden möchten. Sie können mit Ihrem P2S-VPN entweder eine Azure-Dateifreigabe vom Typ [Standard](storage-how-to-create-file-share.md) oder [Premium](storage-how-to-create-premium-fileshare.md) verwenden.

## <a name="deploy-a-virtual-network"></a>Bereitstellen eines virtuellen Netzwerks
Um von einer lokalen Umgebung aus über ein P2S-VPN auf Ihre Azure-Dateifreigabe und andere Azure-Ressourcen zuzugreifen, müssen Sie ein virtuelles Netzwerk (VNET) erstellen. Die automatisch erstellte P2S-VPN-Verbindung ist eine Brücke zwischen Ihrem lokalen Windows-Computer und diesem virtuellen Azure-Netzwerk.

Das folgende PowerShell-Skript erstellt ein virtuelles Azure-Netzwerk mit drei Subnetzen: ein Subnetz für den Dienstendpunkt Ihres Speicherkontos, ein Subnetz für den privaten Endpunkt Ihres Speicherkontos (dieser wird für den Zugriff auf das lokale Speicherkonto ohne benutzerdefiniertes Routing für die öffentliche IP-Adresse benötigt, die sich ändern kann), und ein Subnetz für das Gateway für virtuelle Netzwerke, das den VPN-Dienst bereitstellt. 

Denken Sie daran, `<region>`, `<resource-group>` und `<desired-vnet-name>` durch die entsprechenden Werte für Ihre Umgebung zu ersetzen.

```PowerShell
$region = "<region>"
$resourceGroupName = "<resource-group>" 
$virtualNetworkName = "<desired-vnet-name>"

$virtualNetwork = New-AzVirtualNetwork `
    -ResourceGroupName $resourceGroupName `
    -Name $virtualNetworkName `
    -Location $region `
    -AddressPrefix "192.168.0.0/16"

Add-AzVirtualNetworkSubnetConfig `
    -Name "ServiceEndpointSubnet" `
    -AddressPrefix "192.168.0.0/24" `
    -VirtualNetwork $virtualNetwork `
    -ServiceEndpoint "Microsoft.Storage" `
    -WarningAction SilentlyContinue | Out-Null

Add-AzVirtualNetworkSubnetConfig `
    -Name "PrivateEndpointSubnet" `
    -AddressPrefix "192.168.1.0/24" `
    -VirtualNetwork $virtualNetwork `
    -WarningAction SilentlyContinue | Out-Null

Add-AzVirtualNetworkSubnetConfig `
    -Name "GatewaySubnet" `
    -AddressPrefix "192.168.2.0/24" `
    -VirtualNetwork $virtualNetwork `
    -WarningAction SilentlyContinue | Out-Null

$virtualNetwork | Set-AzVirtualNetwork | Out-Null
$virtualNetwork = Get-AzVirtualNetwork `
    -ResourceGroupName $resourceGroupName `
    -Name $virtualNetworkName

$serviceEndpointSubnet = $virtualNetwork.Subnets | `
    Where-Object { $_.Name -eq "ServiceEndpointSubnet" }
$privateEndpointSubnet = $virtualNetwork.Subnets | `
    Where-Object { $_.Name -eq "PrivateEndpointSubnet" }
$gatewaySubnet = $virtualNetwork.Subnets | ` 
    Where-Object { $_.Name -eq "GatewaySubnet" }
```

## <a name="restrict-the-storage-account-to-the-virtual-network"></a>Beschränken des Speicherkontos auf das virtuelle Netzwerk
Standardmäßig können Sie beim Erstellen eines Speicherkontos von überall auf der Welt darauf zugreifen, solange Sie Ihre Anforderung authentifizieren können (z. B. mit Ihrer Active Directory-Identität oder mit dem Speicherkontoschlüssel). Um den Zugriff auf dieses Speicherkonto auf das soeben erstellte virtuelle Netzwerk zu beschränken, müssen Sie einen Netzwerkregelsatz erstellen, der den Zugriff innerhalb des virtuellen Netzwerks erlaubt und jeden weiteren Zugriff unterbindet.

Die Beschränkung des Speicherkontos auf das virtuelle Netzwerk erfordert die Verwendung eines Dienstendpunkts. Der Dienstendpunkt ist eine Netzwerkkomponente, mit der nur innerhalb des virtuellen Netzwerks auf das öffentliche DNS/die öffentliche IP-Adresse zugegriffen werden kann. Da nicht garantiert werden kann, dass die öffentliche IP-Adresse unverändert bleibt, möchten wir einen privaten Endpunkt und keinen Dienstendpunkt für das Speicherkonto verwenden. Eine Beschränkung des Speicherkontos ist aber nur möglich, wenn auch ein Dienstendpunkt verfügbar gemacht wird.

Denken Sie daran, `<storage-account-name>` durch das Speicherkonto zu ersetzen, auf das Sie zugreifen möchten.

```PowerShell
$storageAccountName = "<storage-account-name>"

$storageAccount = Get-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName

$networkRule = Add-AzStorageAccountNetworkRule `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -VirtualNetworkResourceId $serviceEndpointSubnet.Id

Update-AzStorageAccountNetworkRuleSet `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -Bypass AzureServices `
    -DefaultAction Deny `
    -VirtualNetworkRule $networkRule | Out-Null
``` 

## <a name="create-a-private-endpoint-preview"></a>Erstellen eines privaten Endpunkts (Vorschau)
Wenn Sie einen privaten Endpunkt für Ihr Speicherkonto erstellen, erhält Ihr Speicherkonto eine IP-Adresse im IP-Adressraum Ihres virtuellen Netzwerks. Wenn Sie Ihre Azure-Dateifreigabe von einer lokalen Umgebung aus mithilfe dieser privaten IP-Adresse einbinden, leiten die von der VPN-Installation automatisch definierten Routingregeln Ihre Einbindungsanforderung über das VPN an das Speicherkonto weiter. 

```PowerShell
$internalVnet = Get-AzResource `
    -ResourceId $virtualNetwork.Id `
    -ApiVersion "2019-04-01"

$internalVnet.Properties.subnets[1].properties.privateEndpointNetworkPolicies = "Disabled"
$internalVnet | Set-AzResource -Force | Out-Null

$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
    -Name "myConnection" `
    -PrivateLinkServiceId $storageAccount.Id `
    -GroupId "file"

$privateEndpoint = New-AzPrivateEndpoint `
    -ResourceGroupName $resourceGroupName `
    -Name "$storageAccountName-privateEndpoint" `
    -Location $region `
    -Subnet $privateEndpointSubnet `
    -PrivateLinkServiceConnection $privateEndpointConnection

$zone = Get-AzPrivateDnsZone -ResourceGroupName $resourceGroupName
if ($null -eq $zone) {
    $zone = New-AzPrivateDnsZone `
        -ResourceGroupName $resourceGroupName `
        -Name "privatelink.file.core.windows.net"
} else {
    $zone = $zone[0]
}

$link = New-AzPrivateDnsVirtualNetworkLink `
    -ResourceGroupName $resourceGroupName `
    -ZoneName $zone.Name `
    -Name ($virtualNetwork.Name + "-link") `
    -VirtualNetworkId $virtualNetwork.Id

$internalNic = Get-AzResource `
    -ResourceId $privateEndpoint.NetworkInterfaces[0].Id `
    -ApiVersion "2019-04-01"

foreach($ipconfig in $internalNic.Properties.ipConfigurations) {
    foreach($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) {
        $recordName = $fqdn.split('.', 2)[0]
        $dnsZone = $fqdn.split('.', 2)[1]
        New-AzPrivateDnsRecordSet `
            -ResourceGroupName $resourceGroupName ` 
            -Name $recordName `
            -RecordType A `
            -ZoneName $zone.Name `
            -Ttl 600 `
            -PrivateDnsRecords (New-AzPrivateDnsRecordConfig `
                -IPv4Address $ipconfig.properties.privateIPAddress) | Out-Null
    }
}
```

## <a name="create-root-certificate-for-vpn-authentication"></a>Erstellen eines Stammzertifikats für die VPN-Authentifizierung
Damit VPN-Verbindungen von Ihren lokalen Windows-Computern für den Zugriff auf Ihr virtuelles Netzwerk authentifiziert werden können, müssen Sie zwei Zertifikate erstellen: ein Stammzertifikat, das für das VM-Gateway bereitgestellt wird, und ein Clientzertifikat, das mit dem Stammzertifikat signiert wird. Das folgende PowerShell-Skript erstellt das Stammzertifikat. Das Clientzertifikat wird nach der Erstellung des Azure-Gateways für virtuelle Netzwerke mithilfe von Informationen aus dem Gateway erstellt. 

```PowerShell
$rootcertname = "CN=P2SRootCert"
$certLocation = "Cert:\CurrentUser\My"
$vpnTemp = "C:\vpn-temp\"
$exportedencodedrootcertpath = $vpnTemp + "P2SRootCertencoded.cer"
$exportedrootcertpath = $vpnTemp + "P2SRootCert.cer"

if (-Not (Test-Path $vpnTemp)) {
    New-Item -ItemType Directory -Force -Path $vpnTemp | Out-Null
}

if ($PSVersionTable.PSVersion -ge [System.Version]::new(6, 0)) {
    Install-Module WindowsCompatibility
    Import-WinModule PKI
}

$rootcert = New-SelfSignedCertificate `
    -Type Custom `
    -KeySpec Signature `
    -Subject $rootcertname `
    -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 `
    -KeyLength 2048 `
    -CertStoreLocation $certLocation `
    -KeyUsageProperty Sign `
    -KeyUsage CertSign

Export-Certificate `
    -Cert $rootcert `
    -FilePath $exportedencodedrootcertpath `
    -NoClobber | Out-Null

certutil -encode $exportedencodedrootcertpath $exportedrootcertpath | Out-Null

$rawRootCertificate = Get-Content -Path $exportedrootcertpath

[System.String]$rootCertificate = ""
foreach($line in $rawRootCertificate) { 
    if ($line -notlike "*Certificate*") { 
        $rootCertificate += $line 
    } 
}
```

## <a name="deploy-virtual-network-gateway"></a>Bereitstellen eines Gateways für virtuelle Netzwerke
Das Azure-Gateway für virtuelle Netzwerke ist der Dienst, mit dem sich Ihre lokalen Windows-Computer verbinden. Zur Bereitstellung dieses Diensts werden zwei grundlegende Komponenten benötigt: eine öffentliche IP-Adresse, die das Gateway gegenüber Ihren Clients identifiziert (unabhängig von deren Standort), und ein zuvor erstelltes Stammzertifikat für die Authentifizierung Ihrer Clients.

Denken Sie daran, `<desired-vpn-name-here>` durch den Namen zu ersetzen, den Sie für diese Ressourcen verwenden möchten.

> [!Note]  
> Die Bereitstellung eines Azure-Gateways für virtuelle Netzwerke kann bis zu 45 Minuten dauern. Während der Bereitstellung dieser Ressource wird dieses PowerShell-Skript blockiert, damit die Bereitstellung abgeschlossen werden kann. Dies entspricht dem erwarteten Verhalten.

```PowerShell
$vpnName = "<desired-vpn-name-here>" 
$publicIpAddressName = "$vpnName-PublicIP"

$publicIPAddress = New-AzPublicIpAddress `
    -ResourceGroupName $resourceGroupName ` 
    -Name $publicIpAddressName `
    -Location $region `
    -Sku Basic `
    -AllocationMethod Dynamic

$gatewayIpConfig = New-AzVirtualNetworkGatewayIpConfig `
    -Name "vnetGatewayConfig" `
    -SubnetId $gatewaySubnet.Id `
    -PublicIpAddressId $publicIPAddress.Id

$azRootCertificate = New-AzVpnClientRootCertificate `
    -Name "P2SRootCert" `
    -PublicCertData $rootCertificate

$vpn = New-AzVirtualNetworkGateway `
    -ResourceGroupName $resourceGroupName `
    -Name $vpnName `
    -Location $region `
    -GatewaySku VpnGw1 `
    -GatewayType Vpn `
    -VpnType RouteBased `
    -IpConfigurations $gatewayIpConfig `
    -VpnClientAddressPool "172.16.201.0/24" `
    -VpnClientProtocol IkeV2 `
    -VpnClientRootCertificates $azRootCertificate
```

## <a name="create-client-certificate"></a>Erstellen eines Clientzertifikats
Das Clientzertifikat wird mit dem URI des Gateways für virtuelle Netzwerke erstellt. Dieses Zertifikat wird mit dem zuvor erstellten Stammzertifikat signiert.

```PowerShell
$clientcertpassword = "1234"

$vpnClientConfiguration = New-AzVpnClientConfiguration `
    -ResourceGroupName $resourceGroupName `
    -Name $vpnName `
    -AuthenticationMethod EAPTLS

Invoke-WebRequest `
    -Uri $vpnClientConfiguration.VpnProfileSASUrl `
    -OutFile "$vpnTemp\vpnclientconfiguration.zip"

Expand-Archive `
    -Path "$vpnTemp\vpnclientconfiguration.zip" `
    -DestinationPath "$vpnTemp\vpnclientconfiguration"

$vpnGeneric = "$vpnTemp\vpnclientconfiguration\Generic"
$vpnProfile = ([xml](Get-Content -Path "$vpnGeneric\VpnSettings.xml")).VpnProfile

$exportedclientcertpath = $vpnTemp + "P2SClientCert.pfx"
$clientcertname = "CN=" + $vpnProfile.VpnServer

$clientcert = New-SelfSignedCertificate `
    -Type Custom `
    -DnsName $vpnProfile.VpnServer `
    -KeySpec Signature `
    -Subject $clientcertname `
    -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 `
    -KeyLength 2048 `
    -CertStoreLocation $certLocation `
    -Signer $rootcert `
    -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")

$mypwd = ConvertTo-SecureString -String $clientcertpassword -Force -AsPlainText

Export-PfxCertificate `
    -FilePath $exportedclientcertpath `
    -Password $mypwd `
    -Cert $clientcert | Out-Null
```

## <a name="configure-the-vpn-client"></a>Konfigurieren des VPN-Clients
Das Azure-Gateway für virtuelle Netzwerke erstellt ein herunterladbares Paket mit Konfigurationsdateien, die für die Initialisierung der VPN-Verbindung auf Ihrem lokalen Windows-Computer erforderlich sind. Die VPN-Verbindung wird mit dem [Always On-VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/)-Feature von Windows 10/Windows Server 2016+ konfiguriert. Dieses Paket enthält auch ausführbare Pakete, mit denen auf Wunsch der Legacy-Windows-VPN-Client konfiguriert wird. Dieser Leitfaden verwendet anstelle des Legacy-Windows-VPN-Clients das Always On-VPN-Feature, weil es den Endbenutzern ermöglicht, sich ohne Administratorrechte für den Computer mit dem Azure-VPN zu verbinden bzw. die VPN-Verbindung zu trennen. 

Das folgende Skript installiert das Clientzertifikat, das für die Authentifizierung beim Gateway für virtuelle Netzwerke erforderlich ist, lädt das VPN-Paket herunter und installiert es. Denken Sie daran, `<computer1>` und `<computer2>` durch die gewünschten Computer zu ersetzen. Sie können dieses Skript auf beliebig vielen Computern ausführen, indem Sie weitere PowerShell-Sitzungen in das `$sessions`-Array einfügen. Das verwendete Konto muss als Administrator auf jedem dieser Computer festgelegt sein. Wenn einer dieser Computer der lokale Computer ist, von dem aus Sie das Skript ausführen, müssen Sie das Skript aus einer PowerShell-Sitzung mit erhöhten Rechten ausführen. 

```PowerShell
$sessions = [System.Management.Automation.Runspaces.PSSession[]]@()
$sessions += New-PSSession -ComputerName "<computer1>"
$sessions += New-PSSession -ComputerName "<computer2>"

foreach ($session in $sessions) {
    Invoke-Command -Session $session -ArgumentList $vpnTemp -ScriptBlock { 
        $vpnTemp = $args[0]
        if (-Not (Test-Path $vpnTemp)) {
            New-Item `
                -ItemType Directory `
                -Force `
                -Path "C:\vpn-temp" | Out-Null
        }
    }

    Copy-Item `
        -Path $exportedclientcertpath, $exportedrootcertpath, "$vpnTemp\vpnclientconfiguration.zip" `
        -Destination $vpnTemp `
        -ToSession $session

    Invoke-Command `
        -Session $session `
        -ArgumentList `
            $mypwd, `
            $vpnTemp, `
            $virtualNetworkName
        -ScriptBlock { 
            $mypwd = $args[0] 
            $vpnTemp = $args[1]
            $virtualNetworkName = $args[2]

            Import-PfxCertificate `
                -Exportable `
                -Password $mypwd `
                -CertStoreLocation "Cert:\LocalMachine\My" `
                -FilePath "$vpnTemp\P2SClientCert.pfx" | Out-Null

            Import-Certificate `
                -FilePath "$vpnTemp\P2SRootCert.cer" `
                -CertStoreLocation "Cert:\LocalMachine\Root" | Out-Null

            Expand-Archive `
                -Path "$vpnTemp\vpnclientconfiguration.zip" `
                -DestinationPath "$vpnTemp\vpnclientconfiguration"
            $vpnGeneric = "$vpnTemp\vpnclientconfiguration\Generic"

            $vpnProfile = ([xml](Get-Content -Path "$vpnGeneric\VpnSettings.xml")).VpnProfile

            Add-VpnConnection `
                -Name $virtualNetworkName `
                -ServerAddress $vpnProfile.VpnServer ` 
                -TunnelType Ikev2 `
                -EncryptionLevel Required `
                -AuthenticationMethod MachineCertificate `
                -SplitTunneling `
                -AllUserConnection

            Add-VpnConnectionRoute `
                -Name $virtualNetworkName `
                -DestinationPrefix $vpnProfile.Routes `
                -AllUserConnection

            Add-VpnConnectionRoute `
                -Name $virtualNetworkName `
                -DestinationPrefix $vpnProfile.VpnClientAddressPool `
                -AllUserConnection

            rasdial $virtualNetworkName
        }
}

Remove-Item -Path $vpnTemp -Recurse
```

## <a name="mount-azure-file-share"></a>Einbinden einer Azure-Dateifreigabe
Nun, da Sie Ihr P2S-VPN eingerichtet haben, können Sie es zum Einbinden der Azure-Dateifreigabe auf den Computern verwenden, die Sie mithilfe von PowerShell eingerichtet haben. Das folgende Beispiel bindet die Freigabe ein und stellt durch eine Auflistung des Stammverzeichnisses der Freigabe sicher, dass die Freigabe tatsächlich eingebunden wurde. Anschließend wird die Einbindung der Freigabe aufgehoben. Leider ist es nicht möglich, die Freigabe über eine PowerShell-Remotesitzung dauerhaft einzubinden. Informationen für eine dauerhafte Einbindung finden Sie unter [Verwenden einer Azure-Dateifreigabe mit Windows](storage-how-to-use-files-windows.md). 

```PowerShell
$myShareToMount = "<file-share>"

$storageAccountKeys = Get-AzStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName
$storageAccountKey = ConvertTo-SecureString `
    -String $storageAccountKeys[0].Value `
    -AsPlainText `
    -Force

$nic = Get-AzNetworkInterface -ResourceId $privateEndpoint.NetworkInterfaces[0].Id
$storageAccountPrivateIP = $nic.IpConfigurations[0].PrivateIpAddress

Invoke-Command `
    -Session $sessions `
    -ArgumentList  `
        $storageAccountName, `
        $storageAccountKey, `
        $storageAccountPrivateIP, `
        $myShareToMount `
    -ScriptBlock {
        $storageAccountName = $args[0]
        $storageAccountKey = $args[1]
        $storageAccountPrivateIP = $args[2]
        $myShareToMount = $args[3]

        $credential = [System.Management.Automation.PSCredential]::new(
            "AZURE\$storageAccountName", 
            $storageAccountKey)

        New-PSDrive `
            -Name Z `
            -PSProvider FileSystem `
            -Root "\\$storageAccountPrivateIP\$myShareToMount" `
            -Credential $credential `
            -Persist | Out-Null
        Get-ChildItem -Path Z:\
        Remove-PSDrive -Name Z
    }
```

## <a name="see-also"></a>Weitere Informationen
- [Netzwerküberlegungen für den direkten Zugriff auf Azure-Dateifreigaben](storage-files-networking-overview.md)
- [Konfigurieren eines P2S-VPN (Point-to-Site) unter Linux zur Verwendung mit Azure Files](storage-files-configure-p2s-vpn-linux.md)
- [Konfigurieren eines S2S-VPN (Site-to-Site) zur Verwendung mit Azure Files](storage-files-configure-s2s-vpn.md)