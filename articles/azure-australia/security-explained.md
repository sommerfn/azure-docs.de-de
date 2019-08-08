---
title: Überblick über Sicherheit in Azure Australien
description: Die am häufigsten gefragten Informationen zu den australischen Regionen und den spezifischen Anforderungen zum Erfüllen der Richtlinien, Bestimmungen und Gesetze der australischen Regierung.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 212d14e31c152e50c216f2f34fb225c29fe054b5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571110"
---
# <a name="azure-australia-security-explained"></a>Überblick über Sicherheit in Azure Australien

In diesem Artikel werden einige der häufig gestellten Fragen und Interessensbereiche für australische Regierungsbehörden erläutert, die Microsoft Azure in Australien untersuchen, entwerfen und Bereitstellen.

## <a name="irap-and-certified-cloud-services-list-ccsl-documents"></a>Dokumente des IRAP (Information Security Registered Assessors Program) und der Liste der zertifizierten Clouddienste (Certified Cloud Services List, CCSL)

Das Australian Cyber Security Centre (ACSC) stellt ein Zertifizierungsschreiben, einen Zertifizierungsbericht und einen Verbraucherleitfaden für den Dienst bereit, wenn er der CCSL hinzugefügt wird.

Microsoft ist zurzeit in der CCSL für Azure, Office 365 und Dynamics 365 CRM aufgeführt.

