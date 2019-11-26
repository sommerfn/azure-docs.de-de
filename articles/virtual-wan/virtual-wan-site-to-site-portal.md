---
title: 'Azure Virtual WAN: Erstellen von Site-to-Site-Verbindungen'
description: In diesem Tutorial wird beschrieben, wie Sie Azure Virtual WAN verwenden, um eine Site-to-Site-VPN-Verbindung mit Azure zu erstellen.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: 82f662dd00805cf53c0581fb0a2b3322a0207a11
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74005707"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan"></a>Tutorial: Erstellen einer Site-to-Site-Verbindung per Azure Virtual WAN

In diesem Tutorial wird beschrieben, wie Sie Virtual WAN zum Verbinden Ihrer Ressourcen in Azure über eine IPsec/IKE-VPN-Verbindung (IKEv1 und IKEv2) nutzen. Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine extern zugängliche, öffentliche IP-Adresse zugewiesen ist. Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen WAN
> * Erstellen eines Hubs
> * Erstellen einer Site
> * Herstellen einer Verbindung zwischen einer Site und einem Hub
> * Herstellen einer VPN-Verbindung zwischen einer Site und einem Hub
> * Verbinden eines VNET mit einem Hub
> * Herunterladen einer Konfigurationsdatei
> * Anzeigen Ihrer Virtual WAN-Instanz

> [!NOTE]
> Falls Sie über viele Sites verfügen, verwenden Sie normalerweise einen [Virtual WAN-Partner](https://aka.ms/virtualwan), um diese Konfiguration zu erstellen. Sie können diese Konfiguration aber auch selbst erstellen, wenn Sie mit Netzwerken vertraut sind und sich mit der Konfiguration Ihres eigenen VPN-Geräts auskennen.
>

![Virtual WAN-Diagramm](./media/virtual-wan-about/virtualwan.png)

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

* Sie verfügen über ein virtuelles Netzwerk, mit dem Sie eine Verbindung herstellen möchten. Stellen Sie sicher, dass sich kein Subnetz Ihres lokalen Netzwerks mit den virtuellen Netzwerken überlappt, mit denen Sie eine Verbindung herstellen möchten. Informationen zum Erstellen eines virtuellen Netzwerks im Azure-Portal finden Sie im [Schnellstart](../virtual-network/quick-create-portal.md).

* Ihr virtuelles Netzwerk verfügt nicht über Gateways für virtuelle Netzwerke. Falls Ihr virtuelles Netzwerk über ein Gateway verfügt (entweder VPN oder ExpressRoute), müssen Sie alle Gateways entfernen. Für diese Konfiguration ist es erforderlich, dass virtuelle Netzwerke stattdessen mit dem Gateway des Virtual WAN-Hubs verbunden werden.

* Beschaffen Sie sich einen IP-Adressbereich für Ihre Hubregion. Der Hub ist ein virtuelles Netzwerk, das von Virtual WAN erstellt und verwendet wird. Der von Ihnen für den Hub angegebene Adressbereich darf sich nicht mit einem Ihrer vorhandenen virtuellen Netzwerke überlappen, mit denen Sie eine Verbindung herstellen. Außerdem ist keine Überlappung mit Ihren Adressbereichen möglich, mit denen Sie lokal eine Verbindung herstellen. Falls Sie nicht mit den IP-Adressbereichen in Ihrer lokalen Netzwerkkonfiguration vertraut sind, sollten Sie sich an eine Person wenden, die Ihnen diese Informationen zur Verfügung stellen kann.

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

## <a name="openvwan"></a>Erstellen eines Virtual WAN

Navigieren Sie in einem Browser zum Azure-Portal, und melden Sie sich mit Ihrem Azure-Konto an.

1. Navigieren Sie zur Seite „Virtual WAN“. Klicken Sie im Portal auf **Ressource erstellen**. Geben Sie **Virtual WAN** in das Suchfeld ein, und drücken Sie die EINGABETASTE.
2. Wählen Sie in den Ergebnissen **Virtual WAN** aus. Klicken Sie auf der Seite „Virtual WAN“ auf **Erstellen**, um die Seite „WAN erstellen“ zu öffnen.
3. Füllen Sie auf der Seite **WAN erstellen** auf der Registerkarte **Grundlagen** die folgenden Felder aus:

   ![Virtuelles WAN](./media/virtual-wan-site-to-site-portal/vwan.png)

   * **Abonnement**: Wählen Sie das Abonnement aus, das Sie verwenden möchten.
   * **Ressourcengruppe**: Erstellen Sie eine neue Ressourcengruppe, oder verwenden Sie eine vorhandene.
   * **Ressourcengruppenstandort**: Wählen Sie in der Dropdownliste einen Ressourcengruppenstandort aus. Ein WAN ist eine globale Ressource, die nicht in einer bestimmten Region angeordnet ist. Sie müssen aber eine Region auswählen, damit Sie die von Ihnen erstellte WAN-Ressource leichter verwalten und finden können.
   * **Name**: Geben Sie den Namen ein, den Sie Ihrem WAN geben möchten.
   * **Typ:** „Basic“ oder „Standard“. Bei der Erstellung eines WAN vom Typ „Basic“ können Sie auch nur einen Hub vom Typ „Basic“ erstellen. Für „Basic“-Hubs sind nur Site-to-Site-VPN-Verbindungen möglich.
4. Wählen Sie nach dem Ausfüllen der Felder die Option **Bewerten + erstellen** aus.
5. Wählen Sie nach der bestandenen Überprüfung die Option **Erstellen** aus, um das Virtual WAN zu erstellen.

## <a name="hub"></a>Erstellen eines Hubs

Ein Hub ist ein virtuelles Netzwerk, das Gateways für Verbindungen vom Typ „Site-to-Site“, „ExpressRoute“ oder „Point-to-Site“ enthalten kann. Nachdem der Hub erstellt wurde, werden Ihnen für den Hub auch dann Kosten berechnet, wenn Sie keine Websites zuordnen. Es dauert 30 Minuten, um das Site-to-Site-VPN-Gateway im virtuellen Hub zu erstellen.

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-s2s-hub-include.md)]

