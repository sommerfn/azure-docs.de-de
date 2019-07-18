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
ms.date: 01/11/2019
ms.author: allensu
ms.openlocfilehash: fb7c0c31ad91bfdb6ea360c1909a216f0779ebde
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274614"
---
# <a name="what-is-azure-load-balancer"></a>Was versteht man unter Azure Load Balancer?

Mit Azure Load Balancer können Sie Ihre Anwendungen skalieren und Dienste mit Hochverfügbarkeit bereitstellen. Load Balancer unterstützt Szenarios mit eingehenden sowie ausgehenden Verbindungen, bietet niedrige Latenzen und einen hohen Durchsatz und skaliert Datenflüsse aller TCP- und UDP-Anwendungen im Millionenbereich hoch.  

Load Balancer verteilt neue eingehende Flows, die im Load Balancer-Front-End eintreffen, gemäß den Regeln und Integritätstests auf die Instanzen des Back-End-Pools. 

Zusätzlich kann ein öffentlicher Load Balancer auch ausgehende Verbindungen für virtuelle Computer in Ihrem virtuellen Netzwerk bereitstellen, indem er deren private IP-Adressen in öffentliche IP-Adressen übersetzt.

Azure Load Balancer ist in zwei SKUs verfügbar: Basic und Standard. Diese unterscheiden sich hinsichtlich Staffelung, Funktionen und Preisen. Jedes Szenario, das mit dem Basic Load Balancer möglich ist, kann auch mit dem Standard Load Balancer erstellt werden, auch wenn sich die Vorgehensweise leicht unterscheiden kann. Wenn Sie sich intensiver mit Load Balancer beschäftigen, sollten Sie sich mit den Grundlagen und den SKU-spezifischen Unterschieden vertraut machen.

## <a name="why-use-load-balancer"></a>Gründe für die Verwendung von Load Balancer 

Sie können Azure Load Balancer für Folgendes verwenden:

* Führen Sie einen Lastenausgleich für den eingehenden Internetdatenverkehr für virtuelle Computer aus. Diese Konfiguration wird als [öffentlicher Load Balancer](#publicloadbalancer) bezeichnet.
* Ausführen eines Lastenausgleichs des Datenverkehrs für mehrere VMs in einem virtuellen Netzwerk. Sie können ein Load Balancer-Front-End in einem Hybridszenario auch aus einem lokalen Netzwerk erreichen. In beiden Szenarien wird eine Konfiguration verwendet, die als [interner Load Balancer](#internalloadbalancer) bezeichnet wird.
* Der Port leitet den Datenverkehr an einen bestimmten Port auf bestimmten virtuellen Computern mit Regeln für die Netzwerkadressenübersetzung für eingehenden Datenverkehr weiter.
* Stellen Sie [ausgehende Verbindungen](load-balancer-outbound-connections.md) für virtuelle Computer in Ihrem virtuellen Netzwerk durch Verwenden eines öffentlichen Load Balancers bereit.


>[!NOTE]
> Azure stellt eine Sammlung aus vollständig verwalteten Lastenausgleichslösungen für Ihre Szenarios bereit. Wenn Sie nach Informationen zur Beendigung der Transport Layer Security-Protokollierung oder zur Verarbeitung der Anwendungsschicht pro HTTP/HTTPS-Anforderung suchen, lesen Sie den Artikel [Application Gateway](../application-gateway/application-gateway-introduction.md). Wenn Sie nach Informationen zu globalem DNS-Lastenausgleich suchen, lesen Sie [Traffic Manager](../traffic-manager/traffic-manager-overview.md). Für Ihre End-to-End-Szenarios kann es von Vorteil sein, diese Lösungen nach Bedarf zu kombinieren.

## <a name="what-are-load-balancer-resources"></a>Was sind Load Balancer-Ressourcen?

Eine Load Balancer-Ressource (Lastenausgleichsressource) kann entweder als ein öffentlicher Load Balancer oder als ein interner Load Balancer vorhanden sein. Die Funktionen einer Load Balancer-Ressource werden in Form von Front-End-, Regel-, Integritätstest- und Back-End-Pool-Definitionen ausgedrückt. Platzieren Sie virtuelle Computer im Back-End-Pool, indem Sie diesen über den virtuellen Computer angeben.

Load Balancer-Ressourcen sind Objekte, in denen Sie formulieren können, wie Azure die mehrinstanzenfähige Infrastruktur programmieren muss, um das Szenario zu erzielen, das Sie erstellen möchten. Es gibt keine direkte Beziehung zwischen Load Balancer-Ressourcen und der tatsächlichen Infrastruktur. Beim Erstellen eines Load Balancers wird keine Instanz erstellt, und die Kapazität ist stets verfügbar. 

## <a name="fundamental-load-balancer-features"></a>Grundlegende Load Balancer-Features

Load Balancer stellt die folgenden grundlegenden Funktionen für TCP und UDP-Anwendungen bereit:

* **Lastenausgleich**

    Mit Azure Load Balancer können Sie eine Regel für den Lastenausgleich erstellen, nach der Datenverkehr, der bei einem Front-End eintrifft, auf Back-End-Poolinstanzen verteilt wird. Load Balancer verwendet einen hashbasierten Algorithmus für die Verteilung von eingehenden Flows und schreibt die Header der Flows so um, dass sie den Back-End-Poolinstanzen entsprechen. Wenn der Integritätstest einen fehlerfreien Back-End-Endpunkt angibt, ist ein Server verfügbar, der neue Flows empfangen kann.
    
    Standardmäßig verwendet Load Balancer ein Fünf-Tupel-Hash (IP-Quelladresse, Quellport, IP-Zieladresse, Zielport und IP-Protokollnummer), um den verfügbaren Servern Flows zuzuordnen. Sie können eine Affinität zu einer bestimmten IP-Quelladresse erstellen, indem Sie für eine bestimmte Regel zu einem Zwei- oder Drei-Tupel-Hash wechseln. Alle Pakete desselben Paketflows treffen bei derselben Instanz hinter dem Front-End mit Lastenausgleich ein. Wenn der Client einen neuen Datenfluss von derselben IP-Quelladresse initiiert, wird der Quellport gewechselt. Dies kann dazu führen, dass das Fünf-Tupel dafür sorgt, dass der Datenverkehr an einen anderen Back-End-Endpunkt gesendet wird.

    Weitere Informationen hierzu finden Sie unter [Load Balancer-Verteilungsmodus](load-balancer-distribution-mode.md). In der folgenden Abbildung wird die hashbasierte Verteilung angezeigt:

    ![Hash-basierte Verteilung](./media/load-balancer-overview/load-balancer-distribution.png)

    *Abbildung: Hashbasierte Verteilung*

* **Portweiterleitung**

    Mit Load Balancer können Sie eine Regel für die Netzwerkadressenübersetzung für eingehenden Datenverkehr erstellen, um den Datenverkehr von einem bestimmten Port einer bestimmten Front-End-IP-Adresse auf einen bestimmten Port einer bestimmten Back-End-Instanz im virtuellen Netzwerk zu übertragen. Dies wird auch durch dieselbe hashbasierte Verteilung wie beim Lastausgleich erreicht. Allgemeine Szenarios für diese Fähigkeit sind Remotedesktopprotokoll- oder Secure Shell-Sitzungen für einzelne VM-Instanzen im Azure Virtual Network. Sie können unterschiedlichen Ports mehrere interne Endpunkte unter derselben Front-End-IP-Adresse zuordnen. Sie können die Front-End-IP-Adressen dazu verwenden, Ihre virtuellen Computer remote über das Internet zu verwalten, ohne dass Sie eine zusätzliche Jumpbox benötigen.

* **Anwendungsunabhängig und transparent**

    Der Load Balancer interagiert nicht direkt mit TCP, UDP oder der Anwendungsschicht, und jedes TCP- oder UDP-basierte Anwendungsszenario kann unterstützt werden.  Der Load Balancer beendet oder startet keine Flows, interagiert nicht mit der Payload des Flows, stellt keine Gateway-Funktion auf Anwendungsschicht bereit und Protokollhandshakes werden immer direkt zwischen dem Client und der Back-End-Poolinstanz ausgeführt.  Bei einer Antwort auf einen eingehenden Flow handelt es sich immer um die Antwort eines virtuellen Computers.  Wenn der Flow auf dem virtuellen Computer eingeht, wird auch die IP-Adresse der ursprünglichen Quelle gespeichert.  Nachfolgend sind einige Beispiele aufgeführt, um die Transparenz besser zu verdeutlichen:
    - Jeder Endpunkt erhält nur über eine VM eine Antwort.  Zum Beispiel wird ein TCP-Handshake immer zwischen dem Client und dem ausgewählten virtuellen Back-End-Computer ausgeführt.  Antworten auf Anforderungen auf ein Front-End werden von einem virtuellen Back-End-Computer generiert. Wenn Sie erfolgreich die Konnektivität auf ein Front-End überprüfen, bedeutet das, dass Sie eine End-to-End-Konnektivität für mindestens einen virtuellen Back-End-Computer überprüfen.
    - Anwendungspayloads sind für Load Balancer sichtbar, und jede beliebige UDP- oder TCP-Anwendung kann unterstützt werden. Für Workloads, die eine Verarbeitung pro HTTP-Anforderung oder eine Manipulation der Payloads auf Anwendungsschicht (z.B. Analysieren von HTTP-URLs) erfordern, sollten Sie einen Lastenausgleich der siebten Schicht (z.B. [Application Gateway](https://azure.microsoft.com/services/application-gateway)) verwenden.
    - Da Load Balancer unabhängig von der TCP-Payload ist und keine TLS-Abladung (SSL) bereitgestellt wird, können Sie verschlüsselte End-to-End-Szenarien mithilfe von Load Balancer erstellen und erhalten dadurch ein hohes Maß an horizontaler Skalierung für TLS-Anwendungen, indem Sie die TLS-Verbindung auf dem virtuellen Computer selbst beenden.  Beispielsweise ist die TLS-Funktion zum erstellen von Sitzungsschlüsseln vom Typ und der Nummer der virtuellen Computer beschränkt, die Sie dem Back-End-Pool hinzufügen.  Wenn Sie eine „SSL-Abladung“ oder eine Behandlung der Anwendungsschicht verlangen oder die Zertifikatverwaltung auf Azure übertragen möchten, sollten Sie stattdessen den Lastenausgleich von Azure auf der siebten Schicht verwenden ([Application Gateway](https://azure.microsoft.com/services/application-gateway)).
        

* **Automatische Neukonfiguration**

    Der Load Balancer konfiguriert sich selbst sofort, wenn Sie Instanzen nach oben oder nach unten skalieren. Das Hinzufügen oder Entfernen von virtuellen Computern zum bzw. aus dem Back-End-Pool bewirkt, dass Load Balancer neu konfiguriert wird. Es werden aber keine zusätzlichen Vorgänge für die Load Balancer-Ressource ausgeführt.

* **Integritätstests**

    Load Balancer verwendet von Ihnen definierte Integritätstests, um die Integrität von Instanzen im Back-End-Pool zu ermitteln. Wenn ein Test nicht reagiert, beendet Load Balancer das Senden neuer Verbindungen an die fehlerhaften Instanzen. Vorhandene Verbindungen sind nicht betroffen und werden fortgesetzt, bis die Anwendung den Flow beendet, ein Leerlauftimeout auftritt oder der virtuelle Computer heruntergefahren wird.
     
    Load Balancer bietet [verschiedene Integritätstesttypen](load-balancer-custom-probe-overview.md#types) für TCP-, HTTP- und HTTPS-Endpunkte.

    Zudem ist bei der Verwendung von klassischen Clouddiensten ein weiterer Typ zulässig:  [Gast-Agent](load-balancer-custom-probe-overview.md#guestagent).  Dieser sollte als letzte Option für den Integritätstest gelten und nicht eingesetzt werden, wenn andere Optionen genutzt werden können.
    
* **Ausgehende (SNAT-)Verbindungen**

    Alle ausgehenden Flows von privaten IP-Adressen in Ihrem virtuellen Netzwerk zu öffentlichen IP-Adressen im Internet können in eine Front-End-IP-Adresse von Load Balancer übersetzt werden. Wenn ein öffentliches Front-End über eine Lastenausgleichsregel an einen virtuellen Back-End-Computer gebunden ist, programmiert Azure ausgehende Verbindungen so, dass sie automatisch in die IP-Adresse des öffentlichen Front-Ends übersetzt werden.

  * Es werden einfache Upgrades und eine Notfallwiederherstellung von Diensten ermöglicht, da das Front-End dynamisch einer anderen Instanz des Diensts zugeordnet werden kann.
  * Die Verwaltung von Zugriffssteuerungslisten wird vereinfacht. Zugriffssteuerungslisten, die als Front-End-IP-Adressen ausgedrückt werden, ändern sich nicht, wenn Dienste zentral hoch- oder herunterskaliert oder erneut bereitgestellt werden.  Wenn ausgehende Verbindungen in weniger IP-Adressen übersetzt werden als Computer vorhanden sind, ist ein geringeres Maß an Whitelisting nötig.

    Weitere Informationen finden Sie unter [Ausgehende Verbindungen](load-balancer-outbound-connections.md).

Der Standard-Load Balancer verfügt über zusätzliche SKU-spezifische Fähigkeiten über diese Grundlagen hinaus. Ausführlichere Informationen finden Sie im Rest dieses Artikels.

## <a name="skus"></a> Vergleich der Load Balancer-SKUs

Der Load Balancer unterstützt sowohl Basic- als auch Standard-SKUs, die sich in der Szenariostaffelung sowie den Features und Tarifen unterscheiden. Jedes Szenario, das mit dem Basic-Load Balancer möglich ist, kann auch mit dem Standard-Load Balancer erstellt werden. Tatsächlich sind die APIs für beide SKUs ähnlich, und sie werden über die Spezifikation einer SKU aufgerufen. Die API zur Unterstützung von SKUs für Load Balancer und öffentliche IP-Adressen ist ab der API-Version vom 1.8.2017 verfügbar. Beide SKUs haben die gleiche allgemeine API und Struktur.

Je nachdem, welche SKU ausgewählt wird, kann die komplette Szenariokonfiguration jedoch leicht abweichen. In der Load Balancer-Dokumentation wird darauf hingewiesen, wenn ein Artikel nur für eine bestimmte SKU gilt. Weitere Informationen zu den Unterschieden finden Sie in der folgenden Tabelle. Weitere Informationen finden Sie unter [Übersicht: Azure Standard Load Balancer](load-balancer-standard-overview.md).

>[!NOTE]
> Für neue Entwürfe sollte Load Balancer Standard verwendet werden. 

Eigenständige virtuelle Computer, Verfügbarkeitsgruppen und VM-Skalierungsgruppen können nur mit einer SKU, nie mit beiden verbunden werden. Wenn sie mit öffentlichen IP-Adressen verwendet werden, muss die Load Balancer-SKU mit der SKU für öffentliche IP-Adressen übereinstimmen. Load Balancer-SKUs und SKUs für öffentliche IP-Adressen sind nicht änderbar.

_Es ist eine bewährte Methode, SKUs explizit anzugeben, obwohl dies noch nicht obligatorisch ist._  Zu diesem Zeitpunkt werden notwendige Änderungen auf ein Minimum beschränkt. Ist keine SKU angegeben, wird dies so interpretiert, dass die Basic-SKU in der API-Version vom 1.8.2017 verwendet werden soll.

>[!IMPORTANT]
>Standard Load Balancer ist ein neues Load Balancer-Produkt und zum größten Teil eine Obermenge von Basic Load Balancer. Es gibt wichtige und wohl durchdachte Unterschiede zwischen den beiden Produkten. Jedes End-to-End-Szenario, das mit dem Basic-Load Balancer möglich ist, kann auch mit dem Standard-Load Balancer erstellt werden. Wenn Sie den Basic-Load Balancer bereits kennen, sollten Sie sich mit dem Standard-Load Balancer vertraut machen, um die neusten Änderungen im Verhalten zwischen Standard und Basic und deren Auswirkungen zu verstehen. Lesen Sie diesem Abschnitt sorgfältig.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Weitere Informationen finden Sie unter [Load Balancer-Grenzwerte](https://aka.ms/lblimits). Lesen Sie für den Standard-Load Balancer auch die ausführlicheren Informationen unter [Übersicht](load-balancer-standard-overview.md), [Preise](https://aka.ms/lbpricing) und [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Konzepte

### <a name = "publicloadbalancer"></a>Öffentlicher Load Balancer

Der öffentliche Load Balancer ordnet die öffentliche IP-Adresse und Portnummer des eingehenden Datenverkehrs der privaten IP-Adresse und Portnummer des virtuellen Computers zu und führt auch die Zuordnung des Antwortdatenverkehrs aus dem virtuellen Computer durch. Wenn Sie Regeln für den Lastenausgleich anwenden, können Sie bestimmte Typen von Datenverkehr auf verschiedene VMs oder Dienste verteilen. Sie können zum Beispiel die Netzwerklast von Webanforderungen auf mehrere Webserver verteilen.

Die folgende Abbildung zeigt einen Endpunkt für Webdatenverkehr mit Lastenausgleich, der von drei virtuellen Computern für den öffentlichen TCP-Port 80 genutzt wird. Diese drei virtuellen Computer bilden eine Gruppe mit Lastenausgleich.

![Beispiel für einen öffentlichen Load Balancer](./media/load-balancer-overview/IC727496.png)

*Abbildung: Durchführen des Lastenausgleichs für Webdatenverkehr mit einem öffentlichen Load Balancer*

Wenn Internetclients Webseitenanforderungen über den TCP-Port 80 an die öffentliche IP-Adresse einer Web-App senden, verteilt der Azure Load Balancer diese Anfragen auf die drei virtuellen Computer in der Lastenausgleichsgruppe. Weitere Informationen zu Load Balancer-Algorithmen finden Sie im Abschnitt [Load Balancer-Features](load-balancer-overview.md##fundamental-load-balancer-features) in diesem Artikel.

Standardmäßig verteilt Azure Load Balancer Netzwerkdatenverkehr gleichmäßig auf mehrere Instanzen virtueller Computer. Sie können auch Sitzungsaffinität konfigurieren. Weitere Informationen hierzu finden Sie unter [Load Balancer-Verteilungsmodus](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> Interner Load Balancer

Ein interner Load Balancer leitet Datenverkehr nur an Ressourcen weiter, die sich innerhalb eines virtuellen Netzwerks befinden oder ein VPN für den Zugriff auf die Azure-Infrastruktur verwenden. In dieser Hinsicht unterscheidet sich ein interner Load Balancer von einem öffentlichen Load Balancer. Die Azure-Infrastruktur schränkt den Zugriff auf die Front-End-IP-Adressen eines virtuellen Netzwerks ein, für die Lastenausgleich durchgeführt wird. Front-End-Adressen und virtuelle Netzwerke werden nie direkt für einen Internetendpunkt verfügbar gemacht. Interne Branchenanwendungen werden in Azure ausgeführt. Auf sie wird aus Azure oder von lokalen Ressourcen aus zugegriffen.

Ein interner Load Balancer ermöglicht die folgenden Arten von Lastenausgleich:

* **In einem virtuellen Netzwerk:** Lastenausgleich zwischen virtuellen Computern im virtuellen Netzwerk und einer Gruppe von virtuellen Computern im gleichen virtuellen Netzwerk.
* **Für ein standortübergreifendes virtuelles Netzwerk:** Lastenausgleich zwischen lokalen Computern und einer Gruppe von virtuellen Computern im gleichen virtuellen Netzwerk. 
* **Für Anwendungen mit mehreren Ebenen:** Lastenausgleich für Anwendungen mit mehreren Ebenen und Internetzugriff, wobei die Back-End-Ebenen nicht auf das Internet zugreifen können. Die Back-End-Schichten erfordern einen Lastenausgleich des Datenverkehrs aus einer mit dem Internet verbundenen Schicht (weitere Details finden Sie in der folgenden Abbildung).
* **Für Branchenanwendungen:** Lastenausgleich für Branchenanwendungen, die in Azure ohne zusätzliche Hardware oder Software für den Lastenausgleich gehostet werden. Dieses Szenario enthält lokale Server, die sich in der Gruppe der Computer befinden, für deren Datenverkehr Lastenausgleich stattfindet.

![Beispiel für einen internen Load Balancer](./media/load-balancer-overview/IC744147.png)

*Abbildung: Durchführen eines Lastenausgleichs für Anwendungen mit mehreren Ebenen mithilfe eines öffentlichen und eines internen Load Balancers*

## <a name="pricing"></a>Preise

Die Verwendung von Load Balancer Standard ist kostenpflichtig.

- Anzahl konfigurierter Lastenausgleichs- und Ausgangsregeln (NAT-Regeln für eingehenden Datenverkehr werden nicht auf die Gesamtanzahl der Regeln angerechnet.)
- Menge der eingehenden und ausgehenden verarbeiteten Daten, ungeachtet der Regel. 

Informationen zu den Preisen für den Standard-Load Balancer finden Sie auf der Seite [Load Balancer – Preise](https://azure.microsoft.com/pricing/details/load-balancer/).

Basic Load Balancer wird kostenlos angeboten.

## <a name="sla"></a>SLA

Informationen zum SLA für den Standard-Load Balancer finden Sie auf der Seite [SLA für Load Balancer](https://aka.ms/lbsla). 

## <a name="limitations"></a>Einschränkungen

- Load Balancer ist ein TCP- oder UDP-Produkt für den Lastenausgleich und die Portweiterleitung für diese spezifischen IP-Protokolle.  Regeln für den Lastenausgleich und eingehende NAT-Regeln werden für TCP und UDP, aber nicht für andere IP-Protokolle wie ICMP unterstützt. Der Load Balancer beendet die Payload des UDP- oder TCP-Flows nicht, antwortet nicht auf diese und interagiert auch nicht auf andere Weise mit ihr. Es handelt sich nicht um einen Proxy. Eine erfolgreiche Validierung der Konnektivität auf ein Front-End muss über einen In-Band-Zugriff mit dem Protokoll ausgeführt werden, das in einer Regel für den Lastenausgleich oder die eingehende Netzwerkadressenübersetzung (TCP oder UDP) verwendet wird, _und_ mindestens einer Ihrer virtuellen Computer muss eine Antwort für einen Client generieren, damit eine Antwort von einem Front-End angezeigt werden kann.  Wenn Sie keine In-Band-Antwort vom Load Balancer-Front-End erhalten, deutet dies darauf hin, dass keine virtuellen Computer antworten konnten.  Die Interaktion mit einem Load Balancer-Front-End ist nicht möglich, wenn ein virtueller Computer nicht antworten kann.  Dies gilt auch für ausgehende Verbindungen, für die die [Portmaskierung mit SNAT](load-balancer-outbound-connections.md#snat) nur für TCP und UDP unterstützt wird. Andere IP-Protokolle wir ICMP schlagen fehl.  Weisen Sie eine öffentlichen IP-Adresse auf Instanzebene zu, die eingeschränkt werden soll.
- Im Gegensatz zu öffentlichen Load Balancer-Instanzen, die [ausgehende Verbindungen](load-balancer-outbound-connections.md) bereitstellen, wenn ein Übergang von privaten IP-Adressen innerhalb des virtuellen Netzwerks auf öffentliche IP-Adressen vorgenommen wird, übersetzen interne Load Balancer-Instanzen keine ursprünglichen ausgehenden Verbindungen mit dem Front-End einer internen Load Balancer-Instanz, da sich beide in einem privaten IP-Adressraum befinden.  Dadurch wird das Risiko einer SNAT-Portauslastung innerhalb eines eindeutigen internen IP-Adressraums gemindert, in dem keine Übersetzung erforderlich ist.  Eine Nebenwirkung besteht darin, wenn ein von einem virtuellen Computer ausgehender Flow in den Back-End-Pool versucht, einen Flow auf das Front-End der internen Load Balancer-Instanz in den Pool auszuführen, in dem er sich befindet, _und_ sich selbst zugewiesen ist, stimmen beide Verzweigungen des Flows nicht überein. Somit schlägt der Flow fehl.  Wenn der Flow nicht dem virtuellen Computer im Back-End-Pool zugeordnet ist, die den Flow auf das Front-End erstellt hat, ist der Flow erfolgreich.   Wenn der Flow sich selbst zugeordnet ist, scheint es, als würde der ausgehende Flow vom virtuellen Computer auf das Front-End ausgehen und als würde der zugehörige eingehende Flow vom virtuellen Computer selbst ausgehen. Aus der Sicht des Gastbetriebssystems stimmen die eingehenden und ausgehenden Bestandteile desselben Flows innerhalb des virtuellen Computers nicht überein. Der TCP-Stack erkennt diese Teile desselben Flows als Teil desselben Flows an, da Quelle und Ziel nicht übereinstimmen.  Wenn der Flow keinem anderen virtuellen Computer im Back-End-Pool zugeordnet ist, stimmen die Teile des Flows überein, und der virtuelle Computer kann erfolgreich auf den Flow antworten.  Dieses Szenario hat zeitweilige Verbindungstimeouts zur Folge, wenn der Flow zum selben Back-End zurückkehrt, vom dem er ursprünglich stammt. Es gibt einige häufig verwendete Workarounds, damit Sie dieses Szenario zuverlässig abschließen können. Diese Workarounds umfassen entweder das Einfügen einer Proxyebene hinter der internen Load Balancer-Instanz oder die [Verwendung von DSR-Stilregeln](load-balancer-multivip-overview.md).  Kunden können eine interne Load Balancer-Instanz mit dem Proxy eines Drittanbieters kombinieren oder die interne [Application Gateway](../application-gateway/application-gateway-introduction.md)-Instanz durch Proxyszenarien mit Beschränkung HTTP/HTTPS ersetzen. Sie können zwar einen öffentlichen Load Balancer zur Entschärfung verwenden, aber in dem daraus resultierenden Szenarios kann es zu einer [SNAT-Überlastung](load-balancer-outbound-connections.md#snat) kommen. Daher sollten Sie diesen in dem Zusammenhang nur mit Bedacht verwenden.

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun einen Überblick über die Verwendung von Azure Load Balancer erhalten. Wenn Sie loslegen möchten, erstellen Sie einen Load Balancer, erstellen Sie VMs mit einer installierten benutzerdefinierten IIS-Erweiterung, und richten Sie den Lastenausgleich für die Web-App zwischen den VMs ein. Weitere Informationen finden Sie im Schnellstart [Schnellstart: Erstellen eines öffentlichen Load Balancers](quickstart-create-basic-load-balancer-portal.md).
