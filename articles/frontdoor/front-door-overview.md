---
title: Azure Front Door Service | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht zu Azure Front Door. Erfahren Sie, ob dies die richtige Wahl für den Lastenausgleich des Benutzerdatenverkehrs für Ihre Anwendung ist.
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/08/2019
ms.author: sharadag
ms.openlocfilehash: 96dae96e16ce033ce15a8f1e9386e5252562654a
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73796225"
---
# <a name="what-is-azure-front-door-service"></a>Was ist Azure Front Door Service?
Über Azure Front Door Service können Sie das globale Routing für Ihren Webdatenverkehr definieren, verwalten und überwachen, indem Sie es für beste Leistung und sofortiges globales Failover bei Hochverfügbarkeit optimieren. Mit Front Door können Sie Ihre globalen (mehrere Regionen) Consumer- und Unternehmensanwendungen in stabile und hochleistungsfähige personalisierte moderne Anwendungen, APIs und Inhalte transformieren, die eine globale Zielgruppe mit Azure erreichen.

Front Door arbeitet auf Layer 7 oder HTTP/HTTPS-Layer und verwendet das Anycast-Protokoll mit geteiltem TCP und dem globalen Netzwerk von Microsoft zur Verbesserung der globalen Konnektivität. So können Sie – je nach Ihrer Auswahl der Routingmethode in der Konfiguration – sicherstellen, dass Front Door Ihre Clientanforderungen an das schnellste und am häufigsten verfügbare Anwendungs-Back-End routet. Ein Anwendungs-Back-End ist ein Dienst mit Internetzugriff, der innerhalb oder außerhalb von Azure gehostet wird. Front Door bietet eine Reihe von [Datenverkehrsrouting-Methoden](front-door-routing-methods.md) und [Back-End-Systemüberwachungsoptionen](front-door-health-probes.md), die verschiedene Anwendungsanforderungen und automatische Failovermodelle erfüllen. Ähnlich wie [Traffic Manager](../traffic-manager/traffic-manager-overview.md) zeichnet sich Front Door durch eine geringe Fehleranfälligkeit aus, selbst wenn es zum Ausfall einer ganzen Azure-Region kommt.

>[!NOTE]
> Azure stellt eine Sammlung aus vollständig verwalteten Lastenausgleichslösungen für Ihre Szenarios bereit. Wenn Sie nach Informationen zu einem DNS-basierten globalen Routing suchen und **keine** Anforderungen zur Beendigung der Transport Layer Security (TLS)-Protokollierung oder zur Verarbeitung der Anwendungsschicht pro HTTP/HTTPS-Anforderung suchen, lesen Sie den Artikel [Traffic Manager](../traffic-manager/traffic-manager-overview.md). Wenn Sie nach Informationen zum Lastenausgleich zwischen Ihren Servern in einer Region suchen, lesen Sie im Zusammenhang mit der Anwendungsschicht den Artikel [Application Gateway](../application-gateway/application-gateway-introduction.md) und im Zusammenhang mit der Vermittlungsschicht den Artikel [Load Balancer](../load-balancer/load-balancer-overview.md). Für Ihre End-to-End-Szenarios kann es von Vorteil sein, diese Lösungen nach Bedarf zu kombinieren.

Die folgenden Features sind in Front Door enthalten:

