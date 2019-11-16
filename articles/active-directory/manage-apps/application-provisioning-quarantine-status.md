---
title: Anwendungsbereitstellungsstatus „Quarantäne“ | Microsoft-Dokumentation
description: Wenn Sie eine Anwendung für die automatische Benutzerbereitstellung konfiguriert haben, lesen Sie diesen Artikel, um zu erfahren, was der Bereitstellungsstatus „Quarantäne“ bedeutet und wie Sie ihn löschen können.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/03/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: e3ad689fb57c51d0deb698a723b93e6175bdbb5c
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73882892"
---
# <a name="application-provisioning-in-quarantine-status"></a>Anwendungsbereitstellung im Quarantänestatus

Der Azure AD-Bereitstellungsdienst überwacht die Integrität Ihrer Konfiguration und versetzt fehlerhafte Apps in einen „Quarantänestatus“. Wenn die meisten oder alle Aufrufe an das Zielsystem aufgrund eines Fehlers (z. B. bei ungültigen Administratoranmeldeinformationen) dauerhaft nicht erfolgreich sind, wird der Bereitstellungsauftrag in den Quarantänestatus versetzt.

In der Quarantäne wird die Häufigkeit der inkrementellen Zyklen allmählich auf einmal pro Tag reduziert. Die Quarantäne für den Bereitstellungsauftrag wird aufgehoben, nachdem alle Fehler behoben wurden, und der nächste Synchronisierungszyklus wird gestartet. Wenn der Bereitstellungsauftrag länger als vier Wochen in Quarantäne verbleibt, wird er deaktiviert (nicht mehr ausgeführt).

## <a name="how-do-i-know-if-my-application-is-in-quarantine"></a>Wie kann ich prüfen, ob sich meine Anwendung in Quarantäne befindet?

Es gibt drei Möglichkeiten, um zu überprüfen, ob sich eine Anwendung in Quarantäne befindet:
  
- Navigieren Sie im Azure-Portal zu **Azure Active Directory** > **Unternehmensanwendungen** > &lt;*Anwendungsname*&gt; > **Bereitstellung**, und scrollen Sie nach unten zur Statusanzeige.  

  ![Statusanzeige für die Bereitstellung mit dem Quarantänestatus](media/application-provisioning-quarantine-status/progress-bar-quarantined.png)

- Verwenden Sie die Microsoft Graph-Anforderung [GET synchronizationJob](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-get?view=graph-rest-beta&tabs=http), um den Status des Bereitstellungsauftrags programmgesteuert abzurufen:

        `GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/`

- Prüfen Sie Ihren Posteingang. Wird eine Anwendung unter Quarantäne gestellt, wird eine einmalige Benachrichtigungs-E-Mail gesendet. Wenn sich der Grund für die Quarantäne ändert, wird eine aktualisierte E-Mail mit dem neuen Grund für die Quarantäne gesendet. Wenn keine E-Mail angezeigt wird:

  - Stellen Sie sicher, dass Sie in der Bereitstellungskonfiguration für die Anwendung eine gültige **Benachrichtigungs-E-Mail** angegeben haben.
  - Stellen Sie sicher, dass im Posteingang für Benachrichtigungs-E-Mails keine Spamfilterung aktiviert ist.
  - Stellen Sie sicher, dass Sie keine E-Mails abbestellt haben.

## <a name="why-is-my-application-in-quarantine"></a>Warum befindet sich meine Anwendung in Quarantäne?

Eine Microsoft Graph-Anforderung zum Abrufen des Status des Bereitstellungsauftrags zeigt den folgenden Grund für die Quarantäne an:

- `EncounteredQuarantineException` gibt an, dass ungültige Anmeldeinformationen angegeben wurden. Der Bereitstellungsdienst kann keine Verbindung zwischen dem Quellsystem und dem Zielsystem herstellen.

- `EncounteredEscrowProportionThreshold` gibt an, dass die Bereitstellung den hinterlegten Schwellenwert überschritten hat. Dieser Zustand tritt auf, wenn mehr als 60 % der Bereitstellungsereignisse fehlgeschlagen sind.

- `QuarantineOnDemand` bedeutet, dass wir ein Problem bei Ihrer Anwendung erkannt und sie manuell in den Quarantänestatus versetzt haben.

## <a name="how-do-i-get-my-application-out-of-quarantine"></a>Wie kann ich dafür sorgen, dass meine Anwendung aus der Quarantäne entfernt wird?

Beheben Sie zuerst das Problem, das dazu geführt hat, dass die Anwendung unter Quarantäne gestellt wurde.

- Überprüfen Sie die Bereitstellungseinstellungen der Anwendung, um sicherzustellen, dass Sie [gültige Administratoranmeldeinformationen eingegeben](configure-automatic-user-provisioning-portal.md#configuring-automatic-user-account-provisioning) haben. Azure AD muss eine Vertrauensstellung mit der Zielanwendung herstellen können. Stellen Sie sicher, dass Sie gültige Anmeldeinformationen eingegeben haben und Ihr Konto über die erforderlichen Berechtigungen verfügt.

- Überprüfen Sie die [Bereitstellungsprotokolle](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs), um weiter zu untersuchen, welche Fehler zur Quarantäne geführt haben, und beheben Sie den Fehler. Greifen Sie im Azure-Portal auf die Bereitstellungsprotokolle zu, indem Sie zu **Azure Active Directory** &gt; **Unternehmensanwendungen** &gt; **Bereitstellungsprotokolle (Vorschau)** (im Abschnitt **Aktivität**) navigieren.

Nachdem Sie das Problem behoben haben, können Sie den Bereitstellungsauftrag neu starten. Durch bestimmte Änderungen an den Bereitstellungseinstellungen (wie Attributzuordnungen oder Bereichsfilter) der Anwendung wird die Bereitstellung automatisch neu gestartet. Die Statusanzeige auf der Seite **Bereitstellung** der Anwendung gibt an, wann die Bereitstellung zuletzt gestartet wurde. Wenn Sie den Bereitstellungsauftrag manuell neu starten müssen, verwenden Sie eine der folgenden Methoden:  

- Verwenden Sie das Azure-Portal, um den Bereitstellungsauftrag neu zu starten. Wählen Sie auf der Seite **Bereitstellung** der Anwendung unter **Einstellungen** die Option **Aktuellen Status löschen und Synchronisierung neu starten** aus, und legen Sie **Bereitstellungsstatus** auf **Ein** fest. Durch diese Aktion wird der Bereitstellungsdienst vollständig neu gestartet. Dieser Vorgang kann einige Zeit in Anspruch nehmen. Ein vollständiger Startzyklus wird erneut ausgeführt. Dabei werden Hinterlegungen gelöscht, die App wird aus der Quarantäne entfernt, und alle Wasserzeichen werden gelöscht.

- Verwenden Sie Microsoft Graph, um [den Bereitstellungsauftrag neu zu starten](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-restart?view=graph-rest-beta&tabs=http). Sie haben vollständige Kontrolle über die Elemente, die Sie neu starten. Sie können auswählen, ob Hinterlegungen (zum Neustarten des Hinterlegungszählers, der in Richtung Quarantänestatus läuft) gelöscht, die Quarantäne (zum Entfernen der Anwendung aus der Quarantäne) oder die Wasserzeichen gelöscht werden soll(en). Verwenden Sie die folgende Anforderung:
 
       `POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/restart`