---
title: Informationen zu Bedrohungen und Karte mit Sicherheitswarnungen – Azure Security Center
description: Hier erfahren Sie, wie Sie anhand von Informationen zu Bedrohungen und der Karte mit Sicherheitswarnungen in Azure Security Center potenzielle Bedrohungen auf Ihren virtuellen und physischen Computern identifizieren.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: 97975d72214426907a2ab91f0d3cd98d0ce6734b
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73693771"
---
# <a name="security-alerts-map-and-threat-intelligence"></a>Karte mit Sicherheitswarnungen und Informationen zu Bedrohungen
In diesem Artikel erfahren Sie, wie Sie sicherheitsbezogene Probleme mithilfe der Karte mit Sicherheitswarnungen und der auf Sicherheitsereignissen basierenden Informationen zu Bedrohungen in Azure Security Center lösen.

> [!NOTE]
> Die Kartenschaltfläche *Sicherheitsereignisse* wurde am 31. Juli 2019 eingestellt. Weitere Informationen und alternative Dienste finden Sie unter [Auslaufen von Security Center-Funktionen (Juli 2019)](security-center-features-retirement-july2019.md#menu_securityeventsmap).


## <a name="how-the-security-alerts-map-works"></a>Funktionsweise der Karte mit Sicherheitswarnungen
Security Center stellt Ihnen eine Karte bereit, mit deren Hilfe Sie Sicherheitsrisiken für die Umgebung identifizieren können. So können sie beispielsweise ermitteln, ob ein bestimmter Computer Teil eines Botnets ist und woher die Bedrohung stammt. Computer können zu Knoten in einem Botnet werden, wenn Angreifer illegal Schadsoftware installieren, die heimlich mit Befehls- und Steuerelementen interagiert, die das Botnet verwalten. 

Zum Erstellen dieser Karte werden in Security Center Daten aus mehreren Quellen von Microsoft herangezogen. Security Center nutzt diese Daten zur Darstellung potenzieller Bedrohungen für Ihre Umgebung. 

Einer der Schritte bei der [Reaktion auf Sicherheitsvorfälle](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) ist die Ermittlung des Schweregrads für die betroffenen Systeme. In dieser Phase sollten Sie die folgenden Aufgaben durchführen:

- Bestimmen der Art des Angriffs.
- Bestimmen des Ursprungs des Angriffs.
- Bestimmen der Absicht des Angriffs. Wurde Ihre Organisation gezielt angegriffen, um bestimmte Informationen zu erlangen, oder war es ein ungezielter Angriff?
- Ermitteln der betroffenen Systeme.
- Ermitteln der Dateien, auf die zugegriffen wurde, und Bestimmen der Vertraulichkeit dieser Dateien.

Bei diesen Aufgaben können Sie auf die Karte mit Sicherheitswarnungen in Security Center zurückgreifen.

## <a name="access-the-security-alerts-map"></a>Zugreifen auf die Karte mit Sicherheitswarnungen
Öffnen Sie die Karte mit Sicherheitswarnungen, um die aktuellen Bedrohungen für Ihre Umgebung darzustellen:

1. Öffnen Sie das Dashboard **Security Center**.
2. Wählen Sie im linken Bereich unter **Bedrohungsschutz** die Option **Sicherheitshinweise zuordnen** aus. Die Karte wird geöffnet.
3. Um weitere Informationen zu einer Warnung und Schritte zur Behebung anzuzeigen, klicken Sie auf den Punkt für die Warnung auf der Karte, und folgen Sie den Anweisungen. 
 
Die Karte mit Sicherheitswarnungen basiert auf Warnungen. Diese Warnungen basieren auf Aktivitäten, bei denen der Netzwerkkommunikation eine IP-Adresse zugeordnet wurde, die erfolgreich aufgelöst wurde, und zwar unabhängig davon, ob die IP-Adresse eine bekannte riskante IP-Adresse (z.B. ein bekannter Crypto-Miner ) oder eine IP-Adresse ist, die zuvor nicht als riskant erkannt wurde. Die Karte enthält Warnungen für alle Abonnements, die Sie zuvor in Azure ausgewählt haben. 

Die Warnungen auf der Karte werden entsprechend dem geografischen Standort angezeigt, der als deren Ursprung erkannt wurde, und sind nach Schweregrad farbcodiert. 
    ![Informationen zu Bedrohungen](./media/security-center-threat-intel/security-center-alert-map.png)



## <a name="see-also"></a>Weitere Informationen
In diesem Artikel wurde beschrieben, wie Sie Informationen zu Bedrohungen in Security Center beim Identifizieren von verdächtigen Aktivitäten nutzen. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Hier erfahren Sie, wie Sie Warnungen verwalten und auf Sicherheitsvorfälle in Security Center reagieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md). Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Hier finden Sie Informationen zu den unterschiedlichen Arten von Sicherheitswarnungen.
* [Azure Security Center – Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Hier erfahren Sie, wie Sie allgemeine Probleme in Security Center behandeln.
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md)festgelegt ist. Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](https://blogs.msdn.com/b/azuresecurity/). Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.
