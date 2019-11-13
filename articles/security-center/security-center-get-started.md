---
title: Upgrade auf den Standard-Tarif – Azure Security Center
description: Dieser Schnellstart zeigt Ihnen, wie Sie für zusätzliche Sicherheit ein Upgrade auf den Security Center-Tarif Standard durchführen können.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: memildin
ms.openlocfilehash: 26d62f2c027a093ba518b98fa37ce3a31a14f175
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73664282"
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Schnellstart: Einbinden Ihres Azure-Abonnements in Security Center Standard
Azure Security Center bietet einheitliche Funktionen für die Sicherheitsverwaltung und den Schutz vor Bedrohungen für Ihre Hybrid Cloud-Workloads. Während der Free-Tarif nur eingeschränkte Sicherheit für Ihre Azure-Ressourcen bietet, erweitert der Standard-Tarif diese Funktionen auf lokale Umgebungen und andere Clouds. Security Center Standard hilft Ihnen, Sicherheitsrisiken zu finden und zu beseitigen, Zugriffs- und Anwendungssteuerungen anzuwenden, um böswillige Aktivitäten zu blockieren, Bedrohungen mithilfe von Analysen und intelligenter Funktionen zu erkennen und bei Angriffen schnell zu reagieren. Sie können Security Center Standard kostenlos testen. Weitere Informationen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/security-center/).

In diesem Artikel führen Sie für zusätzliche Sicherheit ein Upgrade auf den Tarif Standard durch und installieren den Microsoft Monitoring Agent auf Ihren virtuellen Computern, um Sicherheitslücken und Bedrohungen ausfindig zu machen.

## <a name="prerequisites"></a>Voraussetzungen
Für den Einstieg in Security Center benötigen Sie ein Microsoft Azure-Abonnement. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

Um ein Abonnement auf den Standard-Tarif zu aktualisieren, muss Ihnen die Rolle „Abonnementbesitzer“, „Mitwirkender“ am Abonnement oder „Sicherheitsadministrator“ zugewiesen werden.

## <a name="enable-your-azure-subscription"></a>Aktivieren Ihres Azure-Abonnements

1. Melden Sie sich beim [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) an.
2. Wählen Sie im Menü **Microsoft Azure** die Option **Security Center**. **Security Center – Übersicht** wird geöffnet.

   ![Übersicht über das Security Center][2]

In der **Security Center – Übersicht** erhalten Sie einen vereinheitlichten Überblick über den Sicherheitsstatus Ihrer Hybrid Cloud-Workloads und können so die Sicherheit Ihrer Workloads ermitteln und bewerten sowie Risiken identifizieren und mindern. Das Security Center aktiviert automatisch alle Ihre Azure-Abonnements, die Sie oder ein anderer Abonnementbenutzer nicht zuvor dem Free-Tarif zugeordnet haben.

Sie können die Liste der Abonnements einsehen und filtern, indem Sie auf den Menüeintrag **Abonnements** klicken. Das Security Center beginnt anschließend mit der Bewertung der Sicherheit dieser Abonnements, um Sicherheitsrisiken auszumachen. Um die Arten von Beurteilungen anzupassen, können Sie die Sicherheitsrichtlinie ändern. Eine Sicherheitsrichtlinie definiert die gewünschte Konfiguration Ihrer Workloads und trägt zur Erfüllung unternehmensbezogener oder gesetzlicher Sicherheitsanforderungen bei.

Innerhalb weniger Minuten nach dem ersten Start von Security Center wird Ihnen Folgendes angezeigt:

- **Empfehlungen** für Möglichkeiten zur Verbesserung der Sicherheit Ihrer Azure-Abonnements. Wenn Sie auf die Kachel **Empfehlungen** klicken, wird eine priorisierte Liste eingeblendet.
- Ein Bestand an Ressourcen vom Typ **Compute und Apps**, **Netzwerk**, **Datensicherheit** und **Identität und Zugriff**, die nun von Security Center zusammen mit dem jeweiligen Sicherheitsstatus bewertet werden.

Um den vollen Nutzen aus Security Center zu ziehen, müssen Sie die folgenden Schritte ausführen, um auf den Standard-Tarif zu aktualisieren und den Microsoft Monitoring Agent zu installieren.

## <a name="upgrade-to-the-standard-tier"></a>Upgrade auf den Standard-Tarif
Für die Zwecke des Schnellstarts und der Tutorials für Security Center müssen Sie ein Upgrade auf den Standard-Tarif durchführen. Von Security Center Standard gibt es eine kostenlose Testversion. Weitere Informationen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/security-center/). 

1. Wählen Sie im Security Center im Hauptmenü die Option **Erste Schritte** aus.
 
   ![Erste Schritte][4]

2. Unter **Upgrade** werden von Security Center für die Integration geeignete Abonnements und Arbeitsbereiche aufgeführt. 
   - Klicken Sie auf den erweiterbaren Bereich **Ihre Testversion anwenden**, um eine Liste aller Abonnements und Arbeitsbereiche mit einem Berechtigungsstatus für Ihre Testversion anzuzeigen.
   -    Sie können Abonnements und Arbeitsbereiche aktualisieren, die keine Berechtigung für die Testversion haben.
   -    Sie können berechtigte Arbeitsbereiche und Abonnements auswählen, um Ihre Testversion zu starten.
3. Klicken Sie auf **Testversion starten**, um Ihre Testversion für die ausgewählten Abonnements zu starten.


  ![Sicherheitswarnungen][9]

## <a name="automate-data-collection"></a>Automatisieren der Datensammlung
Security Center sammelt Daten von Ihren virtuellen Azure-Computern (VMs) und Azure-fremden Computern, um sie auf Sicherheitslücken und Bedrohungen zu überwachen. Die Daten werden mithilfe von Microsoft Monitoring Agent gesammelt. Der Agent liest verschiedene sicherheitsrelevante Konfigurationen und Ereignisprotokolle auf dem Computer und kopiert die Daten zur Analyse in Ihren Arbeitsbereich. Standardmäßig erstellt Security Center einen neuen Arbeitsbereich für Sie.

Bei aktivierter automatischer Bereitstellung wird Microsoft Monitoring Agent von Security Center auf allen unterstützten virtuellen Azure-Computern sowie auf allen neu erstellten virtuellen Computern installiert. Die automatische Bereitstellung wird dringend empfohlen.

So aktivieren Sie die automatische Bereitstellung von Microsoft Monitoring Agent:

1. Klicken Sie im Hauptmenü von „Security Center“ auf **Preise & Einstellungen**.
2. Klicken Sie in der Zeile des Abonnements auf das Abonnement, dessen Einstellungen Sie ändern möchten.
3. Wählen Sie auf der Registerkarte **Datensammlung** für **Automatische Bereitstellung** **Aktivieren** aus.
4. Wählen Sie **Speichern** aus.
---
  ![Aktivieren der automatischen Bereitstellung][6]

Mit diesen neuen Einblicken in Ihre Azure-VMs kann Security Center zusätzliche Empfehlungen in Bezug auf den Status von Systemupdates, Sicherheitskonfigurationen für Betriebssysteme und des Endgeräteschutzes geben sowie zusätzliche Sicherheitswarnungen generieren.

  ![Empfehlungen][8]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Andere Schnellstartanleitungen und Tutorials in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Wenn Sie planen, mit den nachfolgenden Schnellstartanleitungen und Tutorials fortzufahren, sollten Sie weiter den Tarif „Standard“ nutzen und die automatische Bereitstellung aktiviert lassen. Gehen Sie wie folgt vor, falls Sie nicht fortfahren oder zum Free-Tarif zurückkehren möchten:

1. Kehren Sie zum Hauptmenü von Security Center zurück, und wählen Sie die Option **Preise & Einstellungen** aus.
2. Klicken Sie auf das Abonnement, das Sie in den Free-Tarif ändern möchten.
3. Wählen Sie **Tarif** und anschließend **Free**, um für das Abonnement vom Tarif „Standard“ zu „Free“ zu wechseln.
5. Wählen Sie **Speichern** aus.

Gehen Sie wie folgt vor, um die automatische Bereitstellung zu deaktivieren:

1. Kehren Sie zum Hauptmenü von Security Center zurück, und wählen Sie die Option **Preise & Einstellungen** aus.
2. Klicken Sie auf das Abonnement, für das Sie die automatische Bereitstellung deaktivieren möchten.
3. Wählen Sie auf der Registerkarte **Datensammlung** für **Automatische Bereitstellung** **Deaktivieren** aus.
4. Wählen Sie **Speichern** aus.

>[!NOTE]
> Wenn Sie die automatische Bereitstellung deaktivieren, wird Microsoft Monitoring Agent nicht von virtuellen Azure-Computern entfernt, auf denen der Agent bereitgestellt wurde. Wenn Sie die automatische Bereitstellung deaktivieren, schränkt dies die Sicherheitsüberwachung für Ihre Ressourcen ein.
>

## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart haben Sie ein Upgrade auf den Standard-Tarif durchgeführt und den Microsoft Monitoring Agent für ein einheitliches Sicherheitsmanagement und Bedrohungsschutz für Ihre Hybrid Cloud-Workloads bereitgestellt. Wenn Sie mehr darüber erfahren möchten, wie Sie Security Center verwenden können, fahren Sie mit dem Schnellstart für das Einbinden von Windows-Computern fort, die lokal und in anderen Clouds installiert sind.

> [!div class="nextstepaction"]
> [Schnellstart: Einbinden von Windows-Computern in Azure Security Center](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/get-started.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
[9]: ./media/security-center-get-started/select-subscription.png