## <a name="accelerate-application-performance"></a>Beschleunigen der Anwendungsleistung
Durch Verwendung des TCP-basierten geteilten Anycast-Protokolls stellt Front Door sicher, dass Ihre Endbenutzer sofort mit dem nächstgelegenen Front Door-POP (Point of Presence) verbunden werden. Durch Nutzung des globalen Netzwerks von Microsoft zum Verbinden Ihrer Anwendungs-Back-Ends aus Front Door-POPs sorgen Sie für eine höhere Verfügbarkeit und Zuverlässigkeit bei gleichzeitiger Aufrechterhaltung der Leistung. Diese Konnektivität mit Ihrem Back-End basiert außerdem auf der geringsten Netzwerklatenz. Erfahren Sie mehr zu Front Door-Routingmethoden wie [Geteiltes TCP](front-door-routing-architecture.md#splittcp) und [Anycast-Protokoll](front-door-routing-architecture.md#anycast).

## <a name="increase-application-availability-with-smart-health-probes"></a>Erhöhen der Anwendungsverfügbarkeit mit intelligenten Integritätstests

Front Door bietet Hochverfügbarkeit für Ihre kritischen Anwendungen. Dazu verwendet der Dienst intelligente Integritätstests, überwacht Ihre Back-Ends auf Latenz und Verfügbarkeit und sorgt für sofortiges automatisches Failover, wenn ein Back-End ausfällt. Deshalb können Sie geplante Wartungsarbeiten für Ihre Anwendungen ohne Ausfallzeiten ausführen. Front Door leitet den Datenverkehr während der Wartung an andere Back-Ends weiter.

## <a name="url-based-routing"></a>URL-basiertes Routing
Mit URL-pfadbasiertem Routing können Sie Datenverkehr auf Basis von URL-Pfaden der Anforderung an Back-End-Pools weiterleiten. Ein mögliches Szenario ist die Weiterleitung von Anforderungen für unterschiedliche Inhaltstypen an verschiedene Back-End-Pools.

Beispielsweise werden Anforderungen von `http://www.contoso.com/users/*` an „UserProfilePool“ und von `http://www.contoso.com/products/*` an „ProductInventoryPool“ weitergeleitet.  Front Door ermöglicht sogar komplexere Routenabgleichsszenarien mithilfe eines Best-Match-Algorithmus. Wenn also keines der Pfadmuster übereinstimmt, wird Ihre Standardroutingregel für `http://www.contoso.com/*` ausgewählt und der Datenverkehr an die Catch-All-Standardroutingregel geleitet. Weitere Informationen finden Sie unter [Routenabgleich](front-door-route-matching.md).

## <a name="multiple-site-hosting"></a>Hosten mehrerer Websites
Das Hosten mehrerer Websites ermöglicht es Ihnen, mehr als eine Website in derselben Front Door-Konfiguration zu konfigurieren. Mit diesem Feature können Sie eine effizientere Topologie für Ihre Bereitstellungen konfigurieren, indem Sie einer einzelnen Front Door-Konfiguration verschiedene Websites hinzufügen. Basierend auf der Architektur Ihrer Anwendung können Sie Azure Front Door Service so konfigurieren, dass entweder jede Website an ihren eigenen Back-End-Pool geleitet wird oder dass verschiedene Websites an denselben Back-End-Pool geleitet werden. Beispielsweise kann Front Door den Datenverkehr für `images.contoso.com` und `videos.contoso.com` aus zwei Back-End-Pools namens „ImagePool“ und „VideoPool“ erledigen. Alternativ können Sie beide Front-End-Hosts so konfigurieren, dass der Datenverkehr an einen einzigen Back-End-Pool namens „MediaPool“ geleitet wird.

Entsprechend können zwei verschiedene Domänen (`www.contoso.com` und `www.fabrikam.com`) in demselben Front Door konfiguriert werden.

## <a name="session-affinity"></a>Sitzungsaffinität
Die cookiebasierte Sitzungsaffinität ist ein hilfreiches Feature, mit dem eine Benutzersitzung auf demselben Anwendungs-Back-End fortgesetzt werden kann. Durch die Nutzung von Cookies, die durch Front Door verwaltet werden, wird nachfolgender Datenverkehr von einer Benutzersitzung an dasselbe Anwendungs-Back-End zur Verarbeitung geleitet. Dieses Feature ist wichtig, wenn der Sitzungsstatus für eine Benutzersitzung lokal auf dem Back-End gespeichert wird.

## <a name="secure-sockets-layer-ssl-termination"></a>SSL-Terminierung (Secure Sockets Layer)
Front Door unterstützt SSL-Terminierung auf Edge-Ebene, d. h., einzelne Benutzer können eine SSL-Verbindung mit Front Door-Umgebungen einrichten, statt sie über lange Transportverbindungen mit dem Anwendungs-Back-End herzustellen. Darüber hinaus unterstützt Front Door sowohl HTTP- als auch HTTPS-Konnektivität zwischen Front Door-Umgebungen und Ihren Back-Ends. Deshalb können Sie auch End-to-End-SSL-Verschlüsselung einrichten. Wenn Front Door für Ihre Arbeitsauslastung beispielsweise infolge der Wiederverwendung einer betriebsbereiten Verbindung innerhalb einer Minute über 5.000 Anforderungen für aktive Dienste empfängt, stellt es ungefähr 500 Verbindungen mit Ihrem Anwendungs-Back-End her, wodurch ihre Back-Ends erheblich entlastet werden.

## <a name="custom-domains-and-certificate-management"></a>Benutzerdefinierte Domänen und Zertifikatverwaltung
Wenn Sie Front Door zum Bereitstellen von Inhalten verwenden, ist eine benutzerdefinierte Domäne erforderlich, falls Ihr eigener Domänenname in Ihrer Front Door-URL sichtbar sein soll. Die Verwendung eines sichtbaren Domänennamens kann für Ihre Kunden komfortabel und für Branding-Zwecke hilfreich sein.
Front Door unterstützt auch HTTPS für benutzerdefinierte Domänennamen. Verwenden Sie dieses Feature, indem Sie entweder durch Front Door verwaltete Zertifikate für Ihren Datenverkehr wählen oder Ihr eigenes benutzerdefiniertes SSL-Zertifikat hochladen.

## <a name="application-layer-security"></a>Sicherheit für Anwendungsschichten
Azure Front Door ermöglicht Ihnen, WAF-Regeln (Web Application Firewall) für Zugriffssteuerung zu erstellen, um Ihre HTTP/HTTPS-Workload vor Missbrauch auf Basis von Client-IP-Adressen, Landeskennzahl und HTTP-Parametern zu schützen. Darüber hinaus ermöglicht es Ihnen Front Door, Ratenbegrenzungsregeln zur Bekämpfung von schädlichem Bot-Datenverkehr zu erstellen. Weitere Informationen zu Web Application Firewall finden Sie unter [Was ist die Azure Web Application Firewall?](../web-application-firewall/overview.md).

Die Door-Plattform selbst wird durch [Azure DDoS Protection](../virtual-network/ddos-protection-overview.md) Basic geschützt. Zum weiteren Schutz kann Azure DDoS Protection Standard auf Ihren VNETs aktiviert werden und Ressourcen vor Vermittlungsschichtangriffen (TCP/UDP) über automatische Optimierung und Risikominderung schützen. Front Door ist ein Layer 7-Reverseproxy. Er erlaubt nur Webdatenverkehr über Back-Ends und blockiert standardmäßig andere Arten von Datenverkehr.

## <a name="url-redirection"></a>URL-Umleitung
Da die Branche darauf drängt, nur sichere Kommunikation zu unterstützen, wird erwartet, dass Webanwendungen jeden HTTP-Datenverkehr automatisch zu HTTPS umleiten. Dadurch wird sichergestellt, dass die gesamte Kommunikation zwischen Benutzern und Anwendung über einen verschlüsselten Pfad stattfindet. 

In der Regel haben Anwendungsbesitzer diese Anforderung durch Erstellen eines dedizierten Diensts gelöst, dessen einziger Zweck darin bestand, über HTTP empfangene Anfragen zu HTTPS umzuleiten. Azure Front Door Service unterstützt die Möglichkeit, Datenverkehr von HTTP zu HTTPS umzuleiten. Dies vereinfacht die Anwendungskonfiguration, optimiert die Ressourcennutzung und ermöglicht neue Umleitungsszenarien wie etwa die globale und pfadbasierte Umleitung. Die URL-Umleitung von Azure Front Door Service beschränkt sich nicht auf die HTTP-zu-HTTPS-Umleitung, sondern unterstützt auch die Umleitung zu einem anderen Hostnamen, zu einem anderen Pfad oder sogar zu einer neuen Abfragezeichenfolge in der URL.

Weitere Informationen finden Sie unter [Umleiten von Datenverkehr](front-door-url-redirect.md) mit Azure Front Door Service.

## <a name="url-rewrite"></a>URL Rewrite
Front Door unterstützt [URL Rewrite](front-door-url-rewrite.md), indem es Ihnen ermöglicht, einen optionalen benutzerdefinierten Weiterleitungspfad zu konfigurieren, der beim Erstellen der Anforderung zum Weiterleiten an das Back-End verwendet werden soll. Außerdem ermöglicht es Ihnen Front Door, einen Hostheader so zu konfigurieren, dass er beim Weiterleiten der Anforderung an Ihr Back-End gesendet wird.

## <a name="protocol-support---ipv6-and-http2-traffic"></a>Protokollunterstützung – IPv6- und HTTP/2-Datenverkehr
Azure Front Door unterstützt nativ End-to-End-IPv6-Konnektivität und auch das HTTP/2-Protokoll. 

Das HTTP/2-Protokoll ermöglicht die Vollduplexkommunikation zwischen Anwendungs-Back-Ends und einem Client über eine TCP-Verbindung mit langer Laufzeit. HTTP/2 ermöglicht mehr Interaktivität bei der Kommunikation zwischen dem Back-End und dem Client, da die Kommunikation auch ohne die bei HTTP-basierten Implementierungen erforderlichen Abfragen bidirektional sein kann. Im Vergleich zu HTTP zeichnet sich das HTTP/2-Protokoll durch einen geringen Mehraufwand aus. Außerdem kann es dieselbe TCP-Verbindung für mehrere Anforderungen oder Antworten verwenden, was eine effizientere Ressourcennutzung zur Folge hat. Erfahren Sie mehr über [HTTP/2-Unterstützung in Azure Front Door Service](front-door-http2.md).

## <a name="pricing"></a>Preise

Preisinformationen finden Sie unter [Front Door – Preise](https://azure.microsoft.com/pricing/details/frontdoor/).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Azure Front Door Service-Konfiguration](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
