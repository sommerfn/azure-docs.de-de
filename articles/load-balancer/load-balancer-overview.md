---
title: Was versteht man unter Azure Load Balancer?
titlesuffix: Azure Load Balancer
description: Übersicht über Features, Architektur und Implementierung des Azure Load Balancers. Erfahren Sie, wie Load Balancer funktioniert, und nutzen Sie den Dienst in der Cloud.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/08/2019
ms.author: allensu
ms.openlocfilehash: be293a925e507468d96be4c9f6b47f30eea5f025
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73888678"
---
# <a name="what-is-azure-load-balancer"></a>Was versteht man unter Azure Load Balancer?

Mit Azure Load Balancer können Sie Ihre Anwendungen skalieren und Dienste mit Hochverfügbarkeit bereitstellen. Load Balancer unterstützt Szenarios mit eingehenden sowie ausgehenden Verbindungen, bietet niedrige Latenzen und einen hohen Durchsatz und skaliert Datenflüsse aller TCP- und UDP-Anwendungen im Millionenbereich hoch.

Load Balancer verteilt neue eingehende Datenflüsse, die am Front-End des Load Balancers eintreffen, gemäß den angegebenen Regeln und Integritätstests auf die Instanzen des Back-End-Pools.

Ein öffentlicher Load Balancer kann ausgehende Verbindungen für virtuelle Computer in Ihrem virtuellen Netzwerk bereitstellen, indem er deren private IP-Adressen in öffentliche IP-Adressen übersetzt.

Für Azure Load Balancer sind zwei Tarife bzw. *SKUs* erhältlich: Basic und Standard. Diese unterscheiden sich hinsichtlich Staffelung, Funktionen und Preisen. Jedes Szenario, das mit dem Load Balancer im Tarif „Basic“ möglich ist, kann auch mit Load Balancer Standard erstellt werden – auch wenn sich die Vorgehensweise leicht unterscheidet. Bei der Beschäftigung mit den Load Balancer-Aspekten sollten Sie sich mit den Grundlagen und den SKU-spezifischen Unterschieden vertraut machen.

## <a name="why-use-load-balancer"></a>Gründe für die Verwendung von Load Balancer

Sie können Azure Load Balancer für Folgendes verwenden:

* Führen Sie einen Lastenausgleich für den eingehenden Internetdatenverkehr für virtuelle Computer durch. Diese Konfiguration wird als ein [öffentlicher Load Balancer](#publicloadbalancer) bezeichnet.
* Führen Sie einen Lastenausgleich des Datenverkehrs für mehrere VMs in einem virtuellen Netzwerk durch. Sie können ein Load Balancer-Front-End in einem Hybridszenario auch aus einem lokalen Netzwerk erreichen. Für beide Szenarien wird eine Konfiguration verwendet, die als ein [interner Load Balancer](#internalloadbalancer) bezeichnet wird.
* Der Port leitet den Datenverkehr an einen bestimmten Port auf bestimmten virtuellen Computern mit Regeln für die Netzwerkadressenübersetzung für eingehenden Datenverkehr weiter.
* Stellen Sie [ausgehende Verbindungen](load-balancer-outbound-connections.md) für virtuelle Computer in Ihrem virtuellen Netzwerk durch Verwenden eines öffentlichen Load Balancers bereit.

>[!NOTE]
> Azure stellt eine Sammlung aus vollständig verwalteten Lastenausgleichslösungen für Ihre Szenarios bereit. Wenn Sie nach Informationen zur Beendigung der Transport Layer Security-Protokollierung oder zur Verarbeitung der Anwendungsschicht pro HTTP/HTTPS-Anforderung suchen, hilft Ihnen der Artikel [Was ist Azure Application Gateway?](../application-gateway/application-gateway-introduction.md) weiter. Wenn Sie nach Informationen zum globalen DNS-Lastenausgleich suchen, hilft Ihnen der Artikel [Was ist Traffic Manager?](../traffic-manager/traffic-manager-overview.md) weiter. Für Ihre End-to-End-Szenarien kann es vorteilhaft sein, diese Lösungen zu kombinieren.

## <a name="what-are-load-balancer-resources"></a>Was sind Load Balancer-Ressourcen?

Load Balancer-Ressourcen sind Objekte, mit denen angegeben werden kann, wie Azure die mehrinstanzenfähige Infrastruktur programmieren muss, um das zu erstellende Szenario zu erzielen. Es gibt keine direkte Beziehung zwischen Load Balancer-Ressourcen und der tatsächlichen Infrastruktur. Beim Erstellen eines Load Balancers wird keine Instanz erstellt, und die Kapazität ist stets verfügbar.

Bei einer Load Balancer-Ressource kann es sich entweder um einen öffentlichen oder einen internen Load Balancer handeln. Die Funktionen einer Load Balancer-Ressource werden festgelegt, indem die Elemente Front-End, Regel, Integritätstest und Back-End-Pool definiert werden. Sie ordnen VMs im Back-End-Pool an, indem Sie diesen über die VM angeben.

## <a name="fundamental-load-balancer-features"></a>Grundlegende Load Balancer-Features

Load Balancer stellt die folgenden grundlegenden Funktionen für TCP und UDP-Anwendungen bereit:

* **Lastenausgleich**

  Mit Azure Load Balancer können Sie eine Lastenausgleichsregel erstellen, mit der Datenverkehr, der am Front-End eintrifft, auf Back-End-Poolinstanzen verteilt wird. Load Balancer verwendet einen Hashalgorithmus für die Verteilung von eingehenden Flows (Datenflüssen) und schreibt die Header der Flows für Back-End-Poolinstanzen um. Wenn der Integritätstest einen fehlerfreien Back-End-Endpunkt angibt, ist ein Server verfügbar, der neue Flows empfangen kann.

  Standardmäßig wird für Load Balancer ein 5-Tupel-Hash verwendet. Der Hash umfasst die Elemente IP-Quelladresse, Quellport, IP-Zieladresse, Zielport und IP-Protokollnummer, um Flows den verfügbaren Servern zuzuordnen. Sie können eine Affinität zu einer IP-Quelladresse erstellen, indem Sie für eine bestimmte Regel einen 2- oder 3-Tupel-Hash verwenden. Alle Pakete desselben Paketflows treffen bei derselben Instanz hinter dem Front-End mit Lastenausgleich ein. Wenn der Client einen neuen Flow von derselben IP-Quelladresse initiiert, wird der Quellport geändert. Dies kann dazu führen, dass der Datenverkehr aufgrund des 5-Tupel-Hashs an einen anderen Back-End-Endpunkt gesendet wird.

  Weitere Informationen finden Sie unter [Konfigurieren des Verteilungsmodus für Azure Load Balancer](load-balancer-distribution-mode.md). In der folgenden Abbildung wird die hashbasierte Verteilung angezeigt:

  ![Hash-basierte Verteilung](./media/load-balancer-overview/load-balancer-distribution.png)

  *Abbildung: Hashbasierte Verteilung*

* **Portweiterleitung**

  Mit Load Balancer können Sie eine NAT-Regel für eingehenden Datenverkehr erstellen. Mit dieser NAT-Regel wird Datenverkehr von einem bestimmten Port einer Front-End-IP-Adresse an einen bestimmten Port einer Back-End-Instanz im virtuellen Netzwerk weitergeleitet. Diese Weiterleitung basiert auf der gleichen hashbasierten Verteilung wie beim Lastenausgleich. Allgemeine Szenarien für diese Funktion sind RDP- (Remotedesktopprotokoll) oder SSH-Sitzungen (Secure Shell) für einzelne VM-Instanzen in einem Azure Virtual Network.
  
  Sie können Ports mehrere interne Endpunkte unter derselben Front-End-IP-Adresse zuordnen. Sie können die Front-End-IP-Adressen verwenden, um für Ihre VMs die Remoteverwaltung ohne zusätzliche Jumpbox durchzuführen.

* **Anwendungsunabhängigkeit und -transparenz**

  Load Balancer interagiert nicht direkt mit TCP oder UDP oder der Anwendungsschicht. Alle TCP- oder UDP-Anwendungsszenarien können unterstützt werden. Von Load Balancer werden keine Flows beendet oder angestoßen, es erfolgt keine Interaktion mit der Nutzlast des Flows, und es wird auch keine Gatewayfunktion auf der Anwendungsschicht bereitgestellt. Protokollhandshakes werden immer direkt zwischen dem Client und der Back-End-Poolinstanz durchgeführt. Bei einer Antwort auf einen eingehenden Flow handelt es sich immer um die Antwort eines virtuellen Computers. Wenn der Flow auf dem virtuellen Computer eingeht, wird auch die IP-Adresse der ursprünglichen Quelle gespeichert.

  * Jeder Endpunkt erhält nur über eine VM eine Antwort. Zum Beispiel wird ein TCP-Handshake immer zwischen dem Client und der ausgewählten Back-End-VM ausgeführt. Eine Antwort auf eine Anforderung, die an ein Front-End gesendet wird, wird von einer Back-End-VM generiert. Wenn Sie eine erfolgreiche Überprüfung der Konnektivität für ein Front-End durchführen, bedeutet dies, dass Sie eine End-to-End-Konnektivität für mindestens eine Back-End-VM überprüfen.
  * Anwendungsnutzlasten sind für Load Balancer transparent. Alle UDP- oder TCP-Anwendungen können unterstützt werden. Für Workloads, die eine Verarbeitung pro HTTP-Anforderung oder eine Manipulation der Nutzlasten auf Anwendungsschicht (z. B. Analysieren von HTTP-URLs) erfordern, sollten Sie einen Layer 7-Lastenausgleich (z. B. [Application Gateway](https://azure.microsoft.com/services/application-gateway)) verwenden.
  * Da der Load Balancer nicht mit der TCP-Nutzlast interagiert und keine TLS-Abladung bereitstellt, können Sie verschlüsselte End-to-End-Szenarien erstellen. Die Verwendung des Load Balancers ermöglicht ein hohes Maß an horizontaler Skalierung für TLS-Anwendungen, indem die TLS-Verbindung auf dem virtuellen Computer selbst beendet wird. Beispielsweise ist die TLS-Funktion zum erstellen von Sitzungsschlüsseln vom Typ und der Nummer der VMs beschränkt, die Sie zum Back-End-Pool hinzufügen. Wenn bei Ihnen eine „SSL-Abladung“ oder eine Behandlung der Anwendungsschicht erforderlich ist oder Sie die Zertifikatverwaltung an Azure delegieren möchten, sollten Sie stattdessen den Layer 7-Lastenausgleich von Azure ([Application Gateway](https://azure.microsoft.com/services/application-gateway)) verwenden.

* **Automatische Neukonfiguration**

  Der Load Balancer konfiguriert sich selbst sofort, wenn Sie Instanzen nach oben oder nach unten skalieren. Das Hinzufügen oder Entfernen von virtuellen Computern für den Back-End-Pool bewirkt, dass der Load Balancer neu konfiguriert wird. Es werden aber keine zusätzlichen Vorgänge für die Load Balancer-Ressource ausgeführt.

* **Integritätstests**

  Der Load Balancer verwendet von Ihnen definierte Integritätstests, um die Integrität von Instanzen im Back-End-Pool zu ermitteln. Wenn ein Test nicht reagiert, beendet Load Balancer das Senden neuer Verbindungen an die fehlerhaften Instanzen. Ein Testfehler wirkt sich nicht auf vorhandene Verbindungen aus. Die Verbindung ist weiterhin verfügbar, bis der Flow von der Anwendung beendet wird, eine Leerlaufzeitüberschreitung auftritt oder die VM heruntergefahren wird.

  Load Balancer verfügt über verschiedene Integritätstesttypen für TCP-, HTTP- und HTTPS-Endpunkte. Weitere Informationen finden Sie unter [Testtypen](load-balancer-custom-probe-overview.md#types).

  Bei Verwendung von klassischen Clouddiensten ist ein weiterer Typ zulässig: [Gast-Agent](load-balancer-custom-probe-overview.md#guestagent). Ein Gast-Agent sollte als letztmögliche Option für einen Integritätstest angesehen werden. Microsoft rät von der Nutzung ab, falls andere Optionen zur Verfügung stehen.

* **Ausgehende (SNAT-)Verbindungen**

  Alle ausgehenden Flows von privaten IP-Adressen in Ihrem virtuellen Netzwerk zu öffentlichen IP-Adressen im Internet können in eine Front-End-IP-Adresse des Load Balancers übersetzt werden. Wenn ein öffentliches Front-End per Lastenausgleichsregel an einen virtuellen Back-End-Computer gebunden ist, übersetzt Azure ausgehende Verbindungen in die IP-Adresse des öffentlichen Front-Ends. Diese Konfiguration hat die folgenden Vorteile:

  * Einfache Upgrades und eine Notfallwiederherstellung von Diensten, da das Front-End dynamisch einer anderen Instanz des Diensts zugeordnet werden kann.
  * Vereinfachte Verwaltung von Zugriffssteuerungslisten. Zugriffssteuerungslisten, die als Front-End-IP-Adressen ausgedrückt werden, ändern sich nicht, wenn Dienste zentral hoch- oder herunterskaliert oder erneut bereitgestellt werden. Die Übersetzung von ausgehenden Verbindungen in eine Anzahl von IP-Adressen, die geringer als die Anzahl von Computern ist, verringert den Aufwand für die Implementierung sicherer Empfängerlisten.

  Weitere Informationen finden Sie unter [Ausgehende Verbindungen in Azure](load-balancer-outbound-connections.md).

Load Balancer Standard verfügt über zusätzliche SKU-spezifische Funktionen, die über diese Grundlagen hinausgehen. Dies ist unten beschrieben.

## <a name="skus"></a> Vergleich der Load Balancer-SKUs

Load Balancer unterstützt sowohl Basic- als auch Standard-SKUs. Diese SKUs unterscheiden sich in Bezug auf Skalierung, Features und Preise. Jedes Szenario, das mit dem Load Balancer im Tarif „Basic“ möglich ist, kann auch mit Load Balancer Standard erstellt werden. Die APIs für beide SKUs sind ähnlich, und sie werden über die Spezifikation einer SKU aufgerufen. Die API zur Unterstützung von SKUs für Load Balancer und öffentliche IP-Adressen ist ab der API-Version `2017-08-01` verfügbar. Beide SKUs verfügen über die gleiche allgemeine API und Struktur.

Die Konfiguration des gesamten Szenarios kann sich je nach SKU leicht unterscheiden. In der Load Balancer-Dokumentation wird darauf hingewiesen, wenn ein Artikel nur für eine bestimmte SKU gilt. Weitere Informationen zu den Unterschieden finden Sie in der folgenden Tabelle. Weitere Informationen finden Sie unter [Übersicht: Azure Load Balancer Standard](load-balancer-standard-overview.md).

>[!NOTE]
> Für neue Entwürfe sollte Load Balancer Standard verwendet werden.

Eigenständige virtuelle Computer, Verfügbarkeitsgruppen und VM-Skalierungsgruppen können nur mit einer SKU, nie mit beiden verbunden werden. Die Load Balancer-SKU muss mit der SKU für öffentliche IP-Adressen übereinstimmen, wenn Sie sie mit öffentlichen IP-Adressen verwenden. Load Balancer-SKUs und SKUs für öffentliche IP-Adressen sind nicht änderbar.

Die bewährte Methode besteht darin, die SKUs explizit anzugeben. Zu diesem Zeitpunkt werden notwendige Änderungen auf ein Minimum beschränkt. Wenn keine SKU angegeben ist, wird standardmäßig die API-Version `2017-08-01` der SKU „Basic“ verwendet.

>[!IMPORTANT]
>Load Balancer Standard ist ein neues Load Balancer-Produkt. Es handelt sich im Prinzip um eine Erweiterung von Load Balancer im Tarif „Basic“, aber es gibt einige wichtige Unterschiede zwischen den beiden Produkten. Jedes End-to-End-Szenario, das mit dem Basic-Load Balancer möglich ist, kann auch mit dem Standard-Load Balancer erstellt werden. Falls Sie bereits mit dem Load Balancer im Tarif „Basic“ vertraut sind, sollten Sie einen Vergleich mit Load Balancer Standard durchführen, um sich über die aktuellen Änderungen in Bezug auf das Verhalten zu informieren.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Weitere Informationen finden Sie unter [Load Balancer-Grenzwerte](https://aka.ms/lblimits). Lesen Sie für den Standard-Load Balancer auch die ausführlicheren Informationen unter [Übersicht](load-balancer-standard-overview.md), [Preise](https://aka.ms/lbpricing) und [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Konzepte

### <a name = "publicloadbalancer"></a>Öffentlicher Load Balancer

Bei einem öffentlichen Load Balancer werden die öffentliche IP-Adresse und der Port des eingehenden Datenverkehrs der privaten IP-Adresse und dem Port der VM zugeordnet. Für den Antwortdatenverkehr von der VM führt der Load Balancer eine Zuordnung in umgekehrter Richtung durch. Sie können bestimmte Typen von Datenverkehr auf verschiedene VMs oder Dienste verteilen, indem Sie Lastenausgleichsregeln anwenden. Sie können zum Beispiel die Netzwerklast von Webanforderungen auf mehrere Webserver verteilen.

>[!NOTE]
>Sie können pro Verfügbarkeitsgruppe nur einen öffentlichen Load Balancer und einen internen Load Balancer implementieren.

Die folgende Abbildung zeigt einen Endpunkt für Webdatenverkehr mit Lastenausgleich, der von drei virtuellen Computern für den öffentlichen TCP-Port 80 genutzt wird. Diese drei virtuellen Computer bilden eine Gruppe mit Lastenausgleich.

![Beispiel für einen öffentlichen Load Balancer](./media/load-balancer-overview/IC727496.png)

*Abbildung: Durchführen des Lastenausgleichs für Webdatenverkehr mit einem öffentlichen Load Balancer*

Internetclients senden Webseitenanforderungen an die öffentliche IP-Adresse einer Web-App über TCP-Port 80. Azure Load Balancer verteilt die Anforderungen auf die drei VMs in der Gruppe mit Lastenausgleich. Weitere Informationen zu Load Balancer-Algorithmen finden Sie unter [Grundlegende Load Balancer-Features](load-balancer-overview.md##fundamental-load-balancer-features).

Standardmäßig verteilt Azure Load Balancer Netzwerkdatenverkehr gleichmäßig auf mehrere Instanzen virtueller Computer. Sie können auch Sitzungsaffinität konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren des Verteilungsmodus für Azure Load Balancer](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> Interner Load Balancer

Ein interner Load Balancer leitet Datenverkehr – im Gegensatz zu einem öffentlichen Load Balancer – nur an Ressourcen weiter, die sich innerhalb eines virtuellen Netzwerks befinden oder die für den Zugriff auf die Azure-Infrastruktur ein VPN verwenden. Die Azure-Infrastruktur schränkt den Zugriff auf die Front-End-IP-Adressen eines virtuellen Netzwerks ein, für die ein Lastenausgleich durchgeführt wird. Front-End-IP-Adressen und virtuelle Netzwerke werden nie direkt für einen Internetendpunkt verfügbar gemacht. Interne Branchenanwendungen werden in Azure ausgeführt. Auf sie wird aus Azure oder von lokalen Ressourcen aus zugegriffen.

Ein interner Load Balancer ermöglicht die folgenden Arten von Lastenausgleich:

* **In einem virtuellen Netzwerk:** Lastenausgleich zwischen virtuellen Computern im virtuellen Netzwerk und einer Gruppe von virtuellen Computern, die sich in demselben virtuellen Netzwerk befinden.
* **Für ein standortübergreifendes virtuelles Netzwerk:** Lastenausgleich zwischen lokalen Computern und einer Gruppe von virtuellen Computern, die sich in demselben virtuellen Netzwerk befinden.
* **Für Anwendungen mit mehreren Ebenen:** Lastenausgleich für Anwendungen mit mehreren Ebenen und Internetzugriff, wobei für die Back-End-Ebenen kein Internetzugriff besteht. Die Back-End-Ebenen erfordern einen Lastenausgleich des Datenverkehrs aus einer mit dem Internet verbundenen Ebene. Dies ist in der nächsten Abbildung dargestellt.
* **Für Branchenanwendungen:** Lastenausgleich für Branchenanwendungen, die in Azure ohne zusätzliche Hardware oder Software für den Lastenausgleich gehostet werden. Dieses Szenario enthält lokale Server, die sich in der Gruppe der Computer befinden, für deren Datenverkehr ein Lastenausgleich durchgeführt wird.

![Beispiel für einen internen Load Balancer](./media/load-balancer-overview/IC744147.png)

*Abbildung: Durchführen eines Lastenausgleichs für Anwendungen mit mehreren Ebenen mithilfe eines öffentlichen und eines internen Load Balancers*

## <a name="pricing"></a>Preise

Die Verwendung von Load Balancer Standard ist kostenpflichtig.

* Anzahl von konfigurierten Lastenausgleichsregeln und Regeln für ausgehenden Datenverkehr. NAT-Regeln für eingehenden Datenverkehr werden nicht in die Gesamtzahl von Regeln eingerechnet.
* Menge der verarbeiteten eingehenden und ausgehenden Daten, unabhängig von den Regeln.

Informationen zu den Preisen für Load Balancer Standard finden Sie unter [Load Balancer – Preise](https://azure.microsoft.com/pricing/details/load-balancer/).

Basic Load Balancer wird kostenlos angeboten.

## <a name="sla"></a>SLA

Informationen zur Vereinbarung zum Servicelevel (SLA) für Load Balancer Standard finden Sie unter [SLA für Load Balancer](https://aka.ms/lbsla).

## <a name="limitations"></a>Einschränkungen

* Der Load Balancer ermöglicht den Lastenausgleich und die Portweiterleitung für bestimmte TCP- oder UDP-Protokolle. Für Lastenausgleichsregeln und NAT-Regeln für eingehenden Datenverkehr werden TCP und UDP unterstützt, aber keine anderen IP-Protokolle, z. B. ICMP.

  Der Load Balancer beendet die Nutzlast des UDP- oder TCP-Flows nicht, antwortet nicht darauf und interagiert auch nicht auf andere Weise damit. Es handelt sich nicht um einen Proxy. Die erfolgreiche Überprüfung der Konnektivität mit einem Front-End muss per In-band-Zugriff mit dem gleichen Protokoll erfolgen, das auch in der Lastenausgleichsregel bzw. der NAT-Regel für eingehenden Datenverkehr verwendet wird. Mindestens einer Ihrer virtuellen Computer muss eine Antwort für einen Client generieren, um eine Antwort vom Front-End zu erhalten.

  Wenn Sie keine In-band-Antwort vom Load Balancer-Front-End erhalten, deutet dies darauf hin, dass keine virtuellen Computer antworten konnten. Die Interaktion mit einem Load Balancer-Front-End ist nicht möglich, wenn ein virtueller Computer nicht antworten kann. Dies gilt auch für ausgehende Verbindungen, bei denen die Portmaskierung mit SNAT nur für TCP und UDP unterstützt wird. Für alle anderen IP-Protokolle, z. B. ICMP, tritt ein Fehler auf. Weisen Sie eine öffentliche IP-Adresse auf Instanzebene zu, um dieses Problem zu umgehen. Weitere Informationen finden Sie unter [Grundlagen von SNAT und PAT](load-balancer-outbound-connections.md#snat).

* Interne Load Balancer führen für ausgehende Verbindungen keine Übersetzung für das Front-End eines internen Load Balancers durch, da sich beide im privaten IP-Adressraum befinden. Über öffentliche Load Balancer werden [ausgehende Verbindungen](load-balancer-outbound-connections.md) von privaten IP-Adressen im virtuellen Netzwerk mit öffentlichen IP-Adressen bereitgestellt. Bei internen Load Balancern wird mit diesem Ansatz die potenzielle SNAT-Portüberlastung in einem eindeutigen internen IP-Adressraum vermieden, für den die Übersetzung nicht erforderlich ist.

  Hierbei ergibt sich die folgende Nebenwirkung: Wenn ein von einem virtuellen Computer ausgehender Flow in den Back-End-Pool versucht, einen Flow zum Front-End der internen Load Balancer-Instanz im Pool auszuführen _und_ sich selbst zugewiesen ist, stimmen die beiden Verzweigungen des Flows nicht überein. Aufgrund dieser fehlenden Übereinstimmung tritt für den Flow ein Fehler auf. Der Flow ist erfolgreich, wenn er nicht dem virtuellen Computer im Back-End-Pool zugeordnet ist, die den Flow zum Front-End erstellt hat.

  Wenn der Flow sich selbst zugeordnet ist, geht der ausgehende Flow zum Front-End scheinbar vom virtuellen Computer und der zugehörige eingehende Flow scheinbar vom virtuellen Computer an sich selbst aus. Aus Sicht des Gastbetriebssystems stimmen die eingehenden und ausgehenden Bestandteile desselben Flows innerhalb des virtuellen Computers nicht überein. Der TCP-Stack erkennt nicht, dass es sich bei diesen Hälften um Teile desselben Flows handelt. Die Quelle und das Ziel stimmen nicht überein. Wenn der Flow keinem anderen virtuellen Computer im Back-End-Pool zugeordnet ist, stimmen die Teile des Flows überein, und der virtuelle Computer kann auf den Flow antworten.

  Das Symptom für dieses Szenario sind zeitweilige Verbindungstimeouts, die auftreten, wenn der Flow zu demselben Back-End zurückkehrt, vom dem er ursprünglich stammt. Zu den häufig genutzten Problemumgehungen gehören das Einfügen einer Proxyebene hinter dem internen Load Balancer und das Verwenden von DSR-Stilregeln (Direct Server Return). Weitere Informationen finden Sie unter [Mehrere Front-Ends für Azure Load Balancer](load-balancer-multivip-overview.md).

  Sie können eine interne Load Balancer-Instanz mit dem Proxy eines Drittanbieters kombinieren oder die interne [Application Gateway](../application-gateway/application-gateway-introduction.md)-Instanz für Proxyszenarien mit HTTP/HTTPS verwenden. Es ist zwar möglich, zum Beheben dieses Problems einen öffentlichen Load Balancer zu nutzen, aber das sich ergebende Szenario ist anfällig für [SNAT-Auslastung](load-balancer-outbound-connections.md#snat). Nutzen Sie diesen zweiten Ansatz nur, wenn eine sorgfältige Verwaltung durchgeführt werden kann.

* Im Allgemeinen wird das Weiterleiten von IP-Fragmenten für Lastenausgleichsregeln nicht unterstützt. Die IP-Fragmentierung von UDP- und TCP-Paketen wird für Lastenausgleichsregeln nicht unterstützt. Hochverfügbarkeitsports für Lastenausgleichsregeln können verwendet werden, um vorhandene IP-Fragmente weiterzuleiten. Weitere Informationen finden Sie unter [Übersicht über Hochverfügbarkeitsports](load-balancer-ha-ports-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit einem Load Balancer finden Sie unter [Erstellen eines Load Balancers im Tarif „Basic“](quickstart-create-basic-load-balancer-portal.md). Es wird beschrieben, wie Sie einen Load Balancer und VMs mit einer installierten benutzerdefinierten IIS-Erweiterung erstellen und den Lastenausgleich für die Web-App zwischen den VMs einrichten.
