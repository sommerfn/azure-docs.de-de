---
title: VMware-Lösung von CloudSimple – Azure-Netzwerkverbindungen
description: Erfahren Sie mehr über das Verbinden Ihres virtuellen Azure-Netzwerks mit Ihrem CloudSimple-Regionsnetzwerk.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 90e3121c3f036d1abc8ca372ee349aef3485d07b
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69625027"
---
# <a name="azure-network-connections-overview"></a>Übersicht über Azure-Netzwerkverbindungen

Wenn Sie einen CloudSimple-Dienst in einer Region und Knoten erstellen, können Sie die folgenden Aktionen ausführen:

* Anfordern einer Azure ExpressRoute-Leitung und Anfügen der Leitung and das CloudSimple-Netzwerk in dieser Region.
* Verbinden Ihres CloudSimple-Regionsnetzwerks mit Ihrem virtuellen Azure-Netzwerk oder Ihrem lokalen Netzwerk mithilfe von Azure ExpressRoute.
* Bereitstellen des Zugriffs aus der privaten Cloudumgebung auf Dienste, die in Ihrem Azure-Abonnement oder lokalen Netzwerk ausgeführt werden.

Die ExpressRoute-Verbindung weist eine hohe Bandbreite mit geringer Latenz auf.

## <a name="benefits"></a>Vorteile

Eine Azure-Netzwerkverbindung ermöglicht Ihnen Folgendes:

* Verwenden von Azure als Sicherungsziel für virtuelle Computer in Ihrer privaten Cloud
* Bereitstellen von KMS-Servern in Ihrem Azure-Abonnement, um den vSAN-Datenspeicher Ihrer privaten Cloud zu verschlüsseln
* Verwenden von Hybridanwendungen, bei denen die Webebene der jeweiligen Anwendung in der öffentlichen Cloud ausgeführt wird, während die Anwendungs- und die Datenbankebene in Ihrer privaten Cloud ausgeführt werden

## <a name="azure-virtual-network-connection"></a>Verbindung mit virtuellem Azure-Netzwerk

Private Clouds können mit Ihren Azure-Ressourcen mithilfe von ExpressRoute verbunden werden.  Die ExpressRoute-Verbindung ermöglicht Ihnen den Zugriff auf verschiedene Ressourcen, die in Ihrem Azure-Abonnement über Ihre private Cloud ausgeführt werden.  Diese Verbindung ermöglicht es Ihnen, Ihr privates Cloudnetzwerk auf Ihr virtuelles Azure-Netzwerk zu erweitern.  Routen aus dem CloudSimple-Netzwerk werden über BGP mit Ihrem virtuellen Azure-Netzwerk ausgetauscht.  Wenn Sie Peering virtueller Netzwerke konfiguriert haben, ist der Zugriff auf alle virtuellen Netzwerke mittels Peering über Ihr CloudSimple-Netzwerk möglich.

![Azure ExpressRoute-Verbindung mit virtuellem Netzwerk](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>ExpressRoute-Verbindung mit lokalem Netzwerk

Sie können Ihre vorhandene Azure ExpressRoute-Verbindung (Netzwerk) mit Ihrer CloudSimple-Region verbinden. Über den ExpressRoute-Dienst Global Reach werden die beiden Netzwerke miteinander verbunden.  Es wird eine Verbindung zwischen dem lokalen und dem CloudSimple-ExpressRoute-Netzwerk hergestellt.  Diese Verbindung ermöglicht es Ihnen, Ihre lokalen Netzwerke auf das private Cloudnetzwerk zu erweitern. Routen aus dem CloudSimple-Netzwerk werden über BGP mit Ihrem lokalen Netzwerk ausgetauscht.

![Lokale ExpressRoute-Verbindung – Global Reach](media/cloudsimple-global-reach-connection.png)


## <a name="connection-to-on-premises-network-and-azure-virtual-network"></a>Verbindung mit dem lokalem Netzwerk und dem virtuellen Azure-Netzwerk

Verbindungen mit dem lokalen Netzwerk und dem virtuellen Azure-Netzwerk können aus Ihrem CloudSimple-Netzwerk koexistieren.  Die Verbindung verwendet BGP zum Austauschen von Routen zwischen dem lokalen Netzwerk, dem virtuellen Azure-Netzwerk und dem CloudSimple-Netzwerk.  Wenn eine Global Reach-Verbindung vorhanden ist und Sie Ihr CloudSimple-Netzwerk mit Ihrem virtuellen Azure-Netzwerk verbinden, werden Routen des virtuellen Azure-Netzwerks in Ihrem lokalen Netzwerk angezeigt.  Der Routenaustausch erfolgt in Azure zwischen den Edgeroutern.

![Lokale ExpressRoute-Verbindung mit der Verbindung des virtuellen Azure-Netzwerks](media/cloudsimple-global-reach-and-vnet-connection.png)

### <a name="important-considerations"></a>Wichtige Hinweise

Das Herstellen einer Verbindung mit dem CloudSimple-Netzwerk aus dem lokalen Netzwerk und aus dem virtuellen Azure-Netzwerk ermöglicht den Routenaustausch zwischen allen Netzwerken.

* Das virtuelle Azure-Netzwerk ist im lokalen Netzwerk und im CloudSimple-Netzwerk sichtbar.
* Wenn Sie eine Verbindung mit Ihrem virtuellen Azure-Netzwerk über das lokale Netzwerk hergestellt haben, ermöglicht die Verbindung mit dem CloudSimple-Netzwerk mithilfe von Global Reach den Zugriff auf virtuelle Netzwerke über das CloudSimple-Netzwerk.
* Subnetzadressen **dürfen sich nicht** zwischen den verbundenen Netzwerken überschneiden.
* CloudSimple kündigt die Standardroute **nicht** für ExpressRoute-Verbindungen an
* Wenn Ihr lokaler Router die Standardroute ankündigt, verwendet der Datenverkehr aus dem CloudSimple-Netzwerk und dem virtuellen Azure-Netzwerk die angekündigte Standardroute.  Daher kann auf virtuelle Computer in Azure nicht mit öffentlichen IP-Adressen zugegriffen werden.

## <a name="next-steps"></a>Nächste Schritte

* [Verbinden des virtuellen Azure-Netzwerks mit CloudSimple mithilfe von ExpressRoute](virtual-network-connection.md)
* [Herstellen einer Verbindung von lokalen Standorten mit CloudSimple mithilfe von ExpressRoute](on-premises-connection.md)
