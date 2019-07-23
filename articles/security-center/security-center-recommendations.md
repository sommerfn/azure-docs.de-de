---
title: Sicherheitsempfehlungen in Azure Security Center | Microsoft-Dokumentation
description: In diesem Dokument erfahren Sie, wie Sicherheitsempfehlungen in Azure Security Center Ihnen helfen, Ihre Azure-Ressourcen zu schützen und Ihre Sicherheitsrichtlinien einzuhalten.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2019
ms.author: v-mohabe
ms.openlocfilehash: fe1d4bf27f3c4bb1f70c1c1fa9767c27f8767998
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064225"
---
# <a name="security-recommendations-in-azure-security-center"></a>Sicherheitsempfehlungen in Azure Security Center 
In diesem Thema wird erläutert, wie Sie die Empfehlungen in Azure Security Center anzeigen und nutzen können, mit denen Sie Ihre Azure-Ressourcen schützen.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Dieses Dokument ist keine Schritt-für-Schritt-Anleitung.
>

## <a name="what-are-security-recommendations"></a>Was sind Sicherheitsempfehlungen?
Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen. Wenn Security Center potenzielle Sicherheitsrisiken identifiziert, werden Empfehlungen erstellt. Entsprechend den Empfehlungen werden Sie durch den Prozess der Konfiguration des erforderlichen Sicherheitsmechanismus geführt.

## <a name="implementing-security-recommendations"></a>Implementieren von Sicherheitsempfehlungen
### <a name="set-recommendations"></a>Festlegen von Empfehlungen
Unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](tutorial-security-policy.md)wird Folgendes beschrieben:

* Konfigurieren von Sicherheitsrichtlinien
* Aktivieren der Datensammlung
* Auswählen der Empfehlungen, die Sie als Teil Ihrer Sicherheitsrichtlinie verwenden möchten

Aktuelle Richtlinienempfehlungen beziehen sich auf Systemupdates, Grundregeln, Antischadsoftware, [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md) für Subnetze und Netzwerkschnittstellen, Überwachung der SQL-Datenbank, Transparent Data Encryption für die SQL-Datenbank und Web Application Firewalls.  [Einrichten von Sicherheitsrichtlinien](tutorial-security-policy.md) enthält eine Beschreibung der einzelnen Empfehlungsoptionen.

### <a name="monitor-recommendations"></a>Überwachen von Empfehlungen
Nach Einstellung einer Sicherheitsrichtlinie analysiert Security Center den Sicherheitsstatus Ihrer Ressourcen, um potenzielle Sicherheitsrisiken zu erkennen. Auf der Kachel **Empfehlungen** unter **Übersicht** wird die Gesamtzahl der von Security Center identifizierten Empfehlungen angezeigt.

![Übersicht über Security Center](./media/security-center-recommendations/asc-overview.png)

1. Wählen Sie die Kachel **Empfehlungen** unter **Übersicht** aus. Die Liste **Empfehlungen** wird geöffnet.
    
      ![Anzeigen von Empfehlungen](./media/security-center-recommendations/view-recommendations.png)

    Sie können Empfehlungen filtern. Wählen Sie auf dem Blatt **Empfehlungen** die Option **Filter**, um die Empfehlungen zu filtern. Das Blatt **Filter** wird geöffnet. Sie können Werte für Schweregrad und Status auswählen, die Sie anzeigen möchten.

   * **EMPFEHLUNGEN:** Die Empfehlung
   * **SECURE SCORE-AUSWIRKUNG:** Eine Bewertung, die von Security Center anhand Ihrer Sicherheitsempfehlungen generiert wurde, samt Anwendung von komplexen Algorithmen, mit denen bestimmt wird, wie wichtig jede Empfehlung ist. Weitere Informationen finden Sie unter [Secure Score-Berechnung](security-center-secure-score.md#secure-score-calculation).
   * **RESSOURCE**: Eine Liste mit den Ressourcen, für die diese Empfehlung gilt.
   * **STATUSLEISTEN:**  Beschreibt den Schweregrad der jeweiligen Empfehlung:
       * **Hoch (rot):** Ein Sicherheitsrisiko betrifft eine wichtige Ressource (z. B. eine Anwendung, eine VM oder eine Netzwerksicherheitsgruppe) und erfordert einen Eingriff.
       * **Mittel (orange):** Es besteht ein Sicherheitsrisiko, und es sind nicht kritische oder zusätzliche Schritte erforderlich, um es zu beseitigen oder einen Prozess abzuschließen.
       * **Niedrig (blau):** Es besteht ein Sicherheitsrisiko, das behandelt werden sollte, aber keinen unmittelbaren Eingriff erfordert. (Standardmäßig werden Empfehlungen mit dem Status „Niedrig“ nicht angezeigt, aber Sie können bei Bedarf nach diesen Empfehlungen filtern.) 
       * **Fehlerfrei (grün):**
       * **Nicht verfügbar (grau):**

1. Um die Details einer Empfehlung anzuzeigen, klicken Sie auf die Empfehlung.

    ![Empfehlungsdetails](./media/security-center-recommendations/recommendation-details.png)

>[!NOTE] 
> Informationen zu Azure-Ressourcen finden Sie in der Beschreibung für das [klassische und das Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md).
  
 ### <a name="apply-recommendations"></a>Anwenden von Empfehlungen
> Nach Auswertung aller Empfehlungen müssen Sie entscheiden, welche zuerst angewendet werden soll. Es ist ratsam, anhand der Secure Score-Auswirkung zu bestimmen, welche Empfehlungen zuerst angewendet werden sollen.

1. Klicken Sie in der Liste auf die gewünschte Empfehlung.
1. Befolgen Sie die Anweisungen im Abschnitt *Schritte zur Bereinigung*.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurden Ihnen die Sicherheitsempfehlungen in Security Center vorgestellt. Weitere Informationen zu Security Center finden Sie in den folgenden Themen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.
