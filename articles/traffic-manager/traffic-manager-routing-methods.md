---
title: 'Azure Traffic Manager: Routingmethoden für Datenverkehr'
description: Dieser Artikel erläutert Ihnen die verschiedenen Methoden für das Datenverkehrsrouting, die von Traffic Manager verwendet werden.
services: traffic-manager
author: asudbring
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: allensu
ms.openlocfilehash: 06ce7fb5d18920be6f71821b034dc13061c60032
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051462"
---
# <a name="traffic-manager-routing-methods"></a>Traffic Manager-Routingmethoden

Azure Traffic Manager unterstützt sechs Methoden für das Datenverkehrsrouting, um zu bestimmen, wie Netzwerkverkehr an die verschiedenen Dienstendpunkte weitergeleitet wird. Der Traffic Manager wendet für jedes Profil die Datenverkehrsrouting-Methode an, die ihm in der jeweils empfangenen DNS-Abfrage zugeordnet ist. Mit der Methode für das Datenverkehrsrouting wird festgelegt, welcher Endpunkt in der DNS-Antwort zurückgegeben wird.

Die folgenden vier Methoden für das Datenverkehrsrouting sind in Traffic Manager verfügbar:

* **[Priorität:](#priority)** Wählen Sie **Priorität** aus, wenn Sie einen primären Dienstendpunkt für sämtlichen Datenverkehr verwenden möchten. Stellen Sie Backups bereit, falls der primäre Endpunkt oder der Sicherungsendpunkt nicht verfügbar ist.
* **[Gewichtet:](#weighted)** Wählen Sie **Gewichtet**, wenn Sie Datenverkehr über eine Gruppe von Endpunkten verteilen möchten – gleichmäßig oder gemäß einer von Ihnen definierten Gewichtung.
* **[Leistung:](#performance)** Wählen Sie **Leistung** aus, wenn sich Ihre Endpunkte an unterschiedlichen geografischen Standorten befinden und Endbenutzer an den „nächstgelegenen“ Endpunkt (im Hinblick auf die geringste Netzwerklatenz) weitergeleitet werden sollen.
* **[Geografisch:](#geographic)** Wählen Sie **Geographisch** aus, damit Benutzer auf bestimmte Endpunkte (Azure, Extern oder Geschachtelt) geleitet werden, je nach dem geografischen Ursprungsort ihrer DNS-Abfrage. Dadurch können Traffic Manager-Kunden Szenarien unterstützen, in denen die Kenntnis der geografischen Region eines Benutzers und das Routing auf der Grundlage dieser Kenntnis wichtig sind. Beispiele dafür bilden etwa die Einhaltung von Datenhoheitsmandaten, die Verortung von Inhalten und Benutzererfahrungen und das Messen von Datenverkehr aus verschiedenen Regionen.
* **[MultiValue:](#multivalue)** Wählen Sie **MultiValue** für Traffic Manager-Profile aus, die nur IPv4-/IPv6-Adressen als Endpunkte enthalten können. Wenn eine Abfrage für dieses Profil empfangen wird, werden alle fehlerfreien Endpunkte zurückgegeben.
* **[Subnetz:](#subnet)** Wählen Sie die Datenverkehrsrouting-Methode **Subnetz** aus, um Gruppen von Endbenutzer-IP-Adressbereichen einem bestimmten Endpunkt in einem Traffic Manager-Profil zuzuordnen. Wenn eine Anforderung empfangen wird, wird der Endpunkt zurückgegeben, der für die Quell-IP-Adresse dieser Anforderung zugeordnet ist. 


Alle Traffic Manager-Profile beinhalten die Überwachung der Endpunktintegrität sowie automatisches Endpunktfailover. Weitere Informationen finden Sie unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md). Ein einzelnes Traffic Manager-Profil kann nur eine einzige Methode für das Datenverkehrsrouting verwenden. Sie können jederzeit eine andere Methode für das Datenverkehrsrouting für Ihr Profil auswählen. Änderungen werden innerhalb einer Minute angewendet, und es entstehen keine Ausfallzeiten. Methoden für das Datenverkehrsrouting können durch Verwendung geschachtelter Traffic Manager-Profile kombiniert werden. Durch die Schachtelung können ausgefeilte und flexible Konfigurationen für das Datenverkehrsrouting erstellt werden, um die Anforderungen größerer und komplexer Anwendungen zu erfüllen. Weitere Informationen finden Sie unter [Geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).

## <a name = "priority"></a>Prioritätsmethode für das Datenverkehrsrouting

Wenn Organisationen die Zuverlässigkeit ihrer Dienste gewährleisten möchten, stellen sie häufig einen oder mehrere Sicherungsdienste bereit, für den Fall, dass der primäre Dienst ausfällt. Mithilfe der prioritätsbasierten Methode für das Datenverkehrsrouting können Azure-Kunden dieses Failovermuster problemlos implementieren.

![Prioritätsbasierte Methode für das Datenverkehrsrouting in Azure Traffic Manager](media/traffic-manager-routing-methods/priority.png)

Das Traffic Manager-Profil enthält eine Prioritätenliste mit Dienstendpunkten. Standardmäßig sendet Traffic Manager den gesamten Datenverkehr an den primären Endpunkt (mit der höchsten Priorität). Wenn der primäre Endpunkt nicht verfügbar ist, leitet Traffic Manager den Datenverkehr an den sekundären Endpunkt weiter. Wenn weder der primäre noch der sekundäre Endpunkt verfügbar sind, wird der Datenverkehr an den dritten Endpunkt gesendet usw. Ob ein Endpunkt verfügbar ist, hängt vom konfigurierten Status (aktiviert oder deaktiviert) sowie von der fortlaufenden Endpunktüberwachung ab.

### <a name="configuring-endpoints"></a>Konfigurieren von Endpunkten

Mit Azure Resource Manager wird die Endpunktpriorität mithilfe der für jeden Endpunkt definierten Prioritätseigenschaft explizit konfiguriert. Für diese Eigenschaft wird ein Wert zwischen 1 und 1000 festgelegt. Niedrigere Werte stellen eine höhere Priorität dar. Der gleiche Prioritätswert kann nicht für mehrere Endpunkte konfiguriert werden. Diese Eigenschaft kann optional festgelegt werden. Wird sie nicht festgelegt, wird eine Standardpriorität basierend auf der Endpunktreihenfolge verwendet.

## <a name = "weighted"></a>Gewichtete Methode für das Datenverkehrsrouting
Bei der gewichteten Methode für das Datenverkehrsrouting wird der Datenverkehr gleichmäßig verteilt oder eine vordefinierte Gewichtung verwendet.

![Gewichtete Methode für das Datenverkehrsrouting in Azure Traffic Manager](media/traffic-manager-routing-methods/weighted.png)

Bei der gewichteten Methode für das Datenverkehrsrouting wird jedem Endpunkt im Rahmen der Traffic Manager-Profilkonfiguration eine Gewichtung zugewiesen. Die Gewichtung ist eine Ganzzahl zwischen 1 und 1000. Dieser Parameter ist optional. Wenn er nicht angegeben wird, wird die Standardgewichtung 1 verwendet. Je höher die Gewichtung, desto höher die Priorität.

Für jede empfangene DNS-Abfrage wählt Traffic Manager einen verfügbaren Endpunkt nach dem Zufallsprinzip aus. Die Wahrscheinlichkeit für die Auswahl eines Endpunkts basiert hierbei auf der Gewichtung, die allen verfügbaren Endpunkten zugewiesen wurde. Die Verwendung der gleichen Gewichtung für alle Endpunkte führt zu einer gleichmäßigen Verteilung des Datenverkehrs. Wenn für bestimmte Endpunkte eine höhere oder niedrigere Gewichtung verwendet wird, werden diese Endpunkte häufiger oder seltener in den DNS-Antworten zurückgegeben.

Durch die gewichtete Methode werden einige nützliche Szenarios ermöglicht:

* Allmähliches Anwendungsupgrade: Weisen Sie einen Prozentwert des Datenverkehrs zu, der an einen neuen Endpunkt weitergeleitet wird, und erhöhen Sie den Datenverkehr dann im Lauf der Zeit allmählich auf 100 Prozent.
* Anwendungsmigration zu Azure: Erstellen Sie ein Profil mit Azure-Endpunkten und externen Endpunkten. Passen Sie die Gewichtung der Endpunkte so an, dass die neuen Endpunkte bevorzugt werden.
* Cloudbursting für zusätzliche Kapazität: Erweitern Sie eine lokale Bereitstellung schnell in die Cloud, indem Sie sie hinter einem Traffic Manager-Profil zur Verfügung stellen. Wenn Sie zusätzliche Kapazität in der Cloud benötigen, können Sie weitere Endpunkte hinzufügen oder aktivieren. Geben Sie dann an, welcher Teil des Datenverkehrs an jeden Endpunkt gesendet wird.

Neben der Verwendung des Azure-Portals haben Sie auch die Möglichkeit, Gewichtungen mithilfe von Azure PowerShell, über die Befehlszeilenschnittstelle oder mithilfe von REST-APIs zu konfigurieren.

Hinweis: DNS-Antworten werden sowohl von Clients als auch von den rekursiven DNS-Servern zwischengespeichert, die von diesen Clients für ihre DNS-Abfragen verwendet werden. Dieses Zwischenspeichern hat möglicherweise Auswirkungen auf die gewichtete Datenverkehrsverteilungen. Wenn die Anzahl von Clients und rekursiven DNS-Servern hoch ist, funktioniert die Verteilung des Datenverkehrs wie erwartet. Wenn die Anzahl von Clients oder rekursiven DNS-Servern jedoch gering ist, kann diese Zwischenspeicherung die Verteilung des Datenverkehrs massiv verzerren.

Gängige Anwendungsfälle:

* Entwicklungs- und Testumgebungen
* Kommunikation zwischen Anwendungen
* Anwendungen mit einer kleinen Benutzerbasis, die eine gemeinsame rekursive DNS-Infrastruktur nutzen (beispielsweise die Mitarbeiter eines Unternehmens, in dem Verbindungen über einen Proxy hergestellt werden)

Diese Auswirkungen der DNS-Zwischenspeicherung gelten für alle DNS-basierten Systeme für das Datenverkehrsrouting, nicht nur für Azure Traffic Manager. In einigen Fällen lässt sich das Problem möglicherweise durch eine explizite Bereinigung des DNS-Cache umgehen. In anderen Fällen eignet sich eine alternative Methode für das Datenverkehrsrouting besser.

## <a name = "performance"></a>Leistungsorientierte Methode für das Datenverkehrsrouting

Die Reaktionsfähigkeit vieler Anwendungen kann verbessert werden, indem Sie Endpunkte an mindestens zwei Standorten auf der ganzen Welt bereitstellen und Datenverkehr an den Standort weiterleiten, der Ihnen am nächsten liegt. Diese Möglichkeit bietet die leistungsorientierte Methode für das Datenverkehrsrouting.

![Leistungsorientierte Methode für das Datenverkehrsrouting in Azure Traffic Manager](media/traffic-manager-routing-methods/performance.png)

Der nächstgelegene Endpunkt ist nicht unbedingt derjenige, der geografisch am nächsten liegt. Vielmehr wird mit dieser Methode für das Datenverkehrsrouting der nächstgelegene Endpunkt durch die Messung der Netzwerklatenz bestimmt. Traffic Manager überwacht anhand einer Internetlatenztabelle die Roundtripzeit zwischen IP-Adressbereichen und den einzelnen Azure-Rechenzentren.

In dieser Internetlatenztabelle sucht der Dienst nach der IP-Quelladresse der eingehenden DNS-Abfrage. Anschließend wählt der Traffic Manager den im Azure-Datencenter verfügbaren Endpunkt mit der niedrigsten Wartezeit für diesen IP-Adressbereich aus und gibt diesen Endpunkt in der DNS-Antwort zurück.

Wie unter [Funktionsweise von Traffic Manager Works](traffic-manager-how-it-works.md) bereits beschrieben, erhält Traffic Manager DNS-Abfragen nicht direkt von Clients. Vielmehr erhält er DNS-Abfragen vom rekursiven DNS-Dienst, für deren Verwendung die Clients konfiguriert sind. Daher handelt es sich bei der IP-Adresse, die zur Ermittlung des nächstgelegenen Endpunkts verwendet wird, nicht um die IP-Adresse eines Clients, sondern um die IP-Adresse des zugehörigen rekursiven DNS-Diensts. In der Praxis lässt sich diese IP-Adresse für diesen Zweck gut verwenden.


Um Änderungen im globalen Internet und neue Azure-Regionen zu berücksichtigen, aktualisiert Traffic Manager die verwendete Internetlatenztabelle regelmäßig. Die Anwendungsleistung ist jedoch je nach Echtzeitschwankung der Auslastung im Internet unterschiedlich. Die leistungsorientierte Methode für das Datenverkehrsrouting überwacht nicht die Auslastung eines bestimmten Dienstendpunkts. Wenn ein Endpunkt jedoch nicht mehr verfügbar ist, wird er von Traffic Manager nicht in DNS-Abfrageantworten aufgenommen.


Beachten Sie Folgendes:

* Wenn Ihr Profil mehrere Endpunkte in der gleichen Azure-Region enthält, wird der an diese Region geleitete Datenverkehr gleichmäßig auf die verfügbaren Endpunkte verteilt. Wenn Sie eine andere Verteilung des Datenverkehrs innerhalb einer Region bevorzugen, können Sie dies mithilfe von [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md) erreichen.
* Wenn alle verfügbaren Endpunkte in der nächsten Azure-Region heruntergestuft sind, verschiebt der Traffic Manager den Datenverkehr zu den Endpunkten in der nächstgelegenen Azure-Region. Wenn Sie eine bevorzugte Failoversequenz definieren möchten, verwenden Sie [geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).
* Wenn die leistungsorientierte Methode für das Datenverkehrsrouting mit externen oder geschachtelten Endpunkten verwendet wird, müssen Sie den Standort dieser Endpunkte angeben. Wählen Sie die Azure-Region aus, die Ihrer Bereitstellung am nächsten ist. Bei diesen Standorten handelt es sich um die Werte, die von der Internetlatenztabelle unterstützt werden.
* Der Algorithmus, mit dem der Endpunkt ausgewählt wird, ist deterministisch. Wiederholte DNS-Abfragen des gleichen Clients werden an den gleichen Endpunkt geleitet. Clients verwenden unterwegs üblicherweise verschiedene rekursive DNS-Server. Der Client kann daher an einen anderen Endpunkt weitergeleitet werden. Routing kann sich ebenfalls auf die Internetlatenztabelle auswirken. Daher ist mit der leistungsorientierten Methode für das Datenverkehrsrouting nicht gewährleistet, dass ein Client immer an denselben Endpunkt weitergeleitet wird.
* Wenn die Internetlatenztabelle geändert wird, stellen Sie möglicherweise fest, dass einige Clients an einen anderen Endpunkt weitergeleitet werden. Dies reflektiert ein genaueres Routing basierend auf aktuellen Latenzdaten. Diese Updates sind wichtig, um angesichts der kontinuierlichen Entwicklung des Internets die Genauigkeit der leistungsorientierten Methode für das Datenverkehrsrouting sicherzustellen.

## <a name = "geographic"></a>Geografische Routingmethode für Datenverkehr

Traffic Manager-Profile können für die Verwendung der geografischen Routingmethode konfiguriert werden, damit Benutzer je nach dem geografischen Ursprungsort ihrer DNS-Abfrage auf bestimmte Endpunkte (Azure, Extern oder Geschachtelt) geleitet werden. Dadurch können Traffic Manager-Kunden Szenarien unterstützen, in denen die Kenntnis der geografischen Region eines Benutzers und das Routing auf der Grundlage dieser Kenntnis wichtig sind. Beispiele dafür bilden etwa die Einhaltung von Datenhoheitsmandaten, die Verortung von Inhalten und Benutzererfahrungen und das Messen von Datenverkehr aus verschiedenen Regionen.
Wenn ein Profil für geografisches Routing konfiguriert ist, muss jedem Endpunkt, der dem betreffenden Profil zugeordnet ist, eine Reihe geografischer Regionen zugeordnet sein. Die Granularität einer geografischen Region kann die folgenden Abstufungen aufweisen 
- Welt – jede Region
- Regionale Gruppierung - zum Beispiel Afrika, Naher Osten, Australien/Pazifik usw. 
- Land/Region - zum Beispiel Irland, Peru, Hongkong (SAR) usw. 
- Bundesland/Provinz: zum Beispiel USA-Kalifornien, Australien-Queensland, Kanada-Alberta usw. (Hinweis: Diese Granularitätsstufe wird nur für Bundesstaaten/Provinzen in Australien, Kanada und den USA unterstützt.)

Wenn einem Endpunkt eine Region oder eine Reihe von Regionen zugewiesen ist, werden alle Anforderungen aus diesen Regionen nur an den betreffenden Endpunkt geroutet. Traffic Manager verwendet die IP-Quelladresse der DNS-Abfrage, um die Region zu bestimmen, aus der eine Benutzerabfrage stammt - üblicherweise handelt es sich um die IP-Adresse des lokalen DNS-Auflösers, der die Abfrage im Auftrag des Benutzers ausführt.  

![Geografische Methode für das Datenverkehrsrouting in Azure Traffic Manager](./media/traffic-manager-routing-methods/geographic.png)

Traffic Manager liest die IP-Quelladresse der DNS-Abfrage und entscheidet, aus welcher geografischen Region sie stammt. Anschließend überprüft er, ob ein Endpunkt vorhanden ist, dem die betreffende geografische Region zugeordnet ist. Diese Suche beginnt auf der niedrigsten Granularitätsstufe (Bundesland/Provinz für die unterstützten Regionen, andernfalls auf der Ebene Land/Region) und schreitet bis zu höchsten Ebene **Welt** fort. Die erste Übereinstimmung, die bei diesem Durchlauf gefunden wird, wird als der in der Antwort auf die Abfrage zurückzugebende Endpunkt gekennzeichnet. Falls eine Übereinstimmung mit einem geschachtelten Endpunkttyp festgestellt wird, wird auf der Grundlage seiner Routingmethode ein Endpunkt mit dem entsprechenden untergeordneten Profil zurückgegeben. Die folgenden Punkte treffen auf dieses Verhalten zu:

- Wenn der Routingtyp geografisches Routing ist, kann ein Endpunkt in einem Traffic Manager-Profil nur einer geografischen Region zugeordnet werden. Dadurch ist sichergestellt, dass das Routing von Benutzern deterministisch erfolgt, sodass Kunden Szenarios aktivieren können, die unzweideutige geografische Grenzen erfordern.
- Wenn die Region eines Benutzers unter der geografischen Zuordnung zweier Endpunkte vorkommt, wählt Traffic Manager den Endpunkt mit der niedrigsten Granularität aus und verwirft Routinganforderungen aus der betreffenden Region für den anderen Endpunkt. Betrachten wir zum Beispiel ein Profil vom geografischen Routingtyp mit zwei Endpunkten: Endpunkt 1 und Endpunkt 2. Endpunkt 1 ist für den Empfang von Datenverkehr aus Irland und Endpunkt 2 für den Empfang von Datenverkehr aus Europa konfiguriert. Wenn eine Anforderung aus Irland stammt, wird sie immer an Endpunkt 1 geroutet.
- Da eine Region nur einem Endpunkt zugeordnet sein kann, gibt Traffic Manager diesen Endpunkt unabhängig von seinem Integritätsstatus zurück.

    >[!IMPORTANT]
    >Es wird Kunden dringend empfohlen, die geografische Routingmethode für Endpunkte vom Typ „Geschachtelt“ zu verwenden, die über untergeordnete Profile mit wenigstens zwei erreichbaren Endpunkten verfügen.
- Wenn ein übereinstimmender Endpunkt gefunden wird und dieser den Status **Stopped** (Beendet) aufweist, gibt Traffic Manager die Antwort „NODATA“ zurück. In diesem Fall erfolgen keine weiteren Suchläufe weiter oben in der Hierarchie der geografischen Regionen. Dieses Verhalten trifft auch auf geschachtelte Endpunkttypen zu, wenn sich das untergeordnete Profil im Status **Beendet** oder **Deaktiviert** befindet.
- Wenn sich ein Endpunkt im Status **Disabled** (Deaktiviert) befindet, wird er in den Prozess der Regionszuordnung nicht einbezogen. Dieses Verhalten trifft auch auf geschachtelte Endpunkttypen zu, wenn der Endpunkt sich im Status **Deaktiviert** befindet.
- Wenn eine Abfrage aus einer geografischen Region stammt, für die es im betreffenden Profil keine Zuordnung gibt, gibt Traffic Manager die Antwort „NODATA“ zurück. Daher wird Kunden dringend empfohlen, geografisches Routing mit einem Endpunkt zu verwenden, idealerweise vom Typ „Geschachtelt“ mit mindestens zwei Endpunkten im untergeordneten Profil, dem die Region **Welt** zugeordnet ist. Dadurch wird außerdem sichergestellt, dass alle IP-Adressen, die keiner Region zugeordnet sind, verarbeitet werden.

Wie unter [Funktionsweise von Traffic Manager Works](traffic-manager-how-it-works.md) bereits beschrieben, erhält Traffic Manager DNS-Abfragen nicht direkt von Clients. Vielmehr erhält er DNS-Abfragen vom rekursiven DNS-Dienst, für deren Verwendung die Clients konfiguriert sind. Daher handelt es sich bei der IP-Adresse, die zur Ermittlung der Region verwendet wird, nicht um die IP-Adresse eines Clients, sondern um die IP-Adresse des zugehörigen rekursiven DNS-Diensts. In der Praxis lässt sich diese IP-Adresse für diesen Zweck gut verwenden.

### <a name="faqs"></a>Häufig gestellte Fragen

* [Was sind einige Anwendungsfälle, in denen geografisches Routing nützlich ist?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-are-some-use-cases-where-geographic-routing-is-useful)

* [Wie entscheide ich, ob ich die leistungsbezogene oder geografische Routingmethode verwenden sollte?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-do-i-decide-if-i-should-use-performance-routing-method-or-geographic-routing-method)

* [Welche Regionen werden von Traffic Manager für das geografische Routing unterstützt?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing)

* [Wie bestimmt Traffic Manager, woher die Abfrage eines Benutzers stammt?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-does-traffic-manager-determine-where-a-user-is-querying-from)

* [Ist garantiert, dass Traffic Manager immer den genauen geografischen Standort des Benutzers ermitteln kann?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case)

* [Muss sich ein Endpunkt physisch in der Region befinden, für die er für geografisches Routen konfiguriert ist?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing)

* [Kann ich geografische Regionen Endpunkten in einem Profil zuordnen, das nicht für geografisches Routing konfiguriert ist?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing)

* [Warum tritt ein Fehler auf, wenn ich versuche, die Routingmethode eines vorhandenen Profils in geografisch zu ändern?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic)

* [Warum wird es Kunden dringend empfohlen, in einem Profil mit aktiviertem geografischem Routing geschachtelte Profile anstelle von Endpunkten zu erstellen?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled)

* [Gibt es Einschränkungen hinsichtlich der API-Version, die diesen Routingtyp unterstützt?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type)

## <a name = "multivalue"></a>Routingmethode „MultiValue“ für Datenverkehr
Mit der Routingmethode **MultiValue** für Datenverkehr können Sie mehrere fehlerfreie Endpunkte in einer einzelnen DNS-Abfrageantwort abrufen. Dadurch kann der Aufrufer clientseitige Wiederholungsversuche mit anderen Endpunkten ausführen, falls ein zurückgegebener Endpunkt nicht reagiert. Dieses Muster kann zur Erhöhung der Verfügbarkeit eines Diensts beitragen und bei einer neuen DNS-Abfrage zum Abrufen eines fehlerfreien Endpunkts die Wartezeit verkürzen. Die Routingmethode „MultiValue“ funktioniert nur, wenn alle Endpunkte vom Typ „Extern“ und als IPv4- oder IPv6-Adressen angegeben sind. Wenn eine Abfrage für dieses Profil empfangen wird, werden alle fehlerfreien Endpunkte unter Berücksichtigung einer konfigurierbaren maximalen Rückgabeanzahl zurückgegeben.

### <a name="faqs"></a>Häufig gestellte Fragen

* [Was sind einige Anwendungsfälle, in denen MultiValue-Routing nützlich ist?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-are-some-use-cases-where-multivalue-routing-is-useful)

* [Wie viele Endpunkte werden zurückgegeben, wenn das MultiValue-Routing verwendet wird?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-many-endpoints-are-returned-when-multivalue-routing-is-used)

* [Erhalte ich den gleichen Satz von Endpunkten, wenn das MultiValue-Routing verwendet wird?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#will-i-get-the-same-set-of-endpoints-when-multivalue-routing-is-used)

## <a name = "subnet"></a>Routingmethode „Subnetz“ für Datenverkehr
Mit der Routingmethode **Subnetz** für Datenverkehr können Sie eine Gruppe von Endbenutzer-IP-Adressbereichen bestimmten Endpunkten in einem Profil zuordnen. Wenn der Traffic Manager danach eine DNS-Abfrage für dieses Profil empfängt, untersucht er die Quell-IP-Adresse der Anforderung (in den meisten Fällen ist das die ausgehende IP-Adresse der vom Aufrufer verwendeten DNS-Auflösung), um zu bestimmen, welchem Endpunkt sie zugeordnet ist, und gibt den entsprechenden Endpunkt in der Abfrageantwort zurück. 

Die IP-Adresse, die einem Endpunkt zugeordnet wird, kann in Form von CIDR-Bereichen (etwa 1.2.3.0/24) oder als Adressbereich (etwa 1.2.3.4-5.6.7.8) angegeben werden. Die einem Endpunkt zugeordneten IP-Adressbereiche müssen innerhalb des Profils eindeutig sein und dürfen sich nicht mit der IP-Adressgruppe eines anderen Endpunkts im gleichen Profil überschneiden.
Wenn Sie einen Endpunkt ohne Adressbereich definieren, funktioniert dieser als Fallback und verarbeitet den Datenverkehr aller verbleibenden Subnetze. Wenn kein Fallbackendpunkt enthalten ist, sendet der Traffic Manager eine NODATA-Antwort für undefinierte Bereiche. Daher wird dringend empfohlen, entweder einen Fallbackendpunkt zu definieren oder alle möglichen IP-Adressbereiche für alle Ihre Endpunkte anzugeben.

Die Routingmethode „Subnetz“ kann verwendet werden, um Benutzern, die eine Verbindung über einen bestimmten IP-Adressraum herstellen, ein anderes Erlebnis zu bieten. So kann ein Kunde mithilfe der Routingmethode „Subnetz“ beispielsweise sämtliche Anforderungen, die von der Konzernzentrale ausgehen, an einen anderen Endpunkt weiterleiten, um etwa eine interne Version einer App zu testen. Ein weiteres Szenario wäre etwa die Bereitstellung einer anderen Erfahrung für Benutzer, die eine Verbindung über einen bestimmten ISP herstellen, um beispielsweise Benutzer eines bestimmten ISP zu blockieren.

### <a name="faqs"></a>Häufig gestellte Fragen

* [Welche Anwendungsfälle gibt es, in denen Subnetzrouting sinnvoll ist?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-are-some-use-cases-where-subnet-routing-is-useful)

* [Wie erkennt Traffic Manager die IP-Adresse des Endbenutzers?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-does-traffic-manager-know-the-ip-address-of-the-end-user)

* [Wie kann ich bei der Verwendung von Subnetzrouting IP-Adressen angeben?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-specify-ip-addresses-when-using-subnet-routing)

* [Wie kann ich bei der Verwendung von Subnetzrouting einen Fallbackendpunkt angeben?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-can-i-specify-a-fallback-endpoint-when-using-subnet-routing)

* [Was geschieht, wenn ein Endpunkt in einem Profil vom Typ Subnetzrouting deaktiviert ist?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-happens-if-an-endpoint-is-disabled-in-a-subnet-routing-type-profile)


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie mithilfe der [Traffic Manager endpoint monitoring](traffic-manager-monitoring.md)