Microsoft macht seine Überwachungs-, Bewertungs- und ACSC-Zertifizierungsdokumente Verbrauchern und Partnern auf einer spezifischen Australien-Seite des [Microsoft Service Trust Portal](https://aka.ms/au-irap) verfügbar.

## <a name="dissemination-limiting-markers-dlm-and-protected-certification"></a>Dissemination Limiting Markers (DLM) und PROTECTED-Zertifizierung

Der Prozess, Systeme, z.B. Clouddienste, für die Verwendung durch Regierungsorganisationen zu genehmigen, wird im [Information Security Manual (ISM)](https://acsc.gov.au/infosec/ism/) definiert, das vom Australian Cyber Security Centre (ACSC) erstellt und veröffentlicht wird. Das Australian Cyber Security Centre (ACSC) ist die Stelle innerhalb des ASD, die für Cybersicherheit und Cloudzertifizierung zuständig ist.

Der Genehmigungsprozess umfasst zwei Schritte:

1. Sicherheitsbewertung (IRAP): Ein Prozess, bei dem registrierte Fachleute Systeme, Dienste und Lösungen anhand der technischen Kontrollen im ISM bewerten und auswerten, ob die Kontrollen effektiv implementiert wurden. Außerdem werden durch die Bewertung alle spezifischen Risiken identifiziert, die die Genehmigungsbehörde vor der Ausstellung einer Betriebsgenehmigung (Approval to Operate, ATO) berücksichtigen muss.

1. Betriebsgenehmigung (Approval to Operate, ATO): Der Prozess, bei dem ein leitender Mitarbeiter einer Regierungsbehörde das Restrisiko eines Systems für die von diesem verarbeiteten, gespeicherten und übertragenen Daten formal anerkennt und akzeptiert.  Eine Eingabe für diesen Prozess ist die Sicherheitsbewertung.

Die Bewertungsstufe PROTECTED für die Azure-Dienste gibt an, dass das Vorhandensein und die Funktionalität der Implementierung der Sicherheitskontrollen bestätigt wurde, die für die Speicherung und Verarbeitung von als PROTECTED und niedriger eingestuften Daten erforderlich sind.

## <a name="australian-data-classification-changes"></a>Änderungen an der australischen Datenklassifizierung

Am 1. Oktober 2018 hat die australische Generalstaatsanwaltschaft Änderungen am Protective Security Policy Framework (PSPF) angekündigt, insbesondere ein neues [sensibles und klassifiziertes Informationssystem](https://www.protectivesecurity.gov.au/information/sensitive-classified-information/Pages/default.aspx).

![Überarbeitete PSPF-Klassifizierungen](media/pspf-classifications.png)

Alle australischen Behörden und Organisationen, die unter dem PSPF arbeiten, sind von diesen Änderungen betroffen. Die wichtigste Änderung, die sich auf unsere aktuellen IRAP/CCSL-Zertifizierungen auswirkt, besteht darin, dass die aktuellen Dissemination Limiting Markers (DLM) eingestellt wurden. Die Markierung **OFFICIAL: Sensitive** ersetzt die verschiedenen DLMs, die für den Schutz vertraulicher Informationen verwendet werden. Die Änderung hat auch drei Markierungen zur Verwaltung von Informationen eingeführt, die auf alle offiziellen Informationen in allen Vertraulichkeits- und Klassifizierungsstufen angewendet werden können. Die Klassifizierung **PROTECTED** bleibt unverändert.

Der Begriff „Unclassified“ wird aus dem neuen System entfernt, und der Begriff „Unofficial“ wird auf Nicht-Regierungsinformationen angewendet, obwohl diese keine formale Markierung erfordern.

## <a name="choosing-an-azure-region-for-my-official-sensitive-and-protected-workloads"></a>Auswählen einer Azure-Region für als „OFFICIAL:Sensitive“ und „PROTECTED“ eingestufte Workloads

Als **OFFICIAL: Sensitive** und **PROTECTED** zertifizierte Azure-Dienste werden in allen vier australischen Rechenzentrumsregionen bereitgestellt („Australien, Osten“, „Australien, Südosten“, „Australien, Mitte“ und „Australien, Mitte 2“). Der vom ACSC ausgegebene Zertifizierungsbericht empfiehlt jedoch, dass als **PROTECTED** eingestufte Daten in den Azure Government-Regionen in Canberra bereitgestellt werden, wenn dort ein Dienst verfügbar ist. Ausführlichere Informationen zu den als **PROTECTED** zertifizierten Azure-Diensten finden Sie auf der [Australien-Seite des STP](https://aka.ms/au-irap).

>[!NOTE]
>Microsoft empfiehlt, Daten von Regierungsbehörden aller Vertraulichkeitsstufen und Klassifizierungen in den Regionen „Australien, Mitte“ und „Australien, Mitte 2“ bereitzustellen, da sie speziell für die Bedürfnisse von Regierungsbehörden entworfen wurden und gemäß diesen betrieben werden.

Weitere Informationen zur besonderen Natur der Azure Australien-Regionen finden Sie unter [Azure-Regionen in „Australien, Mitte“](https://azure.microsoft.com/global-infrastructure/australia/).

## <a name="how-microsoft-separates-classified-and-official-data"></a>Wie Microsoft klassifizierte und offizielle Daten trennt

Microsoft führt Azure und Office 365 in Australien so aus, als ob alle Daten vertraulich und/oder klassifiziert wären, und hebt somit die Sicherheitskontrollen auf diese hohe Stufe an.

Die Infrastruktur, die Azure unterstützt, bedient potenziell Daten mehrerer Klassifizierungen.  Da wir jedoch davon ausgehen, dass Kundendaten klassifiziert sind, stehen die entsprechenden Kontrollen bereit. Microsoft hat für seine Dienste einen Baseline-Sicherheitsstatus implementiert, der den PSPF-Anforderungen zum Speichern und Verarbeiten von als **PROTECTED** eingestuften Informationen entspricht.

Um unseren Kunden gegenüber zu gewährleisten, dass ein Mandant in Azure nicht durch andere Mandanten gefährdet ist, implementiert Microsoft umfassende Defense-in-Depth-Kontrollen.

Neben den in der Microsoft Azure-Plattform implementierten Funktionen können zusätzliche, vom Kunden konfigurierbare Kontrollen, wie z.B. Verschlüsselung mit vom Kunden verwalteten Schlüsseln, geschachtelte Virtualisierung und Just-in-Time-Administratorzugriff, das Risiko weiter verringern. Innerhalb der australischen Azure Government-Regionen in Canberra ist ein Prozess für ein formales Whitelisting nur für australische & neuseeländische Regierungsbehörden und wichtige nationale Organisationen im Infrastrukturbereich vorhanden. Diese Communitycloud bietet Organisationen, die für potenzielle Co-Mandanten-Risiken anfällig sind, zusätzliche Sicherheit.

Der Microsoft Azure-Zertifizierungsbericht **PROTECTED** bestätigt, dass diese Kontrollen für die Speicherung und Verarbeitung von als **PROTECTED** klassifizierten Daten und deren Isolation wirksam sind.

## <a name="relevance-of-irapccsl-to-state-government-and-critical-infrastructure-providers"></a>Relevanz von IRAP/CCSL für Landesbehörden und wichtige Infrastrukturanbieter

Viele Landesbehörden und wichtigen Infrastrukturanbieter integrieren die Anforderungen der Bundesbehörden in ihre Sicherheitsrichtlinien und ihr Sicherungsframework. Diese Organisationen verarbeiten ebenfalls als **OFFICIAL**, **OFFICIAL: Sensitive** und in gewissem Umfang als **PROTECTED** klassifizierte Daten, entweder im Rahmen ihrer Interaktion mit Bundesbehörde oder in Bezug auf eigene Daten.

Die australische Regierung konzentriert sich immer stärker auf Richtlinien und Gesetze zum Schutz von Nicht-Regierungsdaten, die sich fundamental auf die Sicherheit und den wirtschaftlichen Wohlstand von Australien auswirken. Als solche sind die Azure Australien-Regionen und die CCSL-Zertifizierung für alle diese Branchen relevant.

![Kritische Infrastruktursektoren](media/nci-sectors.png)

Die Microsoft-Zertifizierungen veranschaulichen, dass Azure-Dienste einer umfassenden, strengen und formalen Bewertung der implementierten Sicherheitsmaßnahmen unterzogen und für die Verarbeitung solch hochsensibler Daten zugelassen wurden.

## <a name="location-and-control-of-microsoft-data-centres"></a>Speicherort und Kontrolle von Microsoft-Rechenzentren

Es ist eine obligatorische Voraussetzung für Regierungsbehörden und kritische Infrastrukturen, den Speicherort des Rechenzentrums und den Besitz der Clouddienste, die die Daten verarbeiten, explizit zu kennen.  Microsoft ist als Hyperscale-Cloudanbieter einzigartig in der Bereitstellung umfassender Informationen zu Speicherorten und Besitzern.

Die Azure Australien-Regionen („Australien, Mitte“ und „Australien, Mitte 2“) werden innerhalb der Räumlichkeiten der CDC-Rechenzentren betrieben.  Die CDC-Rechenzentren werden von Australien kontrolliert und sind zu 48 % im Besitz von Commonwealth Superannuation Corporation, zu 48 % im Besitz von Infratil (einem sowohl an der australischen als auch neuseeländischen Börse gelisteten Investmentunternehmen mit Sitz in Neuseeland, mit langfristigen Investitionen in Unternehmungen der Infrastrukturbranche) und zu 4 % in Verwaltung durch Australien.  

Für die Verwaltung der CDC-Rechenzentren bestehen vertragliche Zusicherungen, und die australische Regierung hat eine zukünftige Übertragung von Besitz und Kontrolle eingeschränkt. Diese Transparenz der Lieferkette und des Besitzes über die Microsoft-Partnerschaft mit CDC-Rechenzentren steht in Einklang mit den Prinzipien der [Whole-of-Government Hosting Strategy](https://www.dta.gov.au/our-projects/whole-government-hosting-strategy) und der Definition eines **zertifizierten souveränen Rechenzentrums**.

## <a name="the-azure-services-that-are-included-in-the-current-ccsl-certification"></a>Die in der aktuellen CCSL-Zertifizierung enthaltenen Azure-Dienste

Im Juni 2017 hat das ACSC 41 Azure-Dienste für die Speicherung und Verarbeitung von Daten der Stufe **Unclassified: DLM** zertifiziert. Im April 2018 wurden 24 dieser Dienste für als **PROTECTED** klassifizierte Daten zertifiziert.

Die Verfügbarkeit von ACSC-zertifizierten Azure-Diensten in den Azure-Regionen in Australien sieht wie folgt aus (die fett formatierten Azure-Dienste verfügen über die **PROTECTED**-Zertifizierung):

|Azure-Regionen in „Australien, Mitte“|Nicht regionale Dienste und andere Regionen|
|---|---|
|API Management, App Gateway, Anwendungsdienste, **Azure Automation, Azure-Portal, Azure Backup, Azure Batch, Azure Cloud Services**, Cosmos DB, Azure Event Hubs, **ExpressRoute**, HDInsight, **Azure Key Vault**, Azure Load Balancer, Log Analytics, **Multi-Factor Authentication**, Redis Cache, **Azure Resource Manager, Azure Service Bus, Azure Service Fabric, Azure Site Recovery, Azure SQL-Datenbank, Azure Storage**, Azure Traffic Manager, **Azure Traffic Manager, Virtual Network, VPN Gateway**|**Azure Active Directory**, Azure CDN, Azure Data Catalog, **Import Export, Azure Information Protection, Azure IoT Hub**, Azure Machine Learning, Azure Media Services, **Azure Notification Hubs**, Power BI, **Security Centre, Azure Scheduler**, Azure Search, Azure Stream Analytics|
|

Microsoft veröffentlicht eine [Übersicht über die Compliance von Microsoft Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/44/Microsoft%20Azure%20Compliance%20Offerings.pdf), in der alle zum Umfang gehörigen Dienste für alle globalen, behördlichen, branchenspezifischen und regionalen Compliance- und Bewertungsprozesse aufgeführt sind, die Azure durchläuft, einschließlich IRAP/CCSL.

## <a name="azure-service-not-listed-or-assessed-at-a-lower-level-than-i-need"></a>Azure-Dienst ist nicht aufgeführt oder für eine niedrigere Stufe bewertet, als benötigt

Dienste, die nicht zertifiziert sind oder für **OFFICIAL: Sensitive** aber nicht **PROTECTED** zertifiziert sind, können zusammen mit oder als Teil einer Lösung verwendet werden, die als **PROTECTED** eingestufte Daten hostet, vorausgesetzt für die Dienste gilt Folgendes:
1. Als **PROTECTED** eingestufte Daten werden nicht unverschlüsselt gespeichert oder verarbeitet. Oder:
1. Sie haben eine Risikobewertung durchgeführt, und den Dienst zum Speichern von als **PROTECTED** eingestuften Daten selbst genehmigt.

Wenn Sie einen Dienst, der nicht in der CCSL enthalten ist, verwenden möchten, um als **OFFICIAL** eingestufte Daten zu speichern und zu verarbeiten, können Sie dies tun, aber das ISM erfordert, dass Sie das ACSC schriftlich darüber benachrichtigen, bevor Sie einen Vertrag mit einem Clouddienstanbieter eingeben oder verlängern.

Für alle von einer Behörde für **PROTECTED**-Workloads verwendete Dienste muss die Sicherheit weiterhin gemäß den im ISM beschriebenen Prozessen und dem von der Behörde verwalteten IRAP-Bewertungsprozess in der [DTA Secure Cloud Strategy](https://www.dta.gov.au/files/cloud-strategy/secure-cloud-strategy.pdf) bewertet und genehmigt werden.

![DTA Secure Cloud Strategy-Zertifizierungsprozess](media/certification.png)

Microsoft führt kontinuierlich Bewertungen seiner Dienste durch, um sicherzustellen, dass die Plattform sicher ist und für die Verwendung durch australische Regierungsbehörden geeignet ist. Wenden Sie sich daher an Microsoft, wenn Sie Unterstützung bei einem Dienst benötigen, der derzeit in der CCSL nicht unter **PROTECTED** aufgeführt ist.

Da Microsoft über eine Reihe von Diensten verfügt, die in der CCSL sowohl für die Klassifizierungen **Unclassified DLM** als auch **PROTECTED** zertifiziert sind, ist es nach dem ISM erforderlich, mindestens alle zwei Jahre eine IRAP-Bewertung der Dienste durchzuführen. Microsoft nimmt eine jährliche Bewertung vor, die auch die Möglichkeit bietet, zusätzliche Dienste zu einzubeziehen.

## <a name="certified-protected-gateway-in-azure"></a>Zertifiziertes PROTECTED-Gateway in Azure

Aufgrund von Einschränkungen hinsichtlich der Anzahl von SIGs, die im Rahmen des Gateway Consolidation-Programms zulässig sind, betreibt Microsoft kein für Regierungsorganisationen zertifiziertes Secure Internet Gateway (SIG).  Die erwarteten und notwendigen Funktionen eines SIG können jedoch in Microsoft Azure konfiguriert werden.

Durch die **PROTECTED**-Zertifizierung von Azure-Diensten gibt das ACSC bestimmte Empfehlungen für Behörden zum Herstellen einer Verbindung mit Azure und zur Implementierung der Netzwerksegmentierung zwischen Sicherheitsdomänen, z.B. zwischen **PROTECTED** und dem Internet. Diese Empfehlungen umfassen die Verwendung von Netzwerksicherheitsgruppen, Firewalls und virtuellen privaten Netzwerken.  Das ACSC empfiehlt die Verwendung einer virtuellen Gatewayappliance. In Azure sind mehrere virtuelle Appliances verfügbar, für die es in der Liste der vom ASD ausgewerteten Produkte eine physische Entsprechung gibt oder die nach dem Common Criteria-Schutzprofil ausgewertet wurden und im Common Criteria-Portal aufgeführt sind. Diese Produkte werden vom ASD als Unterzeichner des Common Criteria Recognition Arrangement (CCRA) anerkannt.

Microsoft hat eine Anleitung zur Implementierung von Azure-basierten Funktionen erstellt, die die zum Schutz der Grenze zwischen verschiedenen Sicherheitsdomänen erforderlichen Sicherheitsfunktionen bereitstellen, die in Kombination das Gegenstück zu einem zertifizierten SIG darstellen. Es gibt eine Reihe von Partnern, die Sie bei Entwurf und Implementierung dieser Funktionen unterstützen können, sowie eine Reihe von verfügbaren Partnerlösungen zum gleichen Zweck.

## <a name="security-clearances-and-citizenship-of-microsoft-support-personnel"></a>Sicherheitsfreigaben und die Staatsbürgerschaft von Microsoft-Supportmitarbeitern

Microsoft betreibt seine Dienste weltweit mit geprüftem und geschultem Sicherheitspersonal.  Mitarbeiter, die unbegleiteten physischen Zugriff auf Einrichtungen in Sydney und Melbourne haben, verfügen über Baseline-Sicherheitsfreigaben der australischen Regierung. Mitarbeiter in den Regionen „Australien, Mitte“ und „Australien, Mitte 2“ verfügen mindestens über Negative Vetting 1 (NV1)-Freigaben (geeignet für Daten, die als **SECRET** eingestuft sind). Dies bietet Kunden die zusätzliche Sicherheit, dass die Mitarbeiter in Rechenzentren, die Azure betreiben, eine hohe Vertrauenswürdigkeit aufweisen.

Microsoft verfügt über eine Zugriffsrichtlinie ohne feste Berechtigungen, bei der der Zugriff durch ein System aus Just-in-Time- und Just-Enough-Administration basierend auf rollenbasierten Zugriffssteuerungen gewährt wird. In den meisten Fällen benötigen unsere Administratoren keinen Zugriff auf und keine Berechtigungen für Kundendaten, um Probleme zu beheben und den Dienst zu verwalten.  Durch ein hohes Maß an Automatisierung und Skripting bei Aufgaben für die Remoteausführung ist kein direkter Zugriff auf Kundendaten erforderlich.

Die Generalstaatsanwaltschaft hat bestätigt, dass die Personalsicherheitsrichtlinien und -verfahren von Microsoft innerhalb von Azure die Absicht der „PSPF Access to Information“-Bestimmungen in INFOSEC-9 erfüllen.

## <a name="storing-international-traffic-of-arms-regulations-itar-or-export-administration-regulations-ear-data"></a>Speichern von ITAR-Daten (International Traffic in Arms, US-amerikanische Vorschriften für den internationalen Waffenhandel) oder EAR-Daten (US Export Administration Regulations, Ausführungsverordnung des amerikanischen Exportkontrollrechts)

Die technischen Kontrollen in Azure, die Kunden bei der Erfüllung ihrer Verpflichtungen in Bezug auf Daten, die einer Ausfuhrkontrolle unterliegen, unterstützen, sind in Azure weltweit gleich. Wichtig ist, dass keine „Häkchen im ITAR/Ear Feld“ gesetzt werden müssen, da es kein formales Bewertungs- und Zertifizierungsframework für Daten gibt, die Ausfuhrkontrollen unterliegen.

Für Azure Government und Office 365 US Government for Defense haben wir zusätzliche Vertrags- und Prozessmeasures eingerichtet, um Kunden zu unterstützen, die Ausfuhrkontrollen unterliegen. Diese zusätzlichen Vertragsklauseln und die garantierte US-amerikanische Unterstützung und Verwaltung der Azure-Regionen sind für Australien nicht vorhanden.

Das bedeutet nicht, dass Azure in Australien nicht für ITAR/EAR verwendet werden kann, aber Sie müssen die Einschränkungen, die Ihnen durch ihre Exportlizenz auferlegt werden, genau verstehen. Außerdem müssen Sie zusätzliche Schutzmaßnahmen implementieren, um diese Verpflichtungen zu erfüllen, bevor Sie Azure zum Speichern dieser Daten verwenden. Beispielsweise kann es erforderlich sein, die Nationalität als Attribut in Azure Active Directory zu erstellen, mit Azure Information Protection Verschlüsselungsregeln für die Daten zu erzwingen und den Zugriff darauf auf US-Bürger bzw. nur diejenigen Nationalitäten zu beschränken, die in der Exportlizenz enthalten sind, alle Daten vor dem Speichern in Azure lokal zu verschlüsseln, einen Kundenschlüssel oder Hold-Your-Own-Key für ITAR-Daten zu verwenden... Die Liste lässt sich beliebig fortsetzen.

Da es sich bei ITAR nicht um eine formale Zertifizierung handelt, müssen Sie verstehen, welche Begrenzungen und Einschränkungen mit der Exportlizenz verbunden sind, und dann überprüfen, ob in Azure ausreichende Kontrollen vorhanden sind, um diese Anforderungen zu erfüllen. In diesem Fall ist eines der genauestens zu betrachtenden Probleme der Zugriff durch unsere Techniker, die möglicherweise nicht über eine für die Exportlizenz zulässige Nationalität verfügen.

## <a name="next-steps"></a>Nächste Schritte

Im Artikel zu [Azure VPN Gateway](vpn-gateway.md) finden Sie die ISM-kompatible Konfiguration und Implementierung der VPN-Konnektivität in Azure Australien.
