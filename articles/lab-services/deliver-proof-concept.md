---
title: Bereitstellen eines Proof of Concept – Azure DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Proof of Concept bereitstellen, damit Azure DevTest Labs erfolgreich in eine Unternehmensumgebung integriert werden kann.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: tanmayeekamath
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: takamath
ms.openlocfilehash: 6739679b1687393737dda1ded4265a95c4fce169
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501604"
---
# <a name="delivering-a-proof-of-concept"></a>Bereitstellen eines Proof of Concept 

Eines der wichtigsten Szenarien für Azure DevTest Labs ist die Bereitstellung von Entwicklungs- und Testumgebungen in der Cloud. Beispiele:

* Erstellen von Entwicklerdesktops in der Cloud
* Konfigurieren von Umgebungen für Tests
* Ermöglichen des Zugriffs auf virtuelle Computer und andere Azure-Ressourcen
* Einrichten eines Sandboxbereichs für Entwickler zum Lernen und Experimentieren

DevTest Labs bietet auch den Richtlinienrahmen und Kostenkontrollen, um dem Unternehmen die Bereitstellung von „Self-Service-Azure“ für Entwickler zu ermöglichen, die Sicherheitsrichtlinien des Unternehmens, gesetzliche Bestimmungen und Konformitätsrichtlinien einhalten. 

Jedes Unternehmen hat unterschiedliche Anforderungen in Hinsicht darauf, wie Azure DevTest Labs erfolgreich in die jeweilige Umgebung integriert werden kann. In diesem Artikel werden die häufigsten Schritte beschrieben, die Unternehmen normalerweise ausführen müssen, um einen erfolgreichen Proof of Concept zu gewährleisten – den ersten Schritt zu einer erfolgreichen End-to-End-Bereitstellung. 

## <a name="getting-started"></a>Erste Schritte 

Um mit der Bereitstellung eines Proof of Concept zu beginnen, ist es wichtig, zunächst etwas über Azure und DevTest Labs zu erfahren.  Hier sind einige Ressourcen für den Einstieg: 

