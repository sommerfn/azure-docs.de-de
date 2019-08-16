---
title: Netzwerkoptionen von Azure Functions
description: Enthält eine Übersicht über alle Netzwerkoptionen, die in Azure Functions verfügbar sind.
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: f4f081001f2573bccc58205ccc7955739b7f5c4c
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779292"
---
# <a name="azure-functions-networking-options"></a>Netzwerkoptionen von Azure Functions

In diesem Artikel werden die Netzwerkfunktionen beschrieben, die für die Hostingoptionen von Azure Functions zur Verfügung stehen. Alle folgenden Netzwerkoptionen verfügen über die Möglichkeit zum Zugreifen auf Ressourcen ohne Verwendung von Adressen, die über das Internet geroutet werden können, oder die Einschränkung des Internetzugriffs auf eine Funktions-App. 

Die Hostingmodelle weisen verschiedene verfügbare Ebenen der Netzwerkisolation auf. Durch Auswahl der jeweils richtigen Ebene lassen sich Ihre Anforderungen an die Netzwerkisolation erfüllen.

Sie können Funktions-Apps auf verschiedene Arten hosten:

* Es gibt eine Gruppe von Tarifoptionen, die in einer mehrinstanzenfähigen Infrastruktur ausgeführt werden und über unterschiedliche Ebenen von Konnektivitäts- und Skalierungsoptionen für virtuelle Netzwerke verfügen:
    * Der [Verbrauchstarif](functions-scale.md#consumption-plan), der als Reaktion auf Last dynamisch skaliert wird und nur über Optionen für eine minimale Netzwerkisolation verfügt.
    * Der [Premium-Tarif](functions-scale.md#premium-plan), bei dem ebenfalls dynamisch skaliert wird, der aber eine umfassendere Netzwerkisolation ermöglicht.
    * Der [Azure App Service-Plan](functions-scale.md#app-service-plan), bei dem eine feste Skalierung verwendet wird und der eine ähnliche Netzwerkisolation wie der Premium-Tarif aufweist.
* Sie können Funktionen in einer [App Service-Umgebung](../app-service/environment/intro.md) ausführen. Mit dieser Methode werden die Funktionen in Ihrem virtuellen Netzwerk bereitgestellt und eine umfassende Netzwerksteuerung und -isolation ermöglicht.

## <a name="matrix-of-networking-features"></a>Matrix der Netzwerkfunktionen

|                |[Verbrauchstarif](functions-scale.md#consumption-plan)|[Premium-Plan (Vorschauversion)](functions-scale.md#premium-plan)|[App Service-Plan](functions-scale.md#app-service-plan)|[App Service-Umgebung](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[IP-Einschränkungen für eingehenden Datenverkehr und Zugriff auf private Sites](#inbound-ip-restrictions)|✅ Ja|✅ Ja|✅ Ja|✅ Ja|
|[Integration in ein virtuelles Netzwerk](#virtual-network-integration)|❌ Nein|✅ Ja (regional)|✅ Ja (regional und Gateway)|✅ Ja|
|[Trigger für virtuelle Netzwerke (nicht HTTP)](#virtual-network-triggers-non-http)|❌ Nein| ❌ Nein|✅ Ja|✅ Ja|
|[Hybridverbindungen](#hybrid-connections)|❌ Nein|❌ Nein|✅ Ja|✅ Ja|
|[IP-Einschränkungen für ausgehenden Datenverkehr](#outbound-ip-restrictions)|❌ Nein| ❌ Nein|❌ Nein|✅ Ja|


## <a name="inbound-ip-restrictions"></a>IP-Einschränkungen für eingehenden Datenverkehr

Mit IP-Einschränkungen können Sie eine nach Priorität sortierte Liste mit IP-Adressen definieren, über die der Zugriff auf Ihre App zugelassen bzw. abgelehnt wird. Die Liste kann IPv4- und IPv6-Adressen enthalten. Wenn mindestens ein Eintrag vorhanden ist, enthält die Liste am Ende einen impliziten Eintrag vom Typ „Alle ablehnen“. IP-Einschränkungen können für alle Hostingoptionen für Funktionen verwendet werden.

> [!NOTE]
> Bei vorhandenen Netzwerkeinschränkungen können Sie den Portal-Editor nur innerhalb Ihres virtuellen Netzwerks verwenden oder wenn Sie die IP-Adresse des Computers, den Sie für den Zugriff auf das Azure-Portal verwenden, in die Whitelist aufgenommen haben. Sie können aber weiterhin von jedem Computer aus auf alle Funktionen der Registerkarte **Plattformfeatures** zugreifen.

Weitere Informationen finden Sie unter [Azure App Service – statische Zugriffseinschränkungen](../app-service/app-service-ip-restrictions.md).

## <a name="private-site-access"></a>Privater Websitezugriff

Privater Websitezugriff bezieht sich darauf, den Zugriff auf Ihre App nur über ein privates Netzwerk zuzulassen, z. B. über ein virtuelles Azure-Netzwerk. 
* Zugriff auf private Sites ist im [Premium](./functions-premium-plan.md)- und [App Service-Plan](functions-scale.md#app-service-plan) verfügbar, wenn **Dienstendpunkte** konfiguriert sind. Weitere Informationen finden Sie unter [VNET-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md).
    * Bedenken Sie, dass Ihre Funktion auch mit Dienstendpunkten immer noch vollständigen ausgehenden Zugriff auf das Internet besitzt, selbst wenn die virtuelle Netzwerkintegration konfiguriert ist.
* Der private Websitezugriff ist auch verfügbar, wenn eine App Service-Umgebung mit einem internen Lastenausgleich (ILB) konfiguriert ist. Weitere Informationen finden Sie unter [Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung](../app-service/environment/create-ilb-ase.md).

## <a name="virtual-network-integration"></a>Integration in ein virtuelles Netzwerk

Durch die Integration des virtuellen Netzwerks kann die Funktions-App auf Ressourcen eines virtuellen Netzwerks zugreifen. Diese Funktion ist sowohl für den Premium-Tarif als auch für den App Service-Plan verfügbar. In einer App Service-Umgebung befindet sich Ihre App bereits in einem virtuellen Netzwerk, sodass die Integration des virtuellen Netzwerks nicht erforderlich ist, um Ressourcen im selben virtuellen Netzwerk zu erreichen.

Mithilfe der Integration des virtuellen Netzwerks kann der Zugriff von Apps auf Datenbanken und Webdienste im virtuellen Netzwerk ermöglicht werden. Mit der Integration des virtuellen Netzwerks müssen Sie keinen öffentlichen Endpunkt für Anwendungen auf Ihrem virtuellen Computer verfügbar machen. Stattdessen können Sie die privaten Adressen verwenden, die nicht über das Internet geroutet werden.

Es gibt zwei Versionen der Funktion für die virtuelle Netzwerkintegration:

1. Die regionale Integration virtueller Netzwerke ermöglicht die Integration in virtuelle Netzwerke in derselben Region. Diese Version der Funktion erfordert ein Subnetz in einem virtuellen Netzwerk in der gleichen Region. Diese Funktion ist noch in der Vorschau, wird aber für Windows-App-Produktionsworkloads unterstützt, wobei einige Einschränkungen unten aufgeführt sind.
2. Die Integration virtueller Netzwerke mit erforderlichem Gateway ermöglicht die Integration in virtuelle Netzwerke in Remoteregionen oder in klassische virtuelle Netzwerke. Diese Version erfordert die Bereitstellung eines virtuellen Netzwerkgateways in Ihrem VNET. Dies ist das Feature, das auf einer Point-to-Site-VPN-Verbindung basiert und nur für Windows-Apps unterstützt wird.

Für eine App kann jeweils nur eine Version der Funktion für die VNET-Integration verwendet werden. Daher stellt sich die Frage, welche Version für Sie die richtige Lösung ist. Beide Versionen können für viele Zwecke verwendet werden. Es gibt jedoch einige wesentliche Unterschiede:

| Problem  | Lösung | 
|----------|----------|
| Sie möchten eine RFC 1918-Adresse (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) in der gleichen Region erreichen. | Regionale VNET-Integration |
| Sie möchten Ressourcen in einem klassischen VNET oder einem VNET in einer anderen Region erreichen. | VNET-Integration, die ein Gateway erfordert |
| Sie möchten RFC 1918-Endpunkte über ExpressRoute erreichen. | Regionale VNET-Integration |
| Sie möchten Ressourcen über Dienstendpunkte erreichen. | Regionale VNET-Integration |

RFC 1918-fremde Adressen können mit keiner Version der Funktion über ExpressRoute erreicht werden. Dazu müssen Sie gegenwärtig eine ASE verwenden.

Bei der regionalen VNET-Integration wird Ihr VNET weder mit dem lokalen Netzwerk verbunden noch werden Dienstendpunkte konfiguriert. Dafür ist eine separate Netzwerkkonfiguration erforderlich. Die regionale VNET-Integration ermöglicht es Ihrer App lediglich, Aufrufe für diese Verbindungstypen auszuführen.

Unabhängig von der verwendeten Version ermöglicht die VNet-Integration Ihrer Funktions-App den Zugriff auf Ressourcen in Ihrem virtuellen Netzwerk, gewährt aber keinen privaten Websitezugriff auf Ihre Funktions-App aus dem virtuellen Netzwerk. Privater Websitezugriff bezieht sich darauf, den Zugriff auf Ihre App nur über ein privates Netzwerk zuzulassen, z.B. aus einem virtuellen Azure-Netzwerk heraus. Die VNET-Integration dient nur zum Ausführen ausgehender Aufrufe von Ihrer App in Ihr VNET. 

Für die Funktion für die VNET-Integration gilt Folgendes:

* Erfordert einen Standard-, Premium- oder PremiumV2-App Service-Plan.
* Unterstützt TCP und UDP.
* Funktioniert mit App Service-Apps und Funktions-Apps.

Einige Dinge werden von der VNET-Integration nicht unterstützt, z.B.:

* Bereitstellung eines Laufwerks
* AD-Integration 
* NetBios

Bei der Integration virtueller Netzwerke in Functions wird die gemeinsame Infrastruktur mit App Service-Web-Apps verwendet. Weitere Informationen zu den beiden Arten der Integration virtueller Netzwerke finden Sie unter:
* [Regionale VNET-Integration](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [VNet-Integration, die ein Gateway erfordert](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

Weitere Informationen zur Verwendung der Integration virtueller Netzwerke finden Sie unter [Integrieren einer Funktions-App in ein virtuelles Azure-Netzwerk](functions-create-vnet.md).

## <a name="virtual-network-triggers-non-http"></a>Trigger für virtuelle Netzwerke (nicht HTTP)

Damit Sie in der Lage sind, andere Funktionstrigger als HTTP in einem virtuellen Netzwerk zu verwenden, müssen Sie zurzeit Ihre Funktions-App in einem App Service-Plan oder in einer App Service-Umgebung ausführen.

Beispiel: Wenn Sie Azure Cosmos DB so konfigurieren möchten, dass nur Datenverkehr aus einem virtuellen Netzwerk akzeptiert wird, müssen Sie Ihre Funktions-App im Rahmen eines App Service-Plans mit virtueller Netzwerkintegration in diesem virtuellen Netzwerk bereitstellen, um Azure Cosmos DB-Trigger von dieser Ressource zu konfigurieren. In der Vorschauphase gestattet das Konfigurieren der VNET-Integration nicht, dass der Premium-Plan aus dieser Azure Cosmos DB-Ressource heraus auslösen kann.

Sehen Sie in [dieser Liste aller Nicht-HTTP-Trigger](./functions-triggers-bindings.md#supported-bindings) nach, um zu überprüfen, was unterstützt wird.

## <a name="hybrid-connections"></a>Hybridverbindungen

[Hybrid Connections](../service-bus-relay/relay-hybrid-connections-protocol.md) ist ein Feature von Azure Relay, das Sie zum Zugreifen auf Anwendungsressourcen in anderen Netzwerken verwenden können. Es ermöglicht den Zugriff von Ihrer App auf einen Anwendungsendpunkt. Sie können es nicht verwenden, um auf Ihre Anwendung zuzugreifen. Hybrid Connections ist für Funktionen verfügbar, die unter einem [App Service-Plan](functions-scale.md#app-service-plan) und in einer [App Service-Umgebung](../app-service/environment/intro.md) ausgeführt werden.

Bei der Verwendung in Azure Functions entspricht jede Hybridverbindung einer Kombination aus einem einzelnen TCP-Host und einem Port. Dies bedeutet, dass sich der Hybridverbindungsendpunkt in einem beliebigen Betriebssystem und einer beliebigen Anwendung befinden kann, solange der Zugriff über einen TCP-Lauschport erfolgt. Das Feature „Hybrid Connections“ verfügt nicht über Informationen zum Anwendungsprotokoll oder zum abzurufenden Inhalt und benötigt diese Informationen auch nicht. Es ermöglicht lediglich den Netzwerkzugriff.

Weitere Informationen finden Sie in der [App Service-Dokumentation zu Hybrid Connections](../app-service/app-service-hybrid-connections.md). Functions wird unter einem App Service-Tarif unterstützt.

## <a name="outbound-ip-restrictions"></a>IP-Einschränkungen für ausgehenden Datenverkehr

IP-Einschränkungen für ausgehenden Datenverkehr sind nur für Funktionen verfügbar, die in einer App Service-Umgebung bereitgestellt werden. Sie können ausgehende Einschränkungen für das virtuelle Netzwerk konfigurieren, wo Ihre App Service-Umgebung bereitgestellt wird.

Wenn Sie eine Funktions-App in einem Premium-Plan oder App Service-Plan in ein virtuelles Netzwerk integrieren, kann die App weiterhin ausgehende Aufrufe an das Internet durchführen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Netzwerk und zu Azure Functions: 

* [Tutorial zum Einstieg in die Integration des virtuellen Netzwerks](./functions-create-vnet.md)
* [Häufig gestellte Fragen zu Netzwerken in Functions](./functions-networking-faq.md)
* [Weitere Informationen zur Integration des virtuellen Netzwerks mit App Service und Functions](../app-service/web-sites-integrate-with-vnet.md)
* [Weitere Informationen zu virtuellen Netzwerken in Azure](../virtual-network/virtual-networks-overview.md)
* [Aktivieren weiterer Netzwerkfunktionen und Steuerung mit App Service-Umgebungen](../app-service/environment/intro.md)
* [Verbinden mit einzelnen lokalen Ressourcen ohne Änderungen an der Firewall mithilfe von Hybrid Connections](../app-service/app-service-hybrid-connections.md)
