---
title: 'Tutorial: Bereitstellen und Konfigurieren von Azure Firewall in einem Hybridnetzwerk über das Azure-Portal'
description: In diesem Tutorial erfahren Sie, wie Sie Azure Firewall über das Azure-Portal bereitstellen und konfigurieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 11/02/2019
ms.author: victorh
customer intent: As an administrator, I want to control network access from an on-premises network to an Azure virtual network.
ms.openlocfilehash: 4a4fd2f89bc662f394b59aa6295c3a909cb8552b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73468468"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-in-a-hybrid-network-using-the-azure-portal"></a>Tutorial: Bereitstellen und Konfigurieren von Azure Firewall in einem Hybridnetzwerk über das Azure-Portal

Wenn Sie Ihr lokales Netzwerk mit einem virtuellen Azure-Netzwerk verbinden, um ein Hybridnetzwerk zu erstellen, ist das Steuern des Zugriffs auf Ihre Azure-Netzwerkressourcen ein wichtiger Bestandteil des globalen Sicherheitsplans.

Mit Azure Firewall können Sie den Netzwerkzugriff in einem Hybridnetzwerk mithilfe von Regeln steuern, die den zulässigen und verweigerten Netzwerkdatenverkehr definieren.

In diesem Tutorial erstellen Sie drei virtuelle Netzwerke:

- **VNet-Hub**: In diesem virtuellen Netzwerk befindet sich die Firewall.
- **VNet-Spoke**: Das virtuelle Spoke-Netzwerk stellt die Workload in Azure dar.
- **VNet-Onprem**: Das lokale virtuelle Netzwerk stellt ein lokales Netzwerk dar. Bei einer tatsächlichen Bereitstellung kann die Verbindung dafür entweder mit einer VPN- oder einer ExpressRoute-Verbindung hergestellt werden. Der Einfachheit halber wird in diesem Tutorial eine VPN-Gatewayverbindung genutzt, und ein virtuelles Netzwerk in Azure wird als lokales Netzwerk verwendet.

![Firewall in einem Hybridnetzwerk](media/tutorial-hybrid-ps/hybrid-network-firewall.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Deklarieren der Variablen
> * Erstellen des virtuellen Firewall-Hub-Netzwerks
> * Erstellen des virtuellen Spoke-Netzwerks
> * Erstellen des lokalen virtuellen Netzwerks
> * Konfigurieren und Bereitstellen der Firewall
> * Erstellen und Verbinden der VPN-Gateways
> * Durchführen des Peerings für die virtuellen Hub- und Spoke-Netzwerke
> * Erstellen der Routen
> * Erstellen der virtuellen Computer
> * Testen der Firewall

Falls Sie anstelle dieser Prozedur Azure PowerShell verwenden möchten, wechseln Sie zu [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall in einem Hybridnetzwerk mit Azure PowerShell](tutorial-hybrid-ps.md).

## <a name="prerequisites"></a>Voraussetzungen

Es gibt drei wichtige Anforderungen, die erfüllt sein müssen, damit dieses Szenario richtig funktioniert:

- Eine benutzerdefinierte Route (User Defined Route, UDR) im Spoke-Subnetz, das auf die Azure Firewall-IP-Adresse als Standardgateway verweist. Die BGP-Routenverteilung muss für diese Routentabelle auf **Deaktiviert** festgelegt werden.
- Eine UDR im Hub-Gatewaysubnetz muss auf die Firewall-IP-Adresse als nächsten Hop auf dem Weg zu den Spoke-Netzwerken verweisen.

   Für das Azure Firewall-Subnetz ist keine UDR erforderlich, da es die Routen über BGP erlernt.
- Stellen Sie sicher, dass **AllowGatewayTransit** für das Peering von VNet-Hub mit VNet-Spoke und **UseRemoteGateways** für das Peering von VNet-Spoke mit VNet-Hub festgelegt ist.

Informationen zur Erstellung dieser Routen finden Sie in diesem Tutorial im Abschnitt [Erstellen von Routen](#create-the-routes).

>[!NOTE]
>Azure Firewall muss über eine direkte Internetverbindung verfügen. Wenn Ihr Subnetz „AzureFirewallSubnet“ eine Standardroute zu Ihrem lokalen Netzwerk über BGP erfasst, müssen Sie diese mit der benutzerdefinierten Route 0.0.0.0/0 überschreiben. Legen Sie dabei den Wert **NextHopType** auf **Internet** fest, um die direkte Internetkonnektivität beizubehalten.
>
>Die Tunnelerzwingung wird derzeit von Azure Firewall nicht unterstützt. Wenn für Ihre Konfiguration die Erzwingung eines Tunnels zu einem lokalen Netzwerk erforderlich ist und Sie die Ziel-IP-Präfixe für Ihre Internetziele ermitteln können, können Sie diese Bereiche mit dem lokalen Netzwerk als nächsten Hop über eine benutzerdefinierte Route im Subnetz „AzureFirewallSubnet“ konfigurieren. Oder Sie können BGP verwenden, um diese Routen zu definieren.

>[!NOTE]
>Der Datenverkehr zwischen per direktem Peering verbundenen VNETs wird direkt weitergeleitet, auch wenn eine UDR auf Azure Firewall als Standardgateway verweist. Um in diesem Szenario Subnetz-zu-Subnetz-Datenverkehr an die Firewall zu senden, muss eine UDR explizit das Zielsubnetzwerk-Präfix in beiden Subnetzen enthalten.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-the-firewall-hub-virtual-network"></a>Erstellen des virtuellen Firewall-Hub-Netzwerks

Erstellen Sie zunächst die Ressourcengruppe für die in diesem Tutorial verwendeten Ressourcen:

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.
2. Wählen Sie auf der Startseite des Azure-Portals **Ressourcengruppen** > **Hinzufügen** aus.
3. Geben Sie unter **Ressourcengruppenname** die Zeichenfolge **FW-Hybrid-Test** ein.
4. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
5. Wählen Sie als **Region** die Option **USA, Osten** aus. Alle später erstellten Ressourcen müssen sich am gleichen Standort befinden.
6. Klicken Sie auf **Überprüfen + erstellen**.
7. Klicken Sie auf **Erstellen**.

Erstellen Sie nun das VNET:

> [!NOTE]
> Das Subnetz „AzureFirewallSubnet“ hat die Größe /26. Weitere Informationen zur Subnetzgröße finden Sie unter [Azure Firewall – Häufig gestellte Fragen](firewall-faq.md#why-does-azure-firewall-need-a-26-subnet-size).

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Wählen Sie unter **Netzwerk** die Option **Virtuelles Netzwerk** aus.
4. Geben Sie unter **Name** den Namen **VNet-hub** ein.
5. Geben Sie unter **Adressraum** den Adressraum **10.5.0.0/16** ein.
6. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
7. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe **FW-Hybrid-Test** aus.
8. Wählen Sie unter **Standort** die Option **USA, Osten** aus.
9. Geben Sie unter **Subnetz** als **Name** die Zeichenfolge **AzureFirewallSubnet** ein. Die Firewall befindet sich diesem Subnetz, und der Subnetzname **muss** „AzureFirewallSubnet“ lauten.
10. Geben Sie unter **Adressbereich** den Adressbereich **10.5.0.0/26** ein. 
11. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und wählen Sie dann **Erstellen** aus.

## <a name="create-the-spoke-virtual-network"></a>Erstellen des virtuellen Spoke-Netzwerks

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Wählen Sie unter **Netzwerk** die Option **Virtuelles Netzwerk** aus.
4. Geben Sie unter **Name** den Namen **VNet-Spoke** ein.
5. Geben Sie unter **Adressraum** den Adressraum **10.6.0.0/16** ein.
6. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
7. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe **FW-Hybrid-Test** aus.
8. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
9. Geben Sie unter **Subnetz** für **Name** den Namen **SN-Workload** ein.
10. Geben Sie unter **Adressbereich** den Adressbereich **10.6.0.0/24** ein.
11. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und wählen Sie dann **Erstellen** aus.

## <a name="create-the-on-premises-virtual-network"></a>Erstellen des lokalen virtuellen Netzwerks

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Wählen Sie unter **Netzwerk** die Option **Virtuelles Netzwerk** aus.
4. Geben Sie unter **Name** den Namen **VN-OnPrem** ein.
5. Geben Sie unter **Adressraum** die Zeichenfolge **192.168.0.0/16** ein.
6. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
7. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe **FW-Hybrid-Test** aus.
8. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
9. Geben Sie unter **Subnetz** für **Name** den Namen **SN-Corp** ein.
10. Geben Sie unter **Adressbereich** die Zeichenfolge **192.168.1.0/24** ein.
11. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und wählen Sie dann **Erstellen** aus.

Erstellen Sie nun ein zweites Subnetz für das Gateway.

1. Wählen Sie auf der Seite **VNet-Onprem** die Option **Subnetze** aus.
2. Wählen Sie **+ Subnetz** aus.
3. Geben Sie unter **Name** den Namen **GatewaySubnet** ein.
4. Geben Sie unter **Adressbereich (CIDR-Block)** den Adressbereich **192.168.2.0/24** ein.
5. Klicken Sie auf **OK**.

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

Dies ist die öffentliche IP-Adresse für das lokale Gateway.

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Geben Sie **öffentliche IP-Adresse** in das Textfeld für die Suche ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie **Öffentliche IP-Adresse** und anschließend **Erstellen** aus.
4. Geben als Name die Zeichenfolge **VNet-Onprem-GW-pip** ein.
5. Geben Sie als Ressourcengruppe die Zeichenfolge **FW-Hybrid-Test** ein.
6. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
7. Übernehmen Sie für die anderen Optionen die Standardwerte, und wählen Sie **Erstellen** aus.

## <a name="configure-and-deploy-the-firewall"></a>Konfigurieren und Bereitstellen der Firewall

Stellen Sie nun die Firewall im virtuellen Firewall-Hub-Netzwerk bereit.

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Wählen Sie in der linken Spalte **Netzwerk** und anschließend **Firewall** aus.
4. Konfigurieren Sie die Firewall auf der Seite **Firewall erstellen** anhand der folgenden Tabelle:

   |Einstellung  |Wert  |
   |---------|---------|
   |Subscription     |\<Ihr Abonnement\>|
   |Resource group     |**FW-Hybrid-Test** |
   |NAME     |**AzFW01**|
   |Location     |Wählen Sie den gleichen Standort aus wie zuvor.|
   |Virtuelles Netzwerk auswählen     |**Vorhandene verwenden**:<br> **VNet-hub**|
   |Öffentliche IP-Adresse     |Neu erstellen: <br>**Name** - **fw-pip**. |

5. Klicken Sie auf **Überprüfen + erstellen**.
6. Überprüfen Sie die Zusammenfassung, und wählen Sie dann **Erstellen** aus, um die Firewall zu erstellen.

   Die Bereitstellung dauert einige Minuten.
7. Navigieren Sie nach Abschluss der Bereitstellung zur Ressourcengruppe **FW-Hybrid-Test**, und wählen Sie die Firewall **AzFW01** aus.
8. Notieren Sie sich die private IP-Adresse. Diese wird später für die Erstellung der Standardroute benötigt.

### <a name="configure-network-rules"></a>Konfigurieren von Netzwerkregeln

Fügen Sie zunächst eine Netzwerkregel hinzu, um Webdatenverkehr zuzulassen.

1. Wählen Sie auf der Seite **AzFW01** die Option **Regeln** aus.
2. Klicken Sie auf die Registerkarte **Netzwerkregelsammlung**.
3. Wählen Sie **Netzwerkregelsammlung hinzufügen** aus.
4. Geben Sie unter **Name** den Namen **RCNet01** ein.
5. Geben Sie unter **Priorität** den Wert **100** ein.
6. Wählen Sie für **Aktion** die Option **Zulassen** aus.
6. Geben Sie unter **Regeln** für **Name** die Zeichenfolge **AllowWeb** ein.
7. Wählen Sie für **Protokoll** die Option **TCP** aus.
8. Geben Sie unter **Quelladressen** die Zeichenfolge **192.168.1.0/24** ein.
9. Geben Sie für die Zieladresse die Zeichenfolge **10.6.0.0/16** ein.
10. Geben Sie unter **Zielports** den Wert **80** ein.

Fügen Sie nun eine Regel hinzu, um RDP-Datenverkehr zuzulassen.

Geben Sie in der zweiten Regel Zeile die folgenden Informationen ein:

1. Geben Sie unter **Name** den Namen **AllowRDP** ein.
2. Wählen Sie für **Protokoll** die Option **TCP** aus.
3. Geben Sie unter **Quelladressen** die Zeichenfolge **192.168.1.0/24** ein.
4. Geben Sie für die Zieladresse die Zeichenfolge **10.6.0.0/16** ein.
5. Geben Sie unter **Zielports** den Wert **3389** ein.
6. Wählen Sie **Hinzufügen**.

## <a name="create-and-connect-the-vpn-gateways"></a>Erstellen und Verbinden der VPN-Gateways

Das virtuelle Hub-Netzwerk und das lokale virtuelle Netzwerk werden über VPN-Gateways verbunden.

### <a name="create-a-vpn-gateway-for-the-hub-virtual-network"></a>Erstellen eines VPN-Gateways für das virtuelle Hub-Netzwerk

Erstellen Sie nun das VPN-Gateway für das virtuelle Hub-Netzwerk. Netzwerk-zu-Netzwerk-Konfigurationen erfordern einen routenbasierten VPN-Typ. Häufig kann die Erstellung eines VPN-Gateways je nach ausgewählter VPN-Gateway-SKU mindestens 45 Minuten dauern.

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Geben Sie **Gateway für virtuelle Netzwerke** in das Textfeld für die Suche ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie **Gateway für virtuelle Netzwerke** und anschließend **Erstellen** aus.
4. Geben Sie unter **Name** den Namen **GW-hub** ein.
5. Wählen Sie unter **Region** die gleiche Region aus wie zuvor.
6. Wählen Sie unter **Gatewaytyp** die Option **VPN** aus.
7. Wählen Sie unter **VPN-Typ** die Option **Routenbasiert** aus.
8. Wählen Sie unter **SKU** die Option **Basic** aus.
9. Wählen Sie unter **Virtuelles Netzwerk** die Option **VNet-hub** aus.
10. Wählen Sie unter **Öffentliche IP-Adresse** die Option **Neu erstellen** aus, und geben Sie als Name die Zeichenfolge **VNet-hub-GW-pip** ein.
11. Übernehmen Sie für die restlichen Optionen die Standardwerte, und klicken Sie auf **Überprüfen + erstellen**.
12. Überprüfen Sie die Konfiguration, und klicken Sie auf **Erstellen**.

### <a name="create-a-vpn-gateway-for-the-on-premises-virtual-network"></a>Erstellen eines VPN-Gateways für das lokale virtuelle Netzwerk

Erstellen Sie nun das VPN-Gateway für das lokale virtuelle Netzwerk. Netzwerk-zu-Netzwerk-Konfigurationen erfordern einen routenbasierten VPN-Typ. Häufig kann die Erstellung eines VPN-Gateways je nach ausgewählter VPN-Gateway-SKU mindestens 45 Minuten dauern.

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Geben Sie **Gateway für virtuelle Netzwerke** in das Textfeld für die Suche ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie **Gateway für virtuelle Netzwerke** und anschließend **Erstellen** aus.
4. Geben Sie unter **Name** den Namen **GW-Onprem** ein.
5. Wählen Sie unter **Region** die gleiche Region aus wie zuvor.
6. Wählen Sie unter **Gatewaytyp** die Option **VPN** aus.
7. Wählen Sie unter **VPN-Typ** die Option **Routenbasiert** aus.
8. Wählen Sie unter **SKU** die Option **Basic** aus.
9. Wählen Sie unter **Virtuelles Netzwerk** die Option **VNet-Onprem** aus.
10. Wählen Sie unter **Öffentliche IP-Adresse** die Option **Neu erstellen** aus, und geben Sie als Name die Zeichenfolge **VNet-Onprem-GW-pip** ein.
11. Übernehmen Sie für die restlichen Optionen die Standardwerte, und klicken Sie auf **Überprüfen + erstellen**.
12. Überprüfen Sie die Konfiguration, und klicken Sie auf **Erstellen**.

### <a name="create-the-vpn-connections"></a>Erstellen der VPN-Verbindungen

Nun können Sie die VPN-Verbindungen zwischen dem Hub-Gateway und dem lokalen Gateway erstellen.

In diesem Schritt erstellen Sie die Verbindung vom virtuellen Hub-Netzwerk zum lokalen virtuellen Netzwerk. In den Beispielen wird auf einen gemeinsam verwendeten Schlüssel verwiesen. Sie können eigene Werte für den gemeinsam verwendeten Schlüssel verwenden. Wichtig ist dabei, dass der gemeinsam verwendete Schlüssel für beide Verbindungen übereinstimmen muss. Das Erstellen einer Verbindung kann etwas dauern.

1. Öffnen Sie die Ressourcengruppe **FW-Hybrid-Test**, und wählen Sie das Gateway **GW-hub** aus.
2. Wählen Sie in der linken Spalte die Option **Verbindungen** aus.
3. Wählen Sie **Hinzufügen**.
4. Geben Sie als Verbindungsname die Zeichenfolge **Hub-to-Onprem** ein.
5. Wählen Sie unter **Verbindungstyp** die Option **VNet-zu-VNet** aus.
6. Wählen Sie für das **zweite Gateway für virtuelle Netzwerke** die Option **GW-Onprem** aus.
7. Geben Sie unter **Gemeinsam verwendeter Schlüssel (PSK)** die Zeichenfolge **AzureA1b2C3** ein.
8. Klicken Sie auf **OK**.

Erstellen Sie die Verbindung vom lokalen virtuellen Netzwerk zum virtuellen Hub-Netzwerk. Dieser Schritt ähnelt dem vorherigen Schritt, hier erstellen Sie jedoch die Verbindung vom lokalen virtuellen Netzwerk zum virtuellen Hub-Netzwerk. Stellen Sie sicher, dass die freigegebenen Schlüssel übereinstimmen. Nach einigen Minuten wird die Verbindung hergestellt.

1. Öffnen Sie die Ressourcengruppe **FW-Hybrid-Test**, und wählen Sie das Gateway **GW-Onprem** aus.
2. Wählen Sie in der linken Spalte die Option **Verbindungen** aus.
3. Wählen Sie **Hinzufügen**.
4. Geben Sie als Verbindungsname die Zeichenfolge **Onprem-to-Hub** ein.
5. Wählen Sie unter **Verbindungstyp** die Option **VNet-zu-VNet** aus.
6. Wählen Sie für das **zweite Gateway für virtuelle Netzwerke** die Option **GW-hub** aus.
7. Geben Sie unter **Gemeinsam verwendeter Schlüssel (PSK)** die Zeichenfolge **AzureA1b2C3** ein.
8. Klicken Sie auf **OK**.


#### <a name="verify-the-connection"></a>Überprüfen der Verbindung

Nach etwa fünf Minuten sollte der Status beider Verbindungen **Verbunden** lauten.

![Gatewayverbindungen](media/tutorial-hybrid-portal/gateway-connections.png)

## <a name="peer-the-hub-and-spoke-virtual-networks"></a>Durchführen des Peerings für die virtuellen Hub- und Spoke-Netzwerke

Führen Sie nun das Peering für die virtuellen Hub- und Spoke-Netzwerke durch.

1. Öffnen Sie die Ressourcengruppe **FW-Hybrid-Test**, und wählen Sie das virtuelle Netzwerk **VNet-hub** aus.
2. Wählen Sie in der linken Spalte **Peerings** aus.
3. Wählen Sie **Hinzufügen**.
4. Geben Sie unter **Name** den Namen **HubtoSpoke** ein.
5. Wählen Sie unter **Virtuelles Netzwerk** die Option **VNet-spoke** aus.
6. Geben Sie als Name für das Peering von „VNetSpoke“ zu „VNet-hub“ die Zeichenfolge **SpoketoHub** ein.
7. Aktivieren Sie **Gatewaytransit zulassen**.
8. Klicken Sie auf **OK**.

### <a name="configure-additional-settings-for-the-spoketohub-peering"></a>Konfigurieren zusätzlicher Einstellungen für das Peering „SpoketoHub“

Für das Peering „SpoketoHub“ muss die Einstellung **Weitergeleiteten Datenverkehr zulassen** aktiviert werden.

1. Öffnen Sie die Ressourcengruppe **FW-Hybrid-Test**, und wählen Sie das virtuelle Netzwerk **VNet-Spoke** aus.
2. Wählen Sie in der linken Spalte **Peerings** aus.
3. Wählen Sie das Peering **SpoketoHub** aus.
4. Wählen Sie unter **Weitergeleiteten Datenverkehr von "VNet-hub" nach "VNet-Spoke" zulassen** die Option **Aktiviert** aus.
5. Wählen Sie **Speichern** aus.

## <a name="create-the-routes"></a>Erstellen der Routen

Erstellen Sie als Nächstes zwei Routen:

- Eine Route vom Hub-Gatewaysubnetz zum Spoke-Subnetz über die Firewall-IP-Adresse
- Eine Standardroute vom Spoke-Subnetz über die Firewall-IP-Adresse

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Geben Sie **Routingtabelle** in das Textfeld für die Suche ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie **Routingtabelle** aus.
4. Klicken Sie auf **Erstellen**.
5. Geben Sie als Name die Zeichenfolge **UDR-Hub-Spoke** ein.
6. Wählen Sie die Ressourcengruppe **FW-Hybrid-Test** aus.
8. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
9. Klicken Sie auf **Erstellen**.
10. Wählen Sie die erstellte Routingtabelle aus, um die Routingtabellenseite zu öffnen.
11. Wählen Sie in der linken Spalte die Option **Routen** aus.
12. Wählen Sie **Hinzufügen**.
13. Geben Sie als Routenname die Zeichenfolge **ToSpoke** ein.
14. Geben als Adresspräfix die Zeichenfolge **10.6.0.0/16** ein.
15. Wählen Sie als Typ des nächsten Hops die Option **Virtuelles Gerät** aus.
16. Geben Sie als Adresse des nächsten Hops die private IP-Adresse der Firewall ein, die Sie sich zuvor notiert haben.
17. Klicken Sie auf **OK**.

Ordnen Sie nun die Routingtabelle dem Subnetz zu.

1. Wählen Sie auf der Seite **UDR-Hub-Spoke – Routen** die Option **Subnetze** aus.
2. Wählen Sie **Zuordnen** aus.
3. Wählen Sie **Virtuelles Netzwerk auswählen** aus.
4. Wählen Sie **VNet-hub** aus.
5. Wählen Sie **GatewaySubnet** aus.
6. Klicken Sie auf **OK**.

Erstellen Sie nun die Standardroute aus dem Spoke-Subnetz.

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Geben Sie **Routingtabelle** in das Textfeld für die Suche ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie **Routingtabelle** aus.
5. Klicken Sie auf **Erstellen**.
6. Geben Sie als Name die Zeichenfolge **UDR-DG** ein.
7. Wählen Sie die Ressourcengruppe **FW-Hybrid-Test** aus.
8. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
4. Wählen Sie für **Routenverteilung des Gateways für virtuelle Netzwerke** die Option **Deaktiviert** aus.
1. Klicken Sie auf **Erstellen**.
2. Wählen Sie die erstellte Routingtabelle aus, um die Routingtabellenseite zu öffnen.
3. Wählen Sie in der linken Spalte die Option **Routen** aus.
4. Wählen Sie **Hinzufügen**.
5. Geben Sie als Routenname die Zeichenfolge **ToHub** ein.
6. Geben Sie für das Adresspräfix **0.0.0.0/0** ein.
7. Wählen Sie als Typ des nächsten Hops die Option **Virtuelles Gerät** aus.
8. Geben Sie als Adresse des nächsten Hops die private IP-Adresse der Firewall ein, die Sie sich zuvor notiert haben.
9. Klicken Sie auf **OK**.

Ordnen Sie nun die Routingtabelle dem Subnetz zu.

1. Wählen Sie auf der Seite **UDR-DG – Routen** die Option **Subnetze** aus.
2. Wählen Sie **Zuordnen** aus.
3. Wählen Sie **Virtuelles Netzwerk auswählen** aus.
4. Wählen Sie **VNet-spoke** aus.
5. Wählen Sie **SN-Workload** aus.
6. Klicken Sie auf **OK**.

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie nun die VM für die Spoke-Workload und die lokale VM, und platzieren Sie sie in den entsprechenden Subnetzen.

### <a name="create-the-workload-virtual-machine"></a>Erstellen des virtuellen Workloadcomputers

Erstellen Sie im virtuellen Spoke-Netzwerk einen virtuellen Computer ohne öffentliche IP-Adresse, auf dem IIS ausgeführt wird.

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Wählen Sie unter **Beliebt** die Option **Windows Server 2016 Datacenter** aus.
3. Geben Sie die folgenden Werte für den virtuellen Computer ein:
    - **Ressourcengruppe**: Wählen Sie **FW-Hybrid-Test** aus.
    - **Name des virtuellen Computers**: *VM-Spoke-01*
    - **Region**: Die gleiche Region, die Sie auch zuvor verwendet haben.
    - **Benutzername**: *azureuser*
    - **Kennwort**: *Azure123456!*
4. Wählen Sie **Weiter: Datenträger**.
5. Übernehmen Sie die Standardeinstellungen, und wählen Sie **Weiter: Netzwerk** aus.
6. Wählen für das virtuelle Netzwerk die Option **VNet-Spoke** und für das Subnetz die Option **SN-Workload** aus.
7. Wählen Sie unter **Öffentliche IP** die Option **Keine** aus.
8. Wählen Sie unter **Öffentliche Eingangsports** die Option **Ausgewählte Ports zulassen** und anschließend **HTTP (80)** und **RDP (3389)** aus.
9. Wählen Sie **Weiter: Verwaltung** aus.
10. Wählen Sie unter **Startdiagnose** die Option **Aus** aus.
11. Wählen Sie **Überprüfen + erstellen** aus, überprüfen Sie die Einstellungen auf der Zusammenfassungsseite, und wählen Sie anschließend **Erstellen** aus.

### <a name="install-iis"></a>Installieren von IIS

1. Öffnen Sie im Azure-Portal die Cloud Shell, und vergewissern Sie sich, dass **PowerShell** festgelegt ist.
2. Führen Sie den folgenden Befehl aus, um IIS auf dem virtuellen Computer zu installieren und ggf. den Speicherort zu ändern:

   ```azurepowershell-interactive
   Set-AzVMExtension `
           -ResourceGroupName FW-Hybrid-Test `
           -ExtensionName IIS `
           -VMName VM-Spoke-01 `
           -Publisher Microsoft.Compute `
           -ExtensionType CustomScriptExtension `
           -TypeHandlerVersion 1.4 `
           -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell      Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
           -Location EastUS
   ```

### <a name="create-the-on-premises-virtual-machine"></a>Erstellen der lokalen VM

Mit diesem virtuellen Computer wird eine Remotedesktopverbindung mit der öffentlichen IP-Adresse hergestellt. Von dort aus stellen Sie dann über die Firewall eine Verbindung mit dem lokalen Server her.

1. Wählen Sie auf der Startseite des Azure-Portals **Ressource erstellen** aus.
2. Wählen Sie unter **Beliebt** die Option **Windows Server 2016 Datacenter** aus.
3. Geben Sie die folgenden Werte für den virtuellen Computer ein:
    - **Ressourcengruppe**: Wählen Sie „Vorhandene verwenden“ und anschließend **FW-Hybrid-Test** aus.
    - **Name des virtuellen Computers** - *VM-Onprem*.
    - **Region**: Die gleiche Region, die Sie auch zuvor verwendet haben.
    - **Benutzername**: *azureuser*
    - **Kennwort**: *Azure123456!*
4. Wählen Sie **Weiter: Datenträger**.
5. Übernehmen Sie die Standardeinstellungen, und wählen Sie **Weiter: Netzwerk** aus.
6. Wählen Sie als virtuelles Netzwerk die Option **VNet-Onprem** und als Subnetz die Option **SN-Corp** aus.
7. Wählen Sie unter **Öffentliche Eingangsports** die Option **Ausgewählte Ports zulassen** und anschließend **RDP (3389)** aus.
8. Wählen Sie **Weiter: Verwaltung** aus.
9. Wählen Sie unter **Startdiagnose** die Option **Aus** aus.
10. Wählen Sie **Überprüfen + erstellen** aus, überprüfen Sie die Einstellungen auf der Zusammenfassungsseite, und wählen Sie anschließend **Erstellen** aus.

## <a name="test-the-firewall"></a>Testen der Firewall

1. Notieren Sie sich zuerst die private IP-Adresse für den virtuellen Computer **VM-spoke-01**.

2. Stellen Sie über das Azure-Portal eine Verbindung mit dem virtuellen Computer **VM-Onprem** her.
<!---2. Open a Windows PowerShell command prompt on **VM-Onprem**, and ping the private IP for **VM-spoke-01**.

   You should get a reply.--->
3. Öffnen Sie auf **VM-Onprem** einen Webbrowser, und navigieren Sie zu „http://\<VM-spoke-01 private IP\>“.

   Daraufhin sollte die Webseite **VM-spoke-01** angezeigt werden: ![Webseite „VM-spoke-01“](media/tutorial-hybrid-portal/VM-Spoke-01-web.png)

4. Öffnen Sie auf dem virtuellen Computer **VM-Onprem** eine Remotedesktopverbindung mit **VM-spoke-01** an der privaten IP-Adresse.

   Die Verbindungsherstellung sollte erfolgreich sein, und Sie sollten sich anmelden können.

Damit haben Sie sich vergewissert, dass die Firewallregeln funktionieren:

<!---- You can ping the server on the spoke VNet.--->
- Sie können den Webserver im virtuellen Spoke-Netzwerk durchsuchen.
- Sie können per Remotedesktopprotokoll (RDP) eine Verbindung mit dem Server im virtuellen Spoke-Netzwerk herstellen.

Ändern Sie als Nächstes die Aktion zum Sammeln von Firewallnetzwerkregeln in **Verweigern**, um zu überprüfen, ob die Firewallregeln wie erwartet funktionieren.

1. Wählen Sie die Firewall **AzFW01** aus.
2. Wählen Sie **Regeln** aus.
3. Wählen Sie die Registerkarte **Netzwerkregelsammlung** und anschließend die Regelsammlung **RCNet01** aus.
4. Wählen Sie unter **Aktion** die Option **Verweigern** aus.
5. Wählen Sie **Speichern** aus.

Schließen Sie alle vorhandenen Remotedesktops, bevor Sie die geänderten Regeln testen. Führen Sie die Tests nun erneut durch. Dieses Mal sollten alle Tests fehlschlagen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die Firewallressourcen für das nächste Tutorial behalten oder die Ressourcengruppe **FW-Hybrid-Test** löschen, wenn Sie sie nicht mehr benötigen. Hierdurch werden alle firewallbezogenen Ressourcen gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes können Sie die Azure Firewall-Protokolle überwachen.

> [!div class="nextstepaction"]
> [Tutorial: Überwachen von Azure Firewall-Protokollen](./tutorial-diagnostics.md)
