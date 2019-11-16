---
title: 'Azure ExpressRoute: Umstellen von öffentlichem Peering auf Microsoft-Peering'
description: In diesem Artikel erfahren Sie, wie Sie Ihr öffentliches Peering über ExpressRoute auf Microsoft-Peering umstellen.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 03/12/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: f9ed841d347d99a2208971d164bbd7b9044e3b09
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74031600"
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Umstellen von öffentlichem Peering auf Microsoft-Peering

Dieser Artikel erklärt, wie Sie ohne Ausfallzeiten von einer öffentlichen Peeringkonfiguration auf Microsoft-Peering umsteigen. ExpressRoute unterstützt die Verwendung von Microsoft-Peering und Routenfiltern für Azure-PaaS-Dienste wie Azure Storage und Azure SQL-Datenbank. Für den Zugriff auf PaaS- und SaaS-Dienste von Microsoft ist nun nur noch eine einzelne Routingdomäne erforderlich. Mit Routenfiltern können Sie selektiv die PaaS-Dienstpräfixe für die zu verwendenden Azure-Regionen ankündigen.

Im öffentlichen Azure-Peering ist jeder BGP-Sitzung eine NAT-IP-Adresse zugeordnet. Microsoft-Peering ermöglicht Ihnen, Ihre eigenen NAT-Zuordnungen zu konfigurieren und Routenfilter für selektive Präfixankündigungen zu verwenden. Öffentliches Peering ist ein unidirektionaler Dienst, bei dem die Konnektivität zu den Microsoft Azure-Diensten immer von Ihrem WAN aus initiiert wird. Microsoft Azure-Dienste können über diese Routingdomäne keine Verbindungen in Ihr Netzwerk initiieren.

Sobald öffentliches Peering aktiviert ist, werden Sie eine Verbindung mit allen Azure-Diensten herstellen können. Es ist Ihnen nicht gestattet, selektiv Dienste auszuwählen, zu denen wir Routen ankündigen. Das Microsoft-Peering stellt eine bidirektionale Konnektivität dar, die vom Microsoft Azure-Dienst und Ihrem WAN initiiert werden kann. Weitere Informationen zu Routingdomänen und Peering finden Sie unter [ExpressRoute-Verbindungen und Routingdomänen](expressroute-circuit-peerings.md).

## <a name="before"></a>Voraussetzungen

Um eine Verbindung mit Microsoft-Peering herstellen zu können, müssen Sie die NAT einrichten und verwalten. Die NAT wird unter Umständen von Ihrem Konnektivitätsanbieter eingerichtet und verwaltet. Wenn Sie im Rahmen von Microsoft-Peering auf die Azure-PaaS- und Azure-SaaS-Dienste zugreifen möchten, muss der NAT-IP-Adresspool die richtige Größe haben. Weitere Informationen zur NAT für ExpressRoute finden Sie unter [NAT-Anforderungen für Microsoft-Peering](expressroute-nat.md#nat-requirements-for-microsoft-peering). Wenn Sie über Azure ExpressRoute (Microsoft-Peering) eine Verbindung mit Microsoft herstellen, verfügen Sie über mehrere Verbindungen mit Microsoft. Ihre vorhandene Internetverbindung und die ExpressRoute-Verbindung. Ein Teil des Datenverkehrs an Microsoft kann über die Internetverbindung gesendet und über ExpressRoute empfangen werden (oder umgekehrt).

![Bidirektionale Konnektivität](./media/how-to-move-peering/bidirectional-connectivity.jpg)

> [!Warning]
> Der Microsoft angekündigte NAT-IP-Adresspool muss nicht im Internet angekündigt werden. Ansonsten wird die Verbindung mit anderen Microsoft-Diensten unterbrochen.

Informieren Sie sich unter [Asymmetrisches Routing mit mehreren Netzwerkpfaden](https://docs.microsoft.com/azure/expressroute/expressroute-asymmetric-routing) über die Einschränkungen des asymmetrischen Routings, bevor Sie das Microsoft-Peering konfigurieren.

* Wenn Sie öffentliches Peering verwenden und derzeit über IP-Netzwerkregeln für öffentliche IP-Adressen verfügen, mit denen auf [Azure Storage](../storage/common/storage-network-security.md) oder [Azure SQL-Datenbank](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md) zugegriffen wird, müssen Sie sicherstellen, dass der mit Microsoft-Peering konfigurierte NAT-IP-Adressenpool in der Liste der öffentlichen IP-Adressen für das Azure Storage- oder Azure SQL-Konto enthalten ist.<br>
* Führen Sie die Schritte in diesem Artikel in der angegebenen Reihenfolge aus, damit es bei der Umstellung auf Microsoft-Peering zu keinen Ausfällen kommt.

## <a name="create"></a>1. Erstellen von Microsoft-Peering

Falls Microsoft-Peering noch nicht erstellt wurde, gehen Sie gemäß einem der folgenden Artikel vor, um es zu erstellen. Wenn Ihr Konnektivitätsanbieter verwaltete Layer 3-Dienste anbietet, können Sie ihn mit der Aktivierung des Microsoft-Peerings für Ihre Verbindung beauftragen.

Wenn die Ebene 3 von Ihnen verwaltet wird, benötigen Sie die folgenden Informationen, bevor Sie fortfahren:

* Ein /30-Subnetz für die primäre Verknüpfung. Dies muss ein gültiges öffentliches IPv4-Präfix sein, das sich in Ihrem Besitz befindet und über eine RIR/IRR-Registrierung verfügt. Über dieses Subnetz weisen Sie die erste verwendbare IP-Adresse für Ihren Router zu, da die zweite verwendbare IP-Adresse von Microsoft für den eigenen Router genutzt wird.<br>
* Ein /30-Subnetz für die sekundäre Verknüpfung. Dies muss ein gültiges öffentliches IPv4-Präfix sein, das sich in Ihrem Besitz befindet und über eine RIR/IRR-Registrierung verfügt. Über dieses Subnetz weisen Sie die erste verwendbare IP-Adresse für Ihren Router zu, da die zweite verwendbare IP-Adresse von Microsoft für den eigenen Router genutzt wird.<br>
* Eine gültige VLAN-ID zum Einrichten dieses Peerings. Stellen Sie sicher, dass kein anderes Peering der Verbindung die gleiche VLAN-ID verwendet. Sie müssen sowohl für primäre als auch für sekundäre Verknüpfungen die gleiche VLAN-ID verwenden.<br>
* AS-Nummer für Peering. Sie können sowohl AS-Nummern mit 2 Byte als auch mit 4 Byte verwenden.<br>
* Angekündigte Präfixe: Sie müssen eine Liste mit allen Präfixen bereitstellen, die Sie über die BGP-Sitzung ankündigen möchten. Nur öffentliche IP-Adresspräfixe werden akzeptiert. Wenn Sie planen, einen Satz mit Präfixen zu senden, können Sie eine durch Komma getrennte Liste senden. Diese Präfixe müssen über eine RIR/IRR-Registrierung für Sie verfügen.<br>
* Routing-Registrierungsname: Sie können den RIR/IRR-Wert angeben, unter dem die AS-Nummer und die Präfixe registriert sind.

* **Optional:** Kunden-ASN: Wenn Sie Präfixe ankündigen, die nicht für die Peering-AS-Nummer registriert sind, können Sie die AS-Nummer angeben, unter der sie registriert sind.<br>
* **Optional:** Einen MD5-Hash, wenn Sie sich für dessen Einsatz entscheiden.

Eine ausführliche Anleitung zum Aktivieren des Microsoft-Peerings finden Sie in den folgenden Artikeln:

* [Erstellen von Microsoft-Peering über das Azure-Portal](expressroute-howto-routing-portal-resource-manager.md#msft)<br>
* [Erstellen von Microsoft-Peering mithilfe von Azure-PowerShell](expressroute-howto-routing-arm.md#msft)<br>
* [Erstellen von Microsoft-Peering mithilfe der Azure-Befehlszeilenschnittstelle](howto-routing-cli.md#msft)

## <a name="validate"></a>2. Überprüfen des Aktivierungsstatus von Microsoft-Peering

Vergewissern Sie sich, dass Microsoft-Peering aktiviert ist und die angekündigten öffentlichen Präfixe konfiguriert sind.

* [Azure-Portal](expressroute-howto-routing-portal-resource-manager.md#getmsft)<br>
* [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)<br>
* [Azure-Befehlszeilenschnittstelle](howto-routing-cli.md#getmsft)

## <a name="routefilter"></a>3. Konfigurieren eines Routenfilters und Anfügen des Routenfilters an die Verbindung

Standardmäßig kündigen neue Microsoft-Peerings keine Präfixe an, solange kein Routenfilter an die Verbindung angefügt wurde. Beim Erstellen einer Routenfilterregel können Sie die Liste der Dienstcommunitys für Azure-Regionen angeben, die Sie für Azure-PaaS-Dienste nutzen möchten. Dies bietet Ihnen mehr Flexibilität zum Filtern von Routen entsprechend Ihrem Bedarf, wie im folgenden Screenshot gezeigt:

![Zusammenführen des öffentlichen Peerings](./media/how-to-move-peering/routefilter.jpg)

Gehen Sie zum Konfigurieren von Routenfiltern gemäß einem der folgenden Artikel vor:

* [Konfigurieren von Routenfiltern für Microsoft-Peering: Azure-Portal](how-to-routefilter-portal.md)<br>
* [Konfigurieren von Routenfiltern für Microsoft-Peering: PowerShell](how-to-routefilter-powershell.md)<br>
* [Konfigurieren von Routenfiltern für Microsoft-Peering: Azure CLI](how-to-routefilter-cli.md)

## <a name="delete"></a>4. Löschen des öffentlichen Peerings

Nachdem Sie sich vergewissert haben, dass das Microsoft-Peering konfiguriert ist und die Präfixe, die Sie nutzen möchten, ordnungsgemäß für Microsoft-Peering angekündigt werden, können Sie das öffentliche Peering löschen. Gehen Sie zum Löschen des öffentlichen Peerings gemäß einem der folgenden Artikel vor:

* [Löschen des öffentlichen Azure-Peerings über das Azure-Portal](expressroute-howto-routing-portal-resource-manager.md#deletepublic)<br>
* [Löschen des öffentlichen Azure-Peerings mithilfe von Azure PowerShell](expressroute-howto-routing-arm.md#deletepublic)<br>
* [Löschen des öffentlichen Azure-Peerings mithilfe der Befehlszeilenschnittstelle](howto-routing-cli.md#deletepublic)
  
## <a name="view"></a>5. Anzeigen der Peerings
  
Im Azure-Portal können Sie eine Liste mit allen ExpressRoute-Leitungen und -Peerings anzeigen. Weitere Informationen finden Sie unter [So zeigen Sie die Details zum Microsoft-Peering an](expressroute-howto-routing-portal-resource-manager.md#getmsft).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).