## <a name="site"></a>Erstellen einer Site

Nun ist alles bereit, um die Sites gemäß Ihren physischen Standorten zu erstellen. Erstellen Sie beliebig viele Sites, die Ihren physischen Standorten entsprechen. Erstellen Sie beispielsweise drei separate Sites, wenn Sie jeweils über eine Filiale in New York, London und Los Angeles verfügen. Diese Sites enthalten Ihre lokalen VPN-Geräteendpunkte. Sie können in einem Virtual WAN bis zu 1.000 Sites pro virtuellem Hub erstellen. Bei mehreren Hubs ist die Erstellung von 1.000 Sites pro Hub möglich. Falls Sie über ein CPE-Gerät eines Virtual WAN-Partners verfügen (Linkeinfügung), können Sie sich beim Partner über die Automatisierungsmöglichkeiten in Azure informieren. Normalerweise umfasst die Automatisierung eine einfache Vorgehensweise per Klick, um umfassende Branchinformationen nach Azure zu exportieren und die Konnektivität vom CPE zum Azure Virtual WAN-VPN-Gateway einzurichten (Link zu einem Leitfaden zur Automatisierung von Azure zu CPE-Partnern).

[!INCLUDE [Create a site](../../includes/virtual-wan-tutorial-s2s-site-include.md)]

## <a name="connectsites"></a>Herstellen einer Verbindung von der VPN-Site mit dem Hub

In diesem Schritt stellen Sie für Ihre VPN-Site eine Verbindung mit dem Hub her.

[!INCLUDE [Connect VPN sites](../../includes/virtual-wan-tutorial-s2s-connect-vpn-site-include.md)]

## <a name="vnet"></a>Herstellen einer Verbindung zwischen VNET und Hub

In diesem Schritt erstellen Sie die Verbindung zwischen Ihrem Hub und einem VNET. Wiederholen Sie diese Schritte für jedes VNET, mit dem Sie eine Verbindung herstellen möchten.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Virtuelle Netzwerkverbindungen**.
2. Klicken Sie auf der Seite für die VNET-Verbindung auf **+Add connection** (+Verbindung hinzufügen).
3. Füllen Sie auf der Seite **Add connection** (Verbindung hinzufügen) die folgenden Felder aus:

    * **Verbindungsname**: Dies ist der Name Ihrer Verbindung.
    * **Hubs**: Wählen Sie den Hub aus, den Sie dieser Verbindung zuordnen möchten.
    * **Abonnement**: Überprüfen Sie das Abonnement.
    * **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk aus, das Sie mit diesem Hub verbinden möchten. Für das virtuelle Netzwerk kann nicht bereits ein Gateway für virtuelle Netzwerke vorhanden sein.
4. Klicken Sie auf **OK**, um die Verbindung für das virtuelle Netzwerk zu erstellen.

## <a name="device"></a>Herunterladen der VPN-Konfiguration

