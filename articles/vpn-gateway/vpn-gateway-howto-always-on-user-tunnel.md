---
title: Konfigurieren eines Always On-VPN-Benutzertunnels für Ihr VPN-Gateway
description: In diesem Artikel wird beschrieben, wie Sie einen Always On-VPN-Benutzertunnel für Ihr VPN-Gateway konfigurieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: cherylmc
ms.openlocfilehash: 9aa1f951add5b79eab12f4957be05a42bbdd4434
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72299931"
---
# <a name="configure-an-always-on-vpn-user-tunnel"></a>Konfigurieren eines Always On-VPN-Benutzertunnels

Ein neues Feature des Windows 10-VPN-Clients namens Always On bietet die Möglichkeit, eine VPN-Verbindung aufrechtzuerhalten. Mit Always On kann das aktive VPN-Profil basierend auf Triggern wie Benutzeranmeldung, Änderung des Netzwerkzustands oder aktiver Gerätebildschirm automatisch eine Verbindung herstellen und aufrechterhalten.

Mithilfe von virtuellen Azure-Netzwerkgateways können Sie unter Windows 10 Always On persistente Benutzertunnel und Gerätetunnel für Azure einzurichten. In diesem Artikel erfahren Sie, wie Sie einen Always On-VPN-Benutzertunnel konfigurieren.

Always On-VPN-Verbindungen umfassen zwei Typen von Tunneln:

* **Gerätetunnel**: Stellt eine Verbindung mit bestimmten VPN-Servern her, bevor sich Benutzer am Gerät anmelden. Ein Gerätetunnel wird für Verbindungsszenarios vor der Anmeldung und zur Geräteverwaltung verwendet.

* **Benutzertunnel**: Stellt erst dann eine Verbindung her, nachdem sich Benutzer am Gerät angemeldet haben. Mithilfe von Benutzertunneln können Sie über VPN-Server auf Organisationsressourcen zugreifen.

Gerätetunnel und Benutzertunnel arbeiten unabhängig von ihren VPN-Profilen. Sie können gleichzeitig verbunden werden und können verschiedene Authentifizierungsmethoden und andere VPN-Konfigurationseinstellungen verwenden.

In den folgenden Abschnitten konfigurieren Sie ein VPN-Gateway und einen Benutzertunnel.

## <a name="step-1-configure-a-vpn-gateway"></a>Schritt 1: Konfigurieren eines VPN-Gateways

Konfigurieren Sie anhand der Anleitungen im Artikel zu [Point-to-Site-Verbindungen](vpn-gateway-howto-point-to-site-resource-manager-portal.md) das VPN-Gateway für die Verwendung von IKEv2 und der zertifikatbasierten Authentifizierung.

## <a name="step-2-configure-a-user-tunnel"></a>Schritt 2: Konfigurieren eines Benutzertunnels

1. Installieren Sie Clientzertifikate auf dem Windows 10-Client wie in diesem Artikel zum [Point-to Site-VPN-Client](point-to-site-how-to-vpn-client-install-azure-cert.md) gezeigt. Das Zertifikat muss sich im Speicher des aktuellen Benutzers befinden.

1. Konfigurieren Sie den Always On-VPN-Client über PowerShell, System Center Configuration Manager oder Intune, indem Sie den Anweisungen unter [Konfigurieren von Always On-VPN-Verbindungen für den Windows 10-Client](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections) folgen.

### <a name="example-configuration-for-the-user-tunnel"></a>Beispielkonfiguration für den Benutzertunnel

Nachdem Sie das virtuelle Netzwerkgateway konfiguriert und das Clientzertifikat im Speicher des lokalen Computers auf dem Windows 10-Client installiert haben, konfigurieren Sie einen Clientgerätetunnel anhand der folgenden Beispiele:

1. Kopieren Sie den folgenden Text, und speichern Sie ihn als *usercert.ps1*:

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
1. Kopieren Sie den folgenden Text, und speichern Sie ihn als *VPNProfile.xml* im gleichen Ordner wie *usercert.ps1*. Bearbeiten Sie den folgenden Text, um ihn an Ihre Umgebung anzupassen:

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

1. Wechseln Sie in PowerShell zu dem Ordner, in dem sich die *usercert.ps1*- und *VPNProfile.xml*-Datei befinden, und führen Sie den folgenden Befehl aus:

   ```powershell
   C:\> .\usercert.ps1 .\VPNProfile.xml UserTest
   ```
   
   ![MachineCertTest](./media/vpn-gateway-howto-always-on-user-tunnel/p2s2.jpg)
1. Suchen Sie unter **VPN-Einstellungen** nach dem Eintrag **UserTest** , und klicken Sie auf **Verbinden**.

1. Wenn die Verbindung hergestellt wird, haben Sie erfolgreich einen Always On-Benutzertunnel konfiguriert.

## <a name="clean-up-your-resources"></a>Bereinigen von Ressourcen

Zum Entfernen des Profils gehen Sie wie folgt vor:

1. Führen Sie den folgenden Befehl aus:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Trennen Sie die Verbindung, und deaktivieren Sie das Kontrollkästchen **Automatisch verbinden**.

![Cleanup](./media/vpn-gateway-howto-always-on-user-tunnel/p2s4..jpg)

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Beheben möglicher Verbindungsprobleme finden Sie unter [Azure Point-to-Site-Verbindungsprobleme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
