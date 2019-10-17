---
title: Konfigurieren eines Always On-VPN-Tunnels für VPN Gateway
description: Schritte zum Konfigurieren eines Always On-VPN-Benutzertunnels für VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: cherylmc
ms.openlocfilehash: dc0abf12c60f845fde0d16bd874a1436aef3b7ab
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71846374"
---
# <a name="configure-an-always-on-vpn-user-tunnel"></a>Konfigurieren eines Always On-VPN-Benutzertunnels

Eines der neuen Features des Windows 10 Virtual Private Network (VPN)-Clients ist die Möglichkeit, eine VPN-Verbindung aufrechtzuerhalten. Always On ist ein Windows 10-Feature, das es dem aktiven VPN-Profil ermöglicht, basierend auf Triggern (Benutzeranmeldung, Änderung des Netzwerkzustands oder aktiver Gerätebildschirm) automatisch eine Verbindung herzustellen und aufrechtzuerhalten.

Die virtuellen Azure-Netzwerkgateways können unter Windows 10 Always On verwendet werden, um persistente Benutzertunnel sowie Gerätetunnel für Azure einzurichten. Mithilfe dieses Artikels können Sie einen Always On-VPN-Benutzertunnel konfigurieren.

Always On-VPN-Verbindungen umfassen zwei Typen von Tunneln:

* Ein **Gerätetunnel** verbindet sich mit bestimmten VPN-Servern, bevor sich Benutzer am Gerät anmelden. Ein Gerätetunnel wird für Verbindungsszenarios vor der Anmeldung und zur Geräteverwaltung verwendet.

* Ein **Benutzertunnel** stellt erst eine Verbindung her, nachdem sich der Benutzer am Gerät angemeldet hat. Mit dem Benutzertunnel können Benutzer über VPN-Server auf Organisationsressourcen zugreifen.

Sowohl Gerätetunnel als auch Benutzertunnel arbeiten unabhängig voneinander mit ihren VPN-Profilen. Sie können gleichzeitig verbunden werden und können verschiedene Authentifizierungsmethoden und andere VPN-Konfigurationseinstellungen verwenden.

## <a name="1-configure-the-gateway"></a>1. Konfigurieren des Gateways

Mithilfe dieses [Artikels zu Point-to-Site-Verbindungen](vpn-gateway-howto-point-to-site-resource-manager-portal.md) konfigurieren Sie das VPN-Gateway für die Verwendung von IKEv2 und der zertifikatbasierten Authentifizierung.

## <a name="2-configure-the-user-tunnel"></a>2. Konfigurieren des Benutzertunnels

1. Installieren Sie Clientzertifikate auf dem Windows 10-Client wie in diesem [Artikel zum Point-to Site-VPN-Client](point-to-site-how-to-vpn-client-install-azure-cert.md) gezeigt. Das Zertifikat muss sich im Speicher „Aktueller Benutzer“ befinden.
2. Anhand [dieser Anweisungen](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections) können Sie den Always On-VPN-Client über PowerShell, SCCM oder Intune konfigurieren.

### <a name="configuration-example-for-user-tunnel"></a>Beispiel für die Konfiguration eines Benutzertunnels

Nachdem Sie das virtuelle Netzwerkgateway konfiguriert und das Clientzertifikat im Speicher „Lokaler Computer“ auf dem Windows 10-Client installiert haben, verwenden Sie die folgenden Beispiele, um einen Clientgerätetunnel zu konfigurieren.

1. Kopieren Sie den folgenden Text, und speichern Sie ihn als ***usercert.ps1***.

   ```
   Param(
   [string]$xmlFilePath,
   [string]$ProfileName
   )

   $a = Test-Path $xmlFilePath
   echo $a

   $ProfileXML = Get-Content $xmlFilePath

   echo $XML

   $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

   $Version = 201606090004

   $ProfileXML = $ProfileXML -replace '<', '&lt;'
   $ProfileXML = $ProfileXML -replace '>', '&gt;'
   $ProfileXML = $ProfileXML -replace '"', '&quot;'

   $nodeCSPURI = './Vendor/MSFT/VPNv2'
   $namespaceName = "root\cimv2\mdm\dmmap"
   $className = "MDM_VPNv2_01"

   $session = New-CimSession

   try
   {
   $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
   $newInstance.CimInstanceProperties.Add($property)

   $session.CreateInstance($namespaceName, $newInstance)
   $Message = "Created $ProfileName profile."
   Write-Host "$Message"
   }
   catch [Exception]
   {
   $Message = "Unable to create $ProfileName profile: $_"
   Write-Host "$Message"
   exit
   }
   $Message = "Complete."
   Write-Host "$Message"
   ```
1. Kopieren Sie den folgenden Text, und speichern Sie ihn als ***VPNProfile.xml***-Datei im gleichen Ordner wie die **usercert.ps1**-Datei. Bearbeiten Sie den folgenden Text, um ihn an Ihre Umgebung anzupassen.

   * `<Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>`
   * `<Address>192.168.3.5</Address>`
   * `<Address>192.168.3.4</Address>`

   ```
    <VPNProfile>  
      <NativeProfile>  
    <Servers>azuregateway-b115055e-0882-49bc-a9b9-7de45cba12c0-8e6946892333.vpn.azure.com</Servers>  
    <NativeProtocolType>IKEv2</NativeProtocolType>  
    <Authentication>  
    <UserMethod>Eap</UserMethod>
    <Eap>
    <Configuration>
    <EapHostConfig xmlns="http://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="http://www.microsoft.com/provisioning/EapCommon">13</Type><VendorId xmlns="http://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="http://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="http://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="http://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="http://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>false</DisableUserPromptForServerValidation><ServerNames></ServerNames></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</PerformServerValidation><AcceptServerName xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</AcceptServerName></EapType></Eap></Config></EapHostConfig>
    </Configuration>
    </Eap>
    </Authentication>  
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>  
     <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
    <DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>  
      </NativeProfile> 
      <!-- use host routes(/32) to prevent routing conflicts -->  
      <Route>  
    <Address>192.168.3.5</Address>  
    <PrefixSize>32</PrefixSize>  
      </Route>  
      <Route>  
    <Address>192.168.3.4</Address>  
    <PrefixSize>32</PrefixSize>  
      </Route>  
    <!-- traffic filters for the routes specified above so that only this traffic can go over the device tunnel --> 
      <TrafficFilter>  
    <RemoteAddressRanges>192.168.3.4, 192.168.3.5</RemoteAddressRanges>  
      </TrafficFilter>
    <!-- need to specify always on = true --> 
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <!--new node to register client IP address in DNS to enable manage out -->
    <RegisterDNS>true</RegisterDNS>
    </VPNProfile>
   ```
1. Führen Sie PowerShell als Administrator aus.

1. Wechseln Sie in PowerShell zu dem Ordner, in dem sich die **usercert.ps1**- und **VPNProfile.xml**-Datei befinden, und führen Sie den folgenden Befehl aus:

   ```powershell
   C:\> .\usercert.ps1 .\VPNProfile.xml UserTest
   ```
   
   ![MachineCertTest](./media/vpn-gateway-howto-always-on-user-tunnel/p2s2.jpg)
1. Suchen Sie unter „VPN-Einstellungen“.

1. Suchen Sie den Eintrag **UserTest**, und klicken Sie auf **Verbinden**.

1. Wenn die Verbindung hergestellt wird, haben Sie erfolgreich einen Always On-Benutzertunnel konfiguriert.

## <a name="cleanup"></a>Cleanup

Führen Sie den folgenden Befehl aus, um das Profil zu entfernen:

1. Trennen Sie die Verbindung, und deaktivieren Sie „Automatisch verbinden“.

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

![Cleanup](./media/vpn-gateway-howto-always-on-user-tunnel/p2s4..jpg)

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Problembehandlung finden Sie unter [Azure Point-to-Site-Verbindungsprobleme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