Verwenden Sie die VPN-Gerätekonfiguration, um Ihr lokales VPN-Gerät zu konfigurieren.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Übersicht**.
2. Klicken Sie oben auf der Seite **Hub > VPNSite** auf **VPN-Konfiguration herunterladen**. Azure erstellt ein Speicherkonto in der Ressourcengruppe „microsoft-network-[location]“, wobei „location“ für den WAN-Standort steht. Nachdem Sie die Konfiguration auf Ihre VPN-Geräte angewendet haben, können Sie dieses Speicherkonto löschen.
3. Nachdem die Erstellung der Datei abgeschlossen wurde, können Sie auf den Link klicken, um sie herunterzuladen.
4. Wenden Sie die Konfiguration auf Ihr lokales VPN-Gerät an.

### <a name="understanding-the-vpn-device-configuration-file"></a>Grundlegendes zur Konfigurationsdatei für VPN-Geräte

Die Gerätekonfigurationsdatei enthält die Einstellungen, die beim Konfigurieren Ihrer lokalen VPN-Geräte verwendet werden. Beachten Sie beim Anzeigen dieser Datei die folgenden Informationen:

* **vpnSiteConfiguration**: In diesem Abschnitt sind die Gerätedetails für die Einrichtung einer Site angegeben, für die eine Verbindung mit dem virtuellen WAN hergestellt wird. Sie enthält den Namen und die öffentliche IP-Adresse des Zweigstellengeräts.
* **vpnSiteConnections**: Dieser Abschnitt enthält die folgenden Einstellungen:

    * **Adressraum** des virtuellen Hub-VNET<br>Beispiel:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Adressraum** der VNETs, die mit dem Hub verbunden sind<br>Beispiel:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * **IP-Adressen** des vpngateway für den virtuellen Hub. Da jede Verbindung des vpngateway aus zwei Tunneln mit Aktiv-Aktiv-Konfiguration besteht, werden in dieser Datei beide IP-Adressen aufgelistet. In diesem Beispiel werden für jede Site „Instance0“ und „Instance1“ angezeigt.<br>Beispiel:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Details zur Konfiguration der vpngateway-Verbindung**, z.B. BGP, vorinstallierter Schlüssel usw. Der vorinstallierte Schlüssel (Pre-Shared Key, PSK) wird automatisch für Sie erstellt. Sie können die Verbindung für einen benutzerdefinierten PSK auf der Seite „Übersicht“ jederzeit bearbeiten.
  
### <a name="example-device-configuration-file"></a>Beispiel für Gerätekonfigurationsdatei

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.30.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

### <a name="configuring-your-vpn-device"></a>Konfigurieren Ihres VPN-Geräts

>[!NOTE]
> Wenn Sie mit einer Virtual WAN-Partnerlösung arbeiten, wird die VPN-Gerätekonfiguration automatisch durchgeführt. Der Gerätecontroller ruft die Konfigurationsdatei aus Azure ab und wendet sie auf das Gerät an, um die Verbindung mit Azure einzurichten. Dies bedeutet, dass Sie nicht wissen müssen, wie Sie Ihr VPN-Gerät manuell konfigurieren.
>

Falls Sie eine Anleitung für die Konfiguration Ihres Geräts benötigen, können Sie die Anleitung auf der [Seite mit den Schritten für die VPN-Gerätekonfiguration](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) verwenden. Beachten Sie hierbei aber die folgenden Einschränkungen:

* Die Anleitung auf der Seite für die VPN-Geräte wurde nicht für Virtual WAN geschrieben, aber Sie können die Virtual WAN-Werte aus der Konfigurationsdatei verwenden, um Ihr VPN-Gerät manuell zu konfigurieren. 
* Die herunterladbaren Skripts für die Gerätekonfiguration, die für VPN Gateway bestimmt sind, funktionieren nicht für Virtual WAN, da sich die Konfiguration unterscheidet.
* Für eine neue Virtual WAN-Instanz können IKEv1 und IKEv2 unterstützt werden.
* Für Virtual WAN können nur routenbasierte VPN-Geräte und die entsprechenden Geräteanweisungen verwendet werden.

## <a name="viewwan"></a>Anzeigen Ihrer Virtual WAN-Instanz

1. Navigieren Sie zum virtuellen WAN.
2. Auf der Seite **Übersicht** steht jeder Punkt auf der Karte für einen Hub. Bewegen Sie den Mauszeiger auf einen beliebigen Punkt, um die Zusammenfassung zur Integrität des Hubs, den Verbindungsstatus und die ein- und ausgehenden Bytes anzuzeigen.
3. Im Abschnitt „Hubs und Verbindungen“ können Sie den Hubstatus, die VPN-Sites und weitere Daten anzeigen. Sie können auf einen bestimmten Hubnamen klicken und zur VPN-Site navigieren, um weitere Informationen zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).