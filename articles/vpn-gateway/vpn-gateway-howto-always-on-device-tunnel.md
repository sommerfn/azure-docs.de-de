---
title: Konfigurieren eines Always On-VPN-Tunnels für VPN Gateway
description: Schritte zum Konfigurieren eines Always On-VPN-Tunnels für VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: cherylmc
ms.openlocfilehash: bc2ec2b952b4f0c6e61fc4953559fa882edfff09
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71841137"
---
# <a name="configure-an-always-on-vpn-device-tunnel"></a>Konfigurieren eines Always On-VPN-Gerätetunnels

Eines der neuen Features des Windows 10 Virtual Private Network (VPN)-Clients ist die Möglichkeit, eine VPN-Verbindung aufrechtzuerhalten. Always On ist ein Windows 10-Feature, das es dem aktiven VPN-Profil ermöglicht, basierend auf Triggern (Benutzeranmeldung, Änderung des Netzwerkzustands oder aktiver Gerätebildschirm) automatisch eine Verbindung herzustellen und aufrechtzuerhalten.

Die virtuellen Azure-Netzwerkgateways können unter Windows 10 Always On verwendet werden, um persistente Benutzertunnel sowie Gerätetunnel für Azure einzurichten. Mithilfe dieses Artikels können Sie einen Always On-VPN-Gerätetunnel konfigurieren.

Always On-VPN-Verbindungen umfassen zwei Typen von Tunneln:

* Ein **Gerätetunnel** verbindet sich mit bestimmten VPN-Servern, bevor sich Benutzer am Gerät anmelden. Ein Gerätetunnel wird für Verbindungsszenarios vor der Anmeldung und zur Geräteverwaltung verwendet.

* Ein **Benutzertunnel** stellt erst eine Verbindung her, nachdem sich der Benutzer am Gerät angemeldet hat. Mit dem Benutzertunnel können Benutzer über VPN-Server auf Organisationsressourcen zugreifen.

Sowohl Gerätetunnel als auch Benutzertunnel arbeiten unabhängig voneinander mit ihren VPN-Profilen. Sie können gleichzeitig verbunden werden und können verschiedene Authentifizierungsmethoden und andere VPN-Konfigurationseinstellungen verwenden.

## <a name="1-configure-the-gateway"></a>1. Konfigurieren des Gateways

Mithilfe dieses [Artikels zu Point-to-Site-Verbindungen](vpn-gateway-howto-point-to-site-resource-manager-portal.md) konfigurieren Sie das VPN-Gateway für die Verwendung von IKEv2 und der zertifikatbasierten Authentifizierung.

## <a name="2-configure-the-device-tunnel"></a>2. Konfigurieren des Gerätetunnels

Um einen Gerätetunnel einzurichten, müssen die folgenden Anforderungen erfüllt werden:

* Das Gerät muss ein in die Domäne eingebundener Computer mit Windows 10 Enterprise oder Education Version 1709 oder höher sein.
* Der Tunnel ist nur für die unter Windows integrierte VPN-Lösung konfigurierbar und wird mit IKEv2 mit Computerzertifikatsauthentifizierung eingerichtet. 
* Pro Gerät kann nur ein Gerätetunnel konfiguriert werden.

1. Installieren Sie Clientzertifikate auf dem Windows 10-Client wie in diesem [Artikel zum Point-to Site-VPN-Client](point-to-site-how-to-vpn-client-install-azure-cert.md) gezeigt. Das Zertifikat muss sich im Speicher „Lokaler Computer“ befinden.
1. Verwenden Sie [diese Anweisungen](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config#vpn-device-tunnel-configuration), um ein VPN-Profil zu erstellen und den Gerätetunnel im Kontext des lokalen Systemkontos zu konfigurieren.

### <a name="configuration-example-for-device-tunnel"></a>Beispiel für die Konfiguration eines Gerätetunnels

Nachdem Sie das virtuelle Netzwerkgateway konfiguriert und das Clientzertifikat im Speicher „Lokaler Computer“ auf dem Windows 10-Client installiert haben, verwenden Sie die folgenden Beispiele, um einen Clientgerätetunnel zu konfigurieren.

1. Kopieren Sie den folgenden Text, und speichern Sie ihn als ***devicecert.ps1***-Datei.

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
1. Kopieren Sie den folgenden Text, und speichern Sie ihn als ***VPNProfile.xml***-Datei im gleichen Ordner wie die **devicecert.ps1**-Datei. Bearbeiten Sie den folgenden Text, um ihn an Ihre Umgebung anzupassen.

   * `<Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>`
   * `<Address>192.168.3.5</Address>`
   * `<Address>192.168.3.4</Address>`

   ```
   <VPNProfile>  
     <NativeProfile>  
   <Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>  
   <NativeProtocolType>IKEv2</NativeProtocolType>  
   <Authentication>  
     <MachineMethod>Certificate</MachineMethod>  
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
   <!-- need to specify always on = true --> 
     <AlwaysOn>true</AlwaysOn> 
   <!-- new node to specify that this is a device tunnel -->  
    <DeviceTunnel>true</DeviceTunnel>
   <!--new node to register client IP address in DNS to enable manage out -->
   <RegisterDNS>true</RegisterDNS>
   </VPNProfile>
   ```
1. Laden Sie **PsExec** von [Sysinternals](https://docs.microsoft.com/sysinternals/downloads/psexec) herunter, und extrahieren Sie die Dateien in **C:\PSTools**.
1. Starten Sie PowerShell über eine Administrator-CMD-Eingabeaufforderung, indem Sie Folgendes ausführen:

   ```
   C:\PsTools\PsExec.exe Powershell for 32-bit Windows
   C:\PsTools\PsExec64.exe Powershell for 64-bit Windows
   ```

   ![powershell](./media/vpn-gateway-howto-always-on-device-tunnel/powershell.png)
1. Wechseln Sie in PowerShell zum Ordner, in dem sich die **devicecert.ps1**- und **VPNProfile.xml**-Datei befinden, und führen Sie den folgenden Befehl aus:

   ```powershell
   C:\> .\devicecert.ps1 .\VPNProfile.xml MachineCertTest
   ```
   
   ![MachineCertTest](./media/vpn-gateway-howto-always-on-device-tunnel/machinecerttest.png)
1. Führen Sie **rasphone** aus.

   ![rasphone](./media/vpn-gateway-howto-always-on-device-tunnel/rasphone.png)
1. Suchen Sie den Eintrag **MachineCertTest**, und klicken Sie auf **Verbinden**.

   ![Verbinden](./media/vpn-gateway-howto-always-on-device-tunnel/connect.png)
1. Wenn die Verbindung erfolgreich hergestellt wurde, starten Sie den Computer neu. Der Tunnel wird automatisch eine Verbindung herstellen.

## <a name="cleanup"></a>Cleanup

Führen Sie den folgenden Befehl aus, um das Profil zu entfernen:

![Cleanup](./media/vpn-gateway-howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Problembehandlung finden Sie unter [Azure Point-to-Site-Verbindungsprobleme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