* [Grundlegendes zum Azure-Portal](https://azure.microsoft.com/features/azure-portal/)
* [Grundlagen von DevTest Labs](devtest-lab-overview.md)
* [Von DevTest Labs unterstützte Szenarien](devtest-lab-guidance-get-started.md)
* [Dokumentation zu DevTest Labs im Unternehmen](devtest-lab-guidance-prescriptive-adoption.md)
* [Einführung in Azure-Netzwerke](../virtual-network/virtual-networks-overview.md)

## <a name="prerequisites"></a>Voraussetzungen 

Zum erfolgreichen Abschluss eines Pilotprojekts oder Proof of Concept mit DevTest Labs müssen einige Voraussetzungen erfüllt sein: 

* **Azure-Abonnement**: Unternehmen verfügen häufig über ein bestehendes [Enterprise Agreement](https://azure.microsoft.com/pricing/purchase-options/enterprise-agreement/), das den Zugriff auf Azure ermöglicht, und es kann ein bestehendes oder neues Abonnement für DevTest Labs verwendet werden. Alternativ kann ein [Visual Studio-Abonnement](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) während der Pilotphase verwendet werden (Nutzung der kostenlosen Azure-Guthaben). Wenn keine dieser Optionen verfügbar ist, kann ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/search/?&OCID=AID719825_SEM_g4lyBqgB&lnkd=Bing_Azure_Brand&msclkid=ecc4275a31b61375749e7a5322c20de8&dclid=CMGW5-m78-ICFaLt4QodmUwGtQ) erstellt und verwendet werden. Ist ein Enterprise Agreement vorhanden, stellt die Verwendung eines [Enterprise Dev/Test-Abonnements](https://azure.microsoft.com/offers/ms-azr-0148p/) eine gute Möglichkeit dar, um Zugriff auf Windows 10/Windows 8.1-Clientbetriebssysteme und vergünstigte Preise für Entwicklungs- und Testworkloads zu erhalten. 
* **Azure Active Directory-Mandant**:  Damit Benutzer verwaltet werden können (z.B. Hinzufügen von Benutzern oder Hinzufügen von Lab-Besitzern), müssen diese Benutzer Teil des [Azure Active Directory-Mandanten](https://azure.microsoft.com/services/active-directory/) sein, der im Azure-Abonnement für das Pilotprojekt verwendet wird. Häufig richten Unternehmen eine [Hybrididentität](../active-directory/hybrid/whatis-hybrid-identity.md) ein, damit Benutzer die eigene lokale Identität in der Cloud nutzen können, doch ist dies für das DevTest Labs-Pilotprojekt nicht erforderlich. 

## <a name="scoping-of-the-pilot"></a>Umfang des Pilotprojekts 

Es ist sehr wichtig, ein geeignetes Pilotprojekt zu planen, bevor Sie mit der Implementierung beginnen. Da im Voraus bekannt ist, dass die Ressourcen nicht unbegrenzt zur Verfügung stehen, bestehen bereits entsprechende Erwartungen an die Benutzer des Pilotprojekts. 

> [!IMPORTANT]
> An dieser Stelle muss noch einmal deutlich darauf hingewiesen werden, wie wichtig es ist, den Umfang des Pilotprojekts genau festzulegen und die Erwartungen im Voraus zu definieren.

Es gibt wichtige Fragen, die vor dem Starten des Pilotprojekts beantwortet werden müssen: 

* Was möchten Sie aus dem Pilotprojekt lernen, und wie sieht eine erfolgreiche Umsetzung aus? 
* Welche Workloads oder Szenarien werden im Pilotprojekt abgedeckt? Es ist wichtig, nur eine kleine Gruppe zu definieren, damit sichergestellt ist, dass der Umfang des Pilotprojekts schnell festgelegt und das Projekt schnell abgeschlossen werden kann. 
* Welche Ressourcen müssen im Lab verfügbar sein? Zum Beispiel: benutzerdefinierte Images, Marketplace-Images, Richtlinien, Netzwerktopologie usw. 
* Wer sind die Endbenutzer/Teams, die am Pilotprojekt zum Überprüfen der Benutzererfahrung beteiligt sind?  
* Wie lange dauert das Pilotprojekt? Wählen Sie einen Zeitrahmen aus, der gut zum geplanten Umfang passt, z.B. zwei Wochen oder einen Monat. 
* Was geschieht nach Abschluss des Pilotprojekts mit den zugeordneten Ressourcen, die während des Projekts verwendet wurden? Haben Sie die Absicht, die Pilotprojektressourcen zu verwerfen?
   
   Wenn beabsichtigt ist, die virtuellen Computer und Labs am Ende des Pilotprojekts zu entsorgen, können wir einfach ein einzelnes Abonnement für das Pilotprojekt einrichten und alle unsere Arbeiten dort ausführen, während wir parallel die Fragen zum umfassenden Rollout lösen. 

Es besteht allgemein die Tendenz, das Pilotprojekt „perfekt“ zu machen, damit es genau den endgültigen Zustand darstellt, der nach dem Rollout des Diensts im Unternehmen gegeben sein wird. Dies ist aber eine falsche Annahme. Je mehr Sie sich dem „perfekten“ Zustand nähern, desto mehr müssen Sie bereits abgeschlossen haben, *bevor* Sie mit dem Pilotprojekt beginnen. Der Zweck des Pilotprojekts besteht darin, die richtigen Entscheidungen für das zentrale Hochskalieren und den Rollout des endgültigen Diensts zu treffen. 

Der Schwerpunkt des Pilotprojekts sollte auf der Auswahl der absolut notwendigen minimalen Workloads und Abhängigkeiten liegen, damit die Frage beantwortet wird, ob Azure DevTest Labs der richtige Dienst für Ihr Unternehmen ist. Es ist ratsam, die einfachste Workload mit den geringsten Abhängigkeiten auszuwählen, um einen schnelleren und klaren Erfolg sicherzustellen. Wenn das nicht möglich ist, besteht die nächstbeste Möglichkeit darin, eine möglichst repräsentative Workload auszuwählen, die potenzielle Komplexität bietet, damit der Erfolg in der Pilotphase in der Phase der horizontalen Skalierung repliziert werden kann. 

Das folgende Beispiel zeigt einen gut ausgelegten Proof of Concept. 

## <a name="example-proof-of-concept-plan"></a>Beispiel: Proof of Concept-Plan 

In diesem Abschnitt wird ein Beispiel für die Bereichsdefinition eines Proof of Concept des Pilotprojekts für DevTest Labs gezeigt. 

> [!TIP]
> Es wird dringend empfohlen, das in diesem Abschnitt beschriebene Beispiel nicht zu überspringen, um die Möglichkeit einer fehlerhaften Einrichtung Ihres Projekts zu minimieren. 

### <a name="overview"></a>Übersicht 

Wir planen die Entwicklung einer neuen Umgebung in Azure auf Grundlage von DevTest Labs, die von Anbietern als isolierte Umgebung aus dem Unternehmensnetzwerk genutzt werden kann. Um festzustellen, ob die Lösung die Anforderungen erfüllt, entwickeln wir einen Proof of Concept zum Überprüfen der End-to-End-Lösung und haben mehrere Anbieter zum Testen und Überprüfen der Benutzererfahrung einbezogen. 

### <a name="outcomes"></a>Ergebnisse 

Beim Erstellen eines Proof of Concept konzentrieren wir uns zunächst auf die Ergebnisse (was möchten wir erreichen). Diese sind hier aufgelistet.  Am Ende des Proof of Concept erwarten wir Folgendes: 

* Eine funktionierende End-to-End-Lösung, mit der Anbieter Gastkonten in Azure Active Directory (Azure AD) nutzen können, um auf eine isolierte Umgebung in Azure zuzugreifen, die über die für ihre Produktivität erforderlichen Ressourcen verfügt. 
* Mögliche Blockierprobleme, die sich auf die breitere Verwendung und Einführung auswirken, sind aufgelistet und verstanden.
* Die Personen, die an der Entwicklung des Proof of Concept beteiligt sind, haben gute Kenntnisse des gesamten Codes. Sie verstehen auch betroffene Sicherheiten und sind zuversichtlich in Hinsicht auf eine breitere Einführung.

### <a name="open-questions--prerequisites"></a>Offene Fragen und Voraussetzungen 

* Haben wir ein Abonnement erstellt, das wir für dieses Projekt verwenden können? 
* Haben wir einen Azure AD-Mandanten und einen globalen Azure AD-Administrator bestimmt, der Hilfe und Anleitung bei Azure AD-bezogene Fragen bieten kann? 
* Ein Ort, an dem die am Projekt beteiligten Personen zusammenarbeiten können: 

   * Quellcode und Skripts (z.B. Azure Repos) 
   * Dokumente (z.B. Teams oder SharePoint)  
   * Unterhaltungen (z.B. Microsoft Teams) 
   * Arbeitselemente (z.B. Azure Boards) 
* Welche Ressourcen sind für Anbieter erforderlich? Dazu gehören sowohl lokal auf den virtuellen Computern als auch auf anderen erforderlichen Servern vorhandene Ressourcen sowie im Netzwerk verfügbare Anwendungen. 
* Werden die virtuellen Computer einer Domäne in Azure hinzugefügt? Handelt es sich in diesem Fall um Azure AD Domain Services oder etwas anderes? 
* Haben wird das Team bzw. Anbieter festgelegt, die das Ziel des Proof of Concept sein werden? Wer werden die Kunden für die Umgebung sein?
* Welche Azure-Region wird für den Proof of Concept verwendet? 
* Gibt es eine Liste von Diensten, die Anbieter neben IaaS (VMs) über DevTest Labs verwenden dürfen? 
* Haben wir einen Plan für die Schulung von Anbietern/Benutzern zur Verwendung des Labs? 

### <a name="proof-of-concept-solution-components"></a>Proof of Concept-Lösungskomponenten 

Wir erwarten, dass die Lösung die folgenden Komponenten umfasst: 

* Eine Reihe von DevTest Labs in Azure für verschiedene Anbieterteams.
* Die Labs sind mit einer Netzwerkinfrastruktur verbunden, die die Anforderungen unterstützt.
* Die Anbieter haben über Azure AD Zugriff auf die Labs und erteilen dem Lab Rollenzuweisungen.
* Eine Möglichkeit für Anbieter, erfolgreich eine Verbindung mit ihren Ressourcen herzustellen. Insbesondere ein Site-to-Site-VPN, das den direkten Zugriff auf virtuelle Computer ohne öffentliche IP-Adressen ermöglicht.
* Eine Reihe von Artefakten, die die erforderliche Software abdecken, die von den Anbietern auf den virtuellen Computern benötigt wird.

## <a name="additional-planning-and-design-decisions"></a>Weitere Planungs- und Entwurfsentscheidungen 

Vor der Veröffentlichung einer vollständigen DevTest Labs-Lösung müssen einige wichtige Planungs- und Entwurfsentscheidungen getroffen werden. Die Erfahrungen, die beim Arbeiten an einem Proof of Concept gesammelt werden, können Ihnen bei diesen Entscheidungen helfen. Weitere Überlegungen umfassen Folgendes: 

* **Abonnementtopologie**: Die Anforderungen an Ressourcen in Azure auf Unternehmensebene können die [verfügbaren Kontingente innerhalb eines einzelnen Abonnements](https://docs.microsoft.com/azure/azure-subscription-service-limits) übersteigen, wodurch mehrere Azure-Abonnements und/oder Dienstanforderungen erforderlich sind, um die anfänglichen Abonnementgrenzwerte zu erhöhen. Es ist wichtig, im Voraus zu entscheiden, wie Ressourcen auf Abonnements verteilt werden sollen. Eine wertvolle Ressource ist der [Leitfaden zur Entscheidungsfindung für Abonnements](https://docs.microsoft.com/azure/architecture/cloud-adoption/decision-guides/subscriptions/), da es schwierig ist, Ressourcen später in ein anderes Abonnement zu verschieben. Beispielsweise kann ein DevTest Lab nicht in ein anderes Abonnement verschoben werden, nachdem es erstellt wurde.  
* **Netzwerktopologie**: Die [Standardnetzwerkinfrastruktur](../app-service/networking-features.md), die automatisch von DevTest Labs erstellt wird, reicht möglicherweise nicht aus, um die Anforderungen und Einschränkungen für die Unternehmensbenutzer zu erfüllen. Es ist üblich, dass [über ExpressRoute verbundene VNets](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/), [Hub-and-Spoke](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) für Konnektivität zwischen Abonnements und sogar [erzwungenes Routing](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) zur Gewährleistung ausschließlich lokaler Konnektivität verwendet werden. Mit DevTest Labs können vorhandene virtuelle Netzwerke mit dem Lab verbunden werden, um deren Verwendung beim Erstellen neuer virtueller Computer im Lab zu ermöglichen. 
* **Remotezugriff auf virtuelle Computer**: Es gibt viele Möglichkeiten für den Remotezugriff auf virtuelle Computer in DevTest Labs. Am einfachsten ist es, öffentliche IP-Adressen oder freigegebene öffentliche IP-Adressen zu verwenden. Dies sind [im Lab verfügbare Einstellungen](devtest-lab-shared-ip.md). Wenn diese Optionen nicht ausreichen, stellt auch die Verwendung eines Remotezugriffgateways eine Möglichkeit dar, wie es unter [Azure DevTest Labs: Referenzarchitektur für Unternehmen](devtest-lab-reference-architecture.md) gezeigt und in der [Dokumentation zu Remotedesktopgateways in DevTest Labs](configure-lab-remote-desktop-gateway.md) ausführlicher beschrieben ist. Unternehmen können auch ExpressRoute oder ein Site-to-Site-VPN verwenden, um DevTest Labs mit dem lokalen Netzwerk zu verbinden. Dies ermöglicht direkte Verbindungen mit den virtuellen Computern über Remotedesktop oder SSH basierend auf der privaten IP-Adresse ohne Zugriff über das Internet. 
* **Handhabung von Berechtigungen**: Die beiden wichtigsten Berechtigungen, die häufig in DevTest Labs verwendet werden, sind [„Besitzer“ und „Lab-Benutzer“](devtest-lab-add-devtest-user.md). Es ist wichtig, sich vor dem umfassenden Rollout von DevTest Labs zu entscheiden, wem die einzelnen Zugriffsebenen im Lab anvertraut werden. Bei einem gängigen Modell ist der Budgetbesitzer (z.B. der Teamleiter) der Lab-Besitzer, und die Teammitglieder sind die Lab-Benutzer. Dadurch kann die Person (Teamleiter), die für das Budget verantwortlich ist, die Richtlinieneinstellungen entsprechend anpassen, um das Team innerhalb des Budgets zu halten.  

## <a name="completing-the-proof-of-concept"></a>Abschließen des Proof of Concept 

Sobald die erwarteten Erkenntnisse vorliegen, ist es an der Zeit, das Pilotprojekt zusammenzufassen abzuschließen. Dies ist der Zeitpunkt, um Feedback von den Benutzern zu sammeln, festzustellen, ob das Pilotprojekt erfolgreich war, und zu entscheiden, ob die Organisation mit einem umfassenden Rollout von DevTest Labs im Unternehmen fortfahren soll. Es ist auch ein guter Zeitpunkt, eine automatisierte Bereitstellung von DevTest Labs und zugeordneten Ressourcen in Betracht zu ziehen, um Konsistenz während des gesamten Rollouts sicherzustellen. 

## <a name="next-steps"></a>Nächste Schritte 

* [Dokumentation zu DevTest Labs im Unternehmen](devtest-lab-guidance-prescriptive-adoption.md)
* [Referenzarchitektur für ein Unternehmen](devtest-lab-reference-architecture.md)
* [Zentrales Hochskalieren der DevTest Labs-Bereitstellung](devtest-lab-guidance-orchestrate-implementation.md)
* [Orchestrieren der Implementierung von Azure DevTest Labs](devtest-lab-guidance-orchestrate-implementation.md)
