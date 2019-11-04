---
title: Sicherheitsempfehlungen in Azure Security Center | Microsoft-Dokumentation
description: In diesem Dokument erfahren Sie, wie Sicherheitsempfehlungen in Azure Security Center Ihnen helfen, Ihre Azure-Ressourcen zu schützen und Ihre Sicherheitsrichtlinien einzuhalten.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/29/2019
ms.author: memildin
ms.openlocfilehash: 6a01ddbc733ce52c02e9abef2569adbfbd19c6a8
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73520736"
---
# <a name="security-recommendations-in-azure-security-center"></a>Sicherheitsempfehlungen in Azure Security Center 
In diesem Thema wird erläutert, wie Sie die Empfehlungen in Azure Security Center anzeigen und nutzen können, mit denen Sie Ihre Azure-Ressourcen schützen.

> [!NOTE]
> Der Dienst wird anhand einer Beispielbereitstellung vorgestellt.  Dieses Dokument ist keine Schritt-für-Schritt-Anleitung.
>

## <a name="what-are-security-recommendations"></a>Was sind Sicherheitsempfehlungen?

Empfehlungen sind Maßnahmen, die Sie ergreifen sollten, um Ihre Ressourcen zu schützen.

Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen, um mögliche Sicherheitsrisiken zu identifizieren. Anschließend erhalten Sie Empfehlungen dazu, wie sie behoben werden können.

Jede Empfehlung beinhaltet Folgendes:

- Eine kurze Beschreibung der empfohlenen Aktion
- Die Schritte zur Bereinigung, die zum Implementieren der Empfehlung ausgeführt werden müssen <!-- In some cases, Quick Fix remediation is available. -->
- Angaben zu den Ressourcen, für die die empfohlene Aktion ausgeführt werden sollte
- Die **Secure Score-Auswirkung**, d. h. die Punkte, um die Ihre Sicherheitsbewertung erhöht wird, wenn Sie die Empfehlung implementieren

## Überwachen von Empfehlungen<a name="monitor-recommendations"></a>

Security Center analysiert den Sicherheitsstatus Ihrer Ressourcen, um mögliche Sicherheitsrisiken zu identifizieren. Auf der Kachel **Empfehlungen** unter **Übersicht** wird die Gesamtzahl der von Security Center identifizierten Empfehlungen angezeigt.

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
 
## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurden Ihnen die Sicherheitsempfehlungen in Security Center vorgestellt. So erfahren Sie, wie Sie die Empfehlungen umsetzen

* [Umsetzen von Empfehlungen:](security-center-remediate-recommendations.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
