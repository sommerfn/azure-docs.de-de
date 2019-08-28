---
title: Azure VPN Gateway in Azure Australien
description: Implementieren von VPN Gateway in Azure Australien zur Konformität mit dem ISM und zum effektiven Schutz australischer Regierungsbehörden
author: galey801
ms.service: azure-australia
ms.topic: article
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 00588042fa11ace51eef40cdedbae14c1bd99801
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69575420"
---
# <a name="azure-vpn-gateway-in-azure-australia"></a>Azure VPN Gateway in Azure Australien

Ein kritischer Dienst in jeder öffentlichen Cloud ist die sichere Verbindung von Cloudressourcen und -diensten mit vorhandenen lokalen Systemen. Der Dienst, der diese Funktionalität in Azure bereitstellt, ist Azure VPN Gateway. In diesem Artikel werden die wichtigsten Aspekte bei der Konfiguration eines VPN-Gateways beschrieben, die zur Einhaltung der [Kontrollmaßnahmen im Information Security Manual (ISM)](https://acsc.gov.au/infosec/ism/) (Handbuch zur Informationssicherheit) des Australian Signals Directorate (ASD) beachtet werden müssen.

Ein VPN-Gateway wird zum Senden von verschlüsseltem Datenverkehr zwischen einem virtuellen Netzwerk in Azure und einem anderen Netzwerk verwendet. VPN-Gateways werden für drei Szenarien verwendet:

- Site-to-Site (S2S)
- Point-to-Site (P2S)
- Netzwerk-zu-Netzwerk

Dieser Artikel konzentriert sich auf S2S-VPN-Gateways. In Abbildung 1 ist ein Beispiel für die Konfiguration eines S2S-VPN-Gateways dargestellt.

![VPN-Gateway mit Verbindungen zwischen mehreren Standorten](media/vpngateway-multisite-connection-diagram.png)

*Abbildung 1: Azure S2S-VPN-Gateway*

## <a name="key-design-considerations"></a>Wichtige Entwurfsaspekte

Es gibt drei Netzwerkoptionen zum Verbinden von Azure mit Kunden der australischen Regierung:

- ICON
- Azure ExpressRoute
- Öffentliches Internet

Im [Consumer Guide for Azure](https://servicetrust.microsoft.com/viewpage/Australia) des Australian Cyber Security Centre wird empfohlen, dass VPN Gateway (oder ein gleichwertiger Drittanbieterdienst mit Zertifizierung für PROTECTED) zusammen mit den drei Netzwerkoptionen verwendet wird. Diese Empfehlung dient der Sicherstellung, dass die Verbindungen den ISM-Kontrollen zur Verschlüsselung und Integrität entsprechen.

### <a name="encryption-and-integrity"></a>Verschlüsselung und Integrität

Standardmäßig handelt das VPN die Algorithmen und Parameter für die Verschlüsselung und Integrität bei der Verbindungsherstellung als Teil der IKE-Handshakes aus. Während des IKE-Handshakes hängen die Konfiguration und die Präferenzreihenfolge davon ab, ob es sich bei dem VPN-Gateway um den Initiator oder den Antwortdienst handelt. Die Bestimmung wird über das VPN-Gerät gesteuert. Die endgültige Konfiguration der Verbindung wird über die Konfiguration des VPN-Geräts gesteuert. Weitere Informationen zu überprüften VPN-Geräten und deren Konfiguration finden Sie unter [Informationen zu VPN-Diensten](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).

VPN-Gateways können die Verschlüsselung und Integrität steuern, indem eine benutzerdefinierte IPsec/IKE-Richtlinie für die Verbindung konfiguriert wird.

### <a name="resource-operations"></a>Ressourcenvorgänge

VPN-Gateways erstellen über das öffentliche Internet eine Verbindung zwischen Azure- und Nicht-Azure-Umgebungen. Das ISM beinhaltet Kontrollen, die sich auf die explizite Autorisierung von Verbindungen beziehen. Standardmäßig ist es möglich, VPN-Gateways zum Erstellen von nicht autorisierten Tunneln in sicheren Umgebungen zu verwenden. Es ist wichtig, dass Organisationen über die rollenbasierte Zugriffssteuerung (RBAC) in Azure steuern, wer VPN-Gateways und die zugehörigen Verbindungen erstellen und ändern kann. Azure besitzt keine „integrierte“ Rolle zum Verwalten von VPN-Gateways, weshalb eine benutzerdefinierte Rolle erforderlich ist.

Der Zugriff auf die Rollen „Besitzer“, „Mitwirkender“ und „Netzwerkmitwirkender“ wird streng kontrolliert. Außerdem wird empfohlen, dass Sie Azure Active Directory Privileged Identity Management für eine differenziertere Zugriffssteuerung verwenden.

### <a name="high-availability"></a>Hochverfügbarkeit

Azure-VPN-Gateways können über mehrere Verbindungen verfügen und unterstützen mehrere lokale VPN-Geräte für die gleiche lokale Umgebung. Siehe Abbildung 1.

Virtuelle Netzwerke in Azure können über mehrere VPN-Gateways verfügen, die unabhängig, in Aktiv/Passiv- oder in Aktiv/Aktiv-Konfigurationen bereitgestellt werden können.

Es wird empfohlen, alle VPN-Gateways in einer [Hochverfügbarkeitskonfiguration](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable) bereitzustellen. Ein Beispiel sind zwei lokale VPN-Geräte, die mit zwei VPN-Gateways im Aktiv/Passiv- oder Aktiv/Aktiv-Modus verbunden sind. Siehe Abbildung 2.

![Redundante Verbindungen des VPN-Gateways](media/dual-redundancy.png)

*Abbildung 2: Aktiv/Aktiv-VPN-Gateways und zwei VPN-Geräte*

### <a name="forced-tunneling"></a>Tunnelerzwingung

Durch die Tunnelerzwingung wird die Zurückleitung des gesamten Internetdatenverkehrs zur lokalen Umgebung über das VPN-Gateway zur Überprüfung und Überwachung erzwungen. Ohne die Tunnelerzwingung wird der Internetdatenverkehr von virtuellen Computern in Azure direkt von der Azure-Netzwerkinfrastruktur an das öffentliche Internet geleitet, ohne die Möglichkeit, den Datenverkehr zu überprüfen oder zu überwachen. Tunnelerzwingung ist jedoch wichtig, wenn eine Organisation ein Secure Internet Gateway (SIG) für eine Umgebung verwenden muss.

## <a name="detailed-configuration"></a>Ausführliche Konfiguration

### <a name="service-attributes"></a>Dienstattribute

VPN-Gateways für S2S-Verbindungen, die für die australische Regierung konfiguriert sind, müssen über folgende Attribute verfügen:

|Attribut | Erforderlich|
|--- | --- |
|gatewayType | „VPN“|
|

Folgende Attributeinstellungen sind zur Einhaltung der ISM-Kontrollen für den SCHUTZ (PROTECTED) erforderlich:

|Attribut | Erforderlich|
|--- |---|
|vpnType |„RouteBased“|
|vpnClientConfiguration/vpnClientProtocols | „IkeV2“|
|

Azure VPN-Gateways unterstützen ein Spektrum von Kryptografiealgorithmen aus den IPsec- und IKE-Protokollstandards. Die Standardrichtlinie legt maximale Interoperabilität mit einer Vielzahl von VPN-Geräten von Drittanbietern fest. Somit ist es möglich, dass während des IKE-Handshakes eine nicht konforme Konfiguration ausgehandelt wird. Es wird daher dringend empfohlen, dass [benutzerdefinierte IPsec/IKE-Richtlinienparameter](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-ipsecikepolicy-rm-powershell) auf „vpnClientConfiguration“ in VPN-Gateways angewandt werden, um sicherzustellen, dass die Verbindungen den ISM-Kontrollen für Verbindungen der lokalen Umgebung mit Azure entsprechen. Die folgende Tabelle enthält die Schlüsselattribute.

|Attribut|Optional|Erforderlich|
|---|---|---|
|saLifeTimeSeconds|< 14.400 s|> 300 s|
|saDataSizeKilobytes| |> 1.024 KB|
|ipsecEncryption| |AES256-GCMAES256|
|ipsecIntegrity| |SHA256-GCMAES256|
|ikeEncryption| |AES256-GCMAES256|
|ikeIntegrity| |SHA256-GCMAES256|
|dhGroup|DHGroup14, DHGroup24, ECP256, ECP384|DHGroup2|
|pfsGroup|PFS2048, PFS24, ECP256, ECP384||
|

Für dhGroup und pfsGroup in der vorherigen Tabelle werden ECP256 und ECP384 bevorzugt, auch wenn andere Einstellungen verwendet werden können.

### <a name="related-services"></a>Verknüpfte Dienste

Beim Entwerfen und Konfigurieren einer Azure VPN Gateway-Instanz müssen verschiedene zugehörige Dienste auch vorhanden sein und konfiguriert werden.

|Dienst | Erforderliche Aktion|
|--- | ---|
|Virtuelles Netzwerk | VPN-Gateways sind an ein virtuelles Netzwerk angefügt. Erstellen Sie ein virtuelles Netzwerk, bevor Sie einen neuen VPN-Gateway erstellen.|
|Öffentliche IP-Adresse | S2S-VPN-Gateways benötigen eine öffentliche IP-Adresse, um eine Verbindung zwischen dem lokalen VPN-Gerät und dem VPN-Gateway herzustellen. Erstellen Sie eine öffentliche IP-Adresse, bevor Sie ein S2S-VPN-Gateway erstellen.|
|Subnet | Erstellen Sie ein Subnetz des virtuellen Netzwerks für das VPN-Gateway.|
|

## <a name="implementation-steps-using-powershell"></a>Implementierungsschritte mithilfe von PowerShell

### <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

1. Dient zum Erstellen einer benutzerdefinierten Rolle. Ein Beispiel ist der „virtualNetworkGateway“-Mitwirkende. Erstellen Sie eine Rolle, die Benutzern zugewiesen werden soll, die VPN-Gateways erstellen und ändern dürfen. Die benutzerdefinierte Rolle sollte die folgenden Vorgänge zulassen:

   Microsoft.Network/virtualNetworkGateways/*  
   Microsoft.Network/connections/*  
   Microsoft.Network/localnetworkgateways/*  
   Microsoft.Network/virtualNetworks/subnets/*  
   Microsoft.Network/publicIPAddresses/*  
   Microsoft.Network/publicIPPrefixes/*  
   Microsoft.Network/routeTables/*  

2. Fügen Sie Benutzern, die berechtigt sind, VPN-Gateways und Verbindungen mit lokalen Umgebungen zu erstellen und zu verwalten, die benutzerdefinierte Rolle hinzu.

### <a name="create-a-vpn-gateway"></a>Erstellen eines VPN-Gateways

Bei diesen Schritten wird davon ausgegangen, dass Sie bereits ein virtuelles Netzwerk erstellt haben.

1. Erstellen einer neuen öffentlichen IP-Adresse.
2. Erstellen eines Subnetzes für das VPN-Gateway.
3. Erstellen einer IP-Konfigurationsdatei für das VPN-Gateway.
4. Erstellen eines VPN-Gateways.
5. Erstellen eines lokalen Netzwerkgateways für das lokale VPN-Gerät.
6. Erstellen einer IPsec-Richtlinie. Dieser Schritt setzt voraus, dass Sie benutzerdefinierte IPsec/IKE-Richtlinien verwenden.
7. Erstellen Sie eine Verbindung zwischen dem VPN Gateway und einem lokalen Netzwerkgateway mithilfe der IPsec-Richtlinie.

### <a name="enforce-tunneling"></a>Erzwingen von Tunneling

Wenn Tunnelerzwingung erforderlich ist, vor dem Erstellen des VPN-Gateways:

1. Erstellen einer Routingtabelle und von Routingregeln.
2. Zuordnen einer Routingtabelle zu den entsprechenden Subnetzen.

Nachdem Sie das VPN-Gateway erstellt haben:

- Festlegen von „GatewayDefaultSite“ auf die lokale Umgebung für das VPN-Gateway.

### <a name="example-powershell-script"></a>PowerShell-Beispielskript

PowerShell-Beispielskript zum Erstellen einer benutzerdefinierten IPsec/IKE-Richtlinie, die den ISM-Kontrollen für die australische Sicherheitsklassifizierung PROTECTED entspricht.

Es wird vorausgesetzt, dass das virtuelle Netzwerk, das VPN-Gateway und lokale Gateways vorhanden sind.

#### <a name="create-an-ipsecike-policy"></a>Erstellen Sie eine IPsec/IKE-Richtlinie.

Im folgenden Beispielskript wird eine IPsec/IKE-Richtlinie mit den folgenden Algorithmen und Parametern erstellt:

- IKEv2: AES256, SHA256, DHGroup ECP256
- IPsec: AES256, SHA256, PFS ECP256, SA-Gültigkeitsdauer von 14.400 Sekunden und 102.400.000 KB

```powershell
$custompolicy = New-AzIpsecPolicy `
                    -IkeEncryption AES256 `
                    -IkeIntegrity SHA256 `
                    -DhGroup ECP256 `
                    -IpsecEncryption AES256 `
                    -IpsecIntegrity SHA256 `
                    -PfsGroup ECP256 `
                    -SALifeTimeSeconds 14400 `
                    -SADataSizeKilobytes 102400000
```

#### <a name="create-a-s2s-vpn-connection-with-the-custom-ipsecike-policy"></a>Erstellen einer S2S-VPN-Verbindung mit der benutzerdefinierten IPsec/IKE-Richtlinie

```powershell
$vpngw = Get-AzVirtualNetworkGateway `
                    -Name "<yourVPNGatewayName>" `
                    -ResourceGroupName "<yourResourceGroupName>"
$localgw = Get-AzLocalNetworkGateway  `
                    -Name "<yourLocalGatewayName>" `
                    -ResourceGroupName "<yourResourceGroupName>"

New-AzVirtualNetworkGatewayConnection `
                    -Name "ConnectionName" `
                    -ResourceGroupName "<yourResourceGroupName>" `
                    -VirtualNetworkGateway1 $vpngw `
                    -LocalNetworkGateway2 $localgw `
                    -Location "Australia Central" `
                    -ConnectionType IPsec `
                    -IpsecPolicies $custompolicy `
                    -SharedKey "AzureA1b2C3"
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde die spezifische Konfiguration von VPN Gateway behandelt, um die Anforderungen zu erfüllen, die im Information Security Manual zum Schützen von Daten der australischen Regierung des Typs PROTECTED während der Übertragung angegeben sind. Schritte und Vorgehensweisen zum Konfigurieren des VPN-Gateways finden Sie unter:

- [Übersicht über Azure VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/)  
- [Was ist VPN Gateway?](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)  
- [Erstellen eines virtuellen Netzwerks mit einer Site-to-Site-VPN-Verbindung mit PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell)  
- [Erstellen und Verwalten eines VPN-Gateways](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-tutorial-create-gateway-powershell)