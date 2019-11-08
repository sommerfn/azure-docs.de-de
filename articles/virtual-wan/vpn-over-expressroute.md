---
title: Erstellen einer Site-to-Site-VPN-Verbindung über privates ExpressRoute-Peering in Azure Virtual WAN | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie Azure Virtual WAN verwenden, um eine Site-to-Site-VPN-Verbindung über privates ExpressRoute-Peering zu erstellen.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 10/11/2019
ms.author: cherylmc
Customer intent: I want to connect my on-premises networks to my VNets using S2S VPN connection over my ExpressRoute private peering using Azure Virtual WAN.
ms.openlocfilehash: 6272d6fe6f8c35c06a8121e10be2dd5a2e5512a8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510959"
---
# <a name="create-a-site-to-site-vpn-connection-over-expressroute-private-peering-using-azure-virtual-wan"></a>Erstellen einer Site-to-Site-VPN-Verbindung über privates ExpressRoute-Peering mit Azure Virtual WAN

In diesem Artikel erfahren Sie, wie Sie mit Virtual WAN über das private Peering einer ExpressRoute-Leitung eine IPsec/IKE-VPN-Verbindung von Ihrem lokalen Netzwerk zu Azure herstellen. So kann eine verschlüsselte Übertragung zwischen den lokalen Netzwerken und virtuellen Azure-Netzwerken über ExpressRoute ermöglicht werden, ohne dass die Übertragung über das öffentliche Internet erfolgt oder öffentliche IP-Adressen verwendet werden.

## <a name="topology-and-routing"></a>Topologie und Routing

Das folgende Diagramm zeigt ein Beispiel für ein VPN über eine private Peering-Verbindung mit ExpressRoute:

![VPN über ExpressRoute](./media/vpn-over-expressroute/vwan-vpn-over-er.png)

Das Diagramm zeigt ein Netzwerk innerhalb des lokalen Netzwerks, das über privates ExpressRoute-Peering mit dem VPN-Gateway des Azure-Hubs verbunden ist. Die Einrichtung de Verbindung ist unkompliziert:

1. Einrichten einer ExpressRoute-Verbindung mit einer ExpressRoute-Leitung und privatem Peering
2. Einrichten der VPN-Verbindung, wie in diesem Dokument beschrieben

Ein wichtiger Aspekt dieser Konfiguration ist das Routing zwischen den lokalen Netzwerken und Azure über die ExpressRoute- und VPN-Pfade.

### <a name="traffic-from-on-premises-networks-to-azure"></a>Datenverkehr von lokalen Netzwerken zu Azure

Für den Datenverkehr von lokalen Netzwerken zu Azure werden die Azure-Präfixe (einschließlich des virtuellen Hubs und aller mit dem Hub verbundenen virtuellen Netzwerke) über das BGP für das private ExpressRoute-Peering und über das VPN-BGP angekündigt. Dies führt zu zwei Netzwerkrouten (Pfaden) zu Azure ausgehend von den lokalen Netzwerken: eine über den geschützten IPsec-Pfad und eine direkt über den ExpressRoute-Pfad **ohne** IPsec-Schutz. Damit die Kommunikation verschlüsselt erfolgt, muss für das VPN-verbundene Netzwerk im Diagramm sichergestellt werden, dass die Azure-Routen über das lokale VPN-Gateway dem direkten ExpressRoute-Pfad gegenüber bevorzugt werden.

### <a name="traffic-from-azure-to-on-premises-networks"></a>Datenverkehr von Azure zu lokalen Netzwerken

Die gleiche Anforderung gilt für den Datenverkehr von Azure zu lokalen Netzwerken. Damit der IPsec-Pfad dem direkten ExpressRoute-Pfad (ohne IPsec) gegenüber bevorzugt wird, haben Sie zwei Möglichkeiten:

- Kündigen Sie in der VPN-BGP-Sitzung für das VPN-verbundene Netzwerk spezifischere Präfixe an. Sie können einen größeren Bereich, der das „VPN-verbundene Netzwerk“ über das private ExpressRoute-Peering umfasst, und dann spezifischere Bereiche in der VPN-BGP-Sitzung ankündigen. Kündigen Sie z. B. 10.0.0.0/16 über ExpressRoute und 10.0.1.0/24 über das VPN an.

- Kündigen Sie getrennte Präfixe für VPN und ExpressRoute an. Wenn die Bereiche für das VPN-verbundene Netzwerk von anderen ExpressRoute-verbundenen Netzwerken getrennt sind, können Sie die Präfixe in den VPN- bzw. ExpressRoute-BGP-Sitzungen ankündigen. Kündigen Sie z. B. 10.0.0.0/24 über ExpressRoute und 10.0.1.0/24 über das VPN an.

In diesen beiden Beispielen sendet Azure den Datenverkehr über die VPN-Verbindung und nicht ohne VPN-Schutz direkt über ExpressRoute an 10.0.1.0/24.

> [!WARNING]
> Wenn Sie die **gleichen** Präfixe über ExpressRoute- und VPN-Verbindungen ankündigen, **verwendet Azure den ExpressRoute-Pfad direkt ohne VPN-Schutz**.
>

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="openvwan"></a>1. Erstellen eines virtuellen WAN und Hubs mit Gateways

Damit Sie fortfahren können, müssen die folgenden Azure-Ressourcen und die entsprechenden lokalen Konfigurationen vorhanden sein:

1. Ein Azure Virtual WAN
2. Ein Virtual WAN-Hub mit einem [ExpressRoute-Gateway](virtual-wan-expressroute-portal.md) und einem [VPN-Gateway](virtual-wan-site-to-site-portal.md)

Weitere Informationen zu den Schritten zum Erstellen eines Azure Virtual WAN und Hubs mit einer ExpressRoute-Zuordnung finden Sie unter [Erstellen einer ExpressRoute-Zuordnung per Azure Virtual WAN](virtual-wan-expressroute-portal.md). Weitere Informationen zu den Schritten zum Erstellen eines VPN-Gateways im virtuellen WAN finden Sie unter [Erstellen einer Site-to-Site-Verbindung per Azure Virtual WAN](virtual-wan-site-to-site-portal.md).

## <a name="site"></a>2. Erstellen eines Standorts für das lokale Netzwerk

Die Standortressource stimmt mit den Nicht-ExpressRoute-VPN-Standorten für das virtuelle WAN überein. Hier muss unbedingt beachtet werden, dass die IP-Adresse des lokalen VPN-Geräts nun entweder eine private IP-Adresse oder eine öffentliche IP-Adresse im lokalen Netzwerk sein kann, das über das private ExpressRoute-Peering erreichbar ist, das in Schritt 1 erstellt wurde.

> [!NOTE]
> Die IP-Adresse des lokalen VPN-Geräts MUSS Teil der Adresspräfixe sein, die dem Virtual WAN-Hub über das private Azure ExpressRoute-Peering angekündigt wurden.
>

1. Navigieren Sie in Ihrem Browser zum Azure-Portal. Klicken Sie auf das von Ihnen erstellte WAN. Klicken Sie auf der WAN-Seite unter **Konnektivität** auf **VPN-Standorte**, um die entsprechende Seite zu öffnen.

2. Klicken Sie auf der Seite **VPN sites** (VPN-Standorte) auf **+Create site** (+ Standort erstellen).

3. Füllen Sie auf der Seite **Create site** (Standort erstellen) die folgenden Felder aus:

   * **Abonnement**: Überprüfen Sie das Abonnement.
   * **Ressourcengruppe:** Wählen Sie die gewünschte Ressourcengruppe aus oder erstellen Sie sie.
   * **Region**: Die Azure-Region für die VPN-Standortressource.
   * **Name**: Der Name, den Sie Ihrer lokalen Site geben möchten.
   * **Gerätehersteller**: Der Hersteller des lokalen VPN-Geräts.
   * **Border Gateway Protocol**: Wählen Sie „Aktivieren“ aus, wenn das lokale Netzwerk BGP verwendet.
   * **Privater Adressraum**: Dies ist der IP-Adressraum Ihrer lokalen Site. Der für diesen Adressraum bestimmte Datenverkehr wird über das VPN-Gateway an das lokale Netzwerk weitergeleitet.
   * **Hubs**: Wählen Sie mindestens einen Hubs für die Verbindung zu diesen VPN-Standort aus. Für die ausgewählten Hubs müssen bereits VPN-Gateways erstellt worden sein.

4. Klicken Sie auf **Weiter: Links >** , um die VPN-Verbindungseinstellungen anzuzeigen:

   * **Name**: Der Name, den Sie für diese Verbindung verwenden möchten.
   * **Anbietername**: Der Name des Internetdienstanbieters für diesen Standort. Bei einem lokalen ExpressRoute-Netzwerk der Name des ExpressRoute-Dienstanbieters.
   * **Geschwindigkeit**: Die Geschwindigkeit der Internetdienstverbindung oder der ExpressRoute-Leitung.
   * **IP-Adresse**: Die öffentliche IP-Adresse des VPN-Geräts, das sich am lokalen Standort befindet. Oder bei einer lokalen ExpressRoute die private IP-Adresse des VPN-Geräts über ExpressRoute.

   Wenn BGP aktiviert ist, wird BGP für alle Verbindungen verwendet, die für diesen Standort in Azure erstellt werden. Die Vorgehensweise zum Konfigurieren von BGP für eine Virtual WAN-Instanz ist dieselbe wie beim Konfigurieren von BGP für ein Azure-VPN-Gateway. Die lokale BGP-Peeradresse darf *NICHT* mit der IP-Adresse Ihres VPN-Geräts oder dem VNet-Adressraum des VPN-Standorts übereinstimmen. Verwenden Sie als BGP-Peer-IP-Adresse eine andere IP-Adresse für das VPN-Gerät. Dabei kann es sich um eine Adresse handeln, die der Loopback-Schnittstelle des Geräts zugewiesen ist. Es darf jedoch *keine* APIPA-Adresse (169.254.*x*.*x*) sein. Geben Sie diese Adresse im entsprechenden lokalen Netzwerkgateway an, das den Standort darstellt. Informationen zu den Voraussetzungen für BGP finden Sie unter [Übersicht über BGP mit Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md).

5. Klicken Sie auf **Weiter: Überprüfen + Erstellen >** , um die festgelegten Werte zu überprüfen und die VPN-Site zu erstellen. Wenn Sie **Hubs** für die Verbindung ausgewählt haben, wird die Verbindung zwischen dem lokalen Netzwerk und dem VPN-Gateway des Hubs hergestellt.

## <a name="hub"></a>3. Aktualisieren der VPN-Verbindungseinstellung für die Verwendung von ExpressRoute

Nachdem Sie den VPN-Standort erstellt und die Verbindung zum Hub hergestellt haben, führen Sie die folgenden Schritte aus, um die Verbindung für das private ExpressRoute-Peering zu konfigurieren:

1. Kehren Sie zur Virtual WAN-Ressourcenseite zurück und klicken Sie auf die Hubressource. Oder navigieren Sie vom VPN-Standort zum verbundenen Hub.

2. Klicken Sie unter **Konnektivität** auf **VPN (Standort-zu-Standort)** .

3. Klicken Sie neben dem VPN-Standort über ExpressRoute auf „...“ und wählen Sie **VPN-Verbindung mit diesem Hub bearbeiten** aus.

4. Wählen Sie für **Private Azure-IP-Adresse verwenden** „Ja“ aus. Mit dieser Einstellung wird das Hub-VPN-Gateway so konfiguriert, dass für diese Verbindung anstelle der öffentlichen IP-Adressen private IP-Adressen innerhalb des Hubadressbereichs auf dem Gateway verwendet werden. Dadurch wird sichergestellt, dass der Datenverkehr aus dem lokalen Netzwerk die privaten ExpressRoute-Peeringpfade durchläuft und nicht das öffentliche Internet für diese VPN-Verbindung verwendet wird. Der folgende Screenshot zeigt das Einstellungsfenster.

   ![VPN-Verbindungseinstellung](./media/vpn-over-expressroute/vpn-link-configuration.png)
   
5. Klicken Sie auf **Speichern**.

Nachdem die Einstellungen gespeichert wurden, verwendet das Hub-VPN-Gateway die privaten IP-Adressen des VPN-Gateways, um die IPsec/IKE-Verbindungen zum lokalen VPN-Gerät über ExpressRoute herzustellen.

## <a name="associate"></a>4. Ermitteln der privaten IP-Adressen des Hub-VPN-Gateways

Laden Sie die VPN-Gerätekonfiguration herunter, um die privaten IP-Adressen des Hub-VPN-Gateways zu ermitteln. Diese sind erforderlich, um das lokale VPN-Gerät zu konfigurieren.

1. Klicken Sie auf der Seite für den Hub unter **Konnektivität** auf **VPN (Standort-zu-Standort)** .

2. Klicken Sie oben auf der Seite „Übersicht“ auf **VPN-Konfiguration herunterladen**. Azure erstellt ein Speicherkonto in der Ressourcengruppe „microsoft-network-[location]“, wobei „location“ für den WAN-Standort steht. Nachdem Sie die Konfiguration auf Ihre VPN-Geräte angewendet haben, können Sie dieses Speicherkonto löschen.

3. Nachdem die Erstellung der Datei abgeschlossen wurde, können Sie auf den Link klicken, um sie herunterzuladen.

4. Wenden Sie die Konfiguration auf Ihr VPN-Gerät an.

### <a name="understanding-the-vpn-device-configuration-file"></a>Grundlegendes zur Konfigurationsdatei für VPN-Geräte

Die Gerätekonfigurationsdatei enthält die Einstellungen, die beim Konfigurieren Ihrer lokalen VPN-Geräte verwendet werden. Beachten Sie beim Anzeigen dieser Datei die folgenden Informationen:

* **vpnSiteConfiguration**: In diesem Abschnitt sind die Gerätedetails für die Einrichtung einer Site angegeben, für die eine Verbindung mit dem virtuellen WAN hergestellt wird. Sie enthält den Namen und die öffentliche IP-Adresse des Zweigstellengeräts.
* **vpnSiteConnections**: Dieser Abschnitt enthält die folgenden Einstellungen:

    * **Adressraum** des virtuellen Hub-VNET<br/>Beispiel:
           ```
           "AddressSpace":"10.51.230.0/24"
           ```
    * **Adressraum** der VNETs, die mit dem Hub verbunden sind<br>Beispiel:
           ```
           "ConnectedSubnets":["10.51.231.0/24"]
            ```
    * **IP-Adressen** des vpngateway für den virtuellen Hub. Da jede Verbindung des vpngateway aus zwei Tunneln mit Aktiv-Aktiv-Konfiguration besteht, werden in dieser Datei beide IP-Adressen aufgelistet. In diesem Beispiel werden „Instance0“ und „Instance1“ für jeden Standort angezeigt. Dabei handelt sich um private IP-Adressen und nicht um öffentliche IP-Adressen.<br>Beispiel:
           ``` 
           "Instance0":"10.51.230.4"
           "Instance1":"10.51.230.5"
           ```
    * **Details zur Konfiguration der vpngateway-Verbindung**, z.B. BGP, vorinstallierter Schlüssel usw. Der vorinstallierte Schlüssel (Pre-Shared Key, PSK) wird automatisch für Sie erstellt. Sie können die Verbindung für einen benutzerdefinierten PSK auf der Seite „Übersicht“ jederzeit bearbeiten.
  
### <a name="example-device-configuration-file"></a>Beispiel für Gerätekonfigurationsdatei

```
[{
      "configurationVersion":{
        "LastUpdatedTime":"2019-10-11T05:57:35.1803187Z",
        "Version":"5b096293-edc3-42f1-8f73-68c14a7c4db3"
      },
      "vpnSiteConfiguration":{
        "Name":"VPN-over-ER-site",
        "IPAddress":"172.24.127.211",
        "LinkName":"VPN-over-ER"
      },
      "vpnSiteConnections":[{
        "hubConfiguration":{
          "AddressSpace":"10.51.230.0/24",
          "Region":"West US 2",
          "ConnectedSubnets":["10.51.231.0/24"]
        },
        "gatewayConfiguration":{
          "IpAddresses":{
            "Instance0":"10.51.230.4",
            "Instance1":"10.51.230.5"
          }
        },
        "connectionConfiguration":{
          "IsBgpEnabled":false,
          "PSK":"abc123",
          "IPsecParameters":{"SADataSizeInKilobytes":102400000,"SALifeTimeInSeconds":3600}
        }
      }]
    },
    {
      "configurationVersion":{
        "LastUpdatedTime":"2019-10-11T05:57:35.1803187Z",
        "Version":"fbdb34ea-45f8-425b-9bc2-4751c2c4fee0"
      },
      "vpnSiteConfiguration":{
        "Name":"VPN-over-INet-site",
        "IPAddress":"13.75.195.234",
        "LinkName":"VPN-over-INet"
      },
      "vpnSiteConnections":[{
        "hubConfiguration":{
          "AddressSpace":"10.51.230.0/24",
          "Region":"West US 2",
          "ConnectedSubnets":["10.51.231.0/24"]
        },
        "gatewayConfiguration":{
          "IpAddresses":{
            "Instance0":"51.143.63.104",
            "Instance1":"52.137.90.89"
          }
        },
        "connectionConfiguration":{
          "IsBgpEnabled":false,
          "PSK":"abc123",
          "IPsecParameters":{"SADataSizeInKilobytes":102400000,"SALifeTimeInSeconds":3600}
        }
      }]
}]
```

### <a name="configuring-your-vpn-device"></a>Konfigurieren Ihres VPN-Geräts

Falls Sie eine Anleitung für die Konfiguration Ihres Geräts benötigen, können Sie die Anleitung auf der [Seite mit den Schritten für die VPN-Gerätekonfiguration](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) verwenden. Beachten Sie hierbei aber die folgenden Einschränkungen:

* Die Anleitung auf der Seite für die VPN-Geräte wurde nicht für Virtual WAN geschrieben, aber Sie können die Virtual WAN-Werte aus der Konfigurationsdatei verwenden, um Ihr VPN-Gerät manuell zu konfigurieren. 
* Die herunterladbaren Skripts für die Gerätekonfiguration, die für VPN Gateway bestimmt sind, funktionieren nicht für Virtual WAN, da sich die Konfiguration unterscheidet.
* Für eine neue Virtual WAN-Instanz können IKEv1 und IKEv2 unterstützt werden.
* Für Virtual WAN können nur routenbasierte VPN-Geräte und die entsprechenden Geräteanweisungen verwendet werden.

## <a name="viewwan"></a>5. Anzeigen Ihrer Virtual WAN-Instanz

1. Navigieren Sie zum virtuellen WAN.

2. Auf der Seite „Übersicht“ steht jeder Punkt auf der Karte für einen Hub. Bewegen Sie den Mauszeiger jeweils auf einen dieser Punkte, um die Zusammenfassung zur Integrität des Hubs anzuzeigen.

3. Im Abschnitt mit den Hubs und Verbindungen können Sie den Hubstatus, die Site, die Region, den VPN-Verbindungsstatus und die Anzahl von ein- und ausgehenden Byte anzeigen.

## <a name="viewhealth"></a>6. Anzeigen der Ressourcenintegrität

1. Navigieren Sie zu Ihrem WAN.

2. Klicken Sie auf der Seite für Ihr WAN im Abschnitt **Support + Problembehandlung** auf **Integrität**, und zeigen Sie Ihre Ressource an.

## <a name="connectmon"></a>7. Überwachen einer Verbindung

Erstellen Sie eine Verbindung, um die Kommunikation zwischen einer Azure-VM und einem Remotestandort zu überwachen. Weitere Informationen zum Einrichten einer Verbindungsüberwachung finden Sie unter [Überwachen der Netzwerkkommunikation](~/articles/network-watcher/connection-monitor.md). Das Quellfeld ist die VM-IP in Azure, und die Ziel-IP ist die Site-IP.

## <a name="cleanup"></a>8. Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht mehr benötigen, können Sie den Befehl [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) verwenden, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Ersetzen Sie „myResourceGroup“ durch den Namen Ihrer Ressourcengruppe, und führen Sie den folgenden PowerShell-Befehl aus:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel unterstützt Sie beim Erstellen einer VPN-Verbindung über privates ExpressRoute-Peering mit Azure Virtual WAN. Weitere Informationen zu Virtual WAN und anderen zugehörigen Funktionen finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).
