---
title: Verwenden von Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte verwenden.
services: active-directory
author: cawrites
manager: daveba
ms.assetid: 4066725c-c430-42b8-a75b-fe2360699b82
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.openlocfilehash: 288fa54a1a6dd4eb05f953a4490bf7736d6d7ff8
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70931253"
---
# <a name="how-to-use-azure-monitor-workbooks-for-azure-active-directory-reports"></a>Verwenden von Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte

Möchten Sie:

- Die Auswirkung Ihrer [Richtlinien für bedingten Zugriff](../conditional-access/overview.md) auf die Anmeldung Ihrer Benutzer verstehen?

- Anmeldefehler beheben, um eine bessere Übersicht über die Anmeldeintegrität in Ihrer Organisation zu erhalten, sowie Probleme schnell beheben?

- Wissen, welche Benutzer sich über Legacyauthentifizierungen bei Ihrer Umgebung anmelden? (Durch [Blockieren der Legacyauthentifizierung](../conditional-access/block-legacy-authentication.md) können Sie den Schutz Ihres Mandanten verbessern.)

Damit Sie diese Fragen beantworten können, stellt Active Directory Arbeitsmappen für die Überwachung bereit. [Azure Monitor-Arbeitsmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) kombinieren Text, Analyseabfragen, Metriken und Parameter zu umfassenden interaktiven Berichten. 

Dieser Artikel:

- Setzt voraus, dass Sie mit dem [Erstellen interaktiver Berichte mit Monitor-Arbeitsmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) vertraut sind.

- Erläutert die Verwendung von Monitor-Arbeitsmappen, um die Auswirkungen Ihrer Richtlinien für bedingten Zugriff verstehen, Anmeldefehler beheben und Legacyauthentifizierungen identifizieren zu können.
 


## <a name="prerequisites"></a>Voraussetzungen

Zum Verwenden von Monitor-Arbeitsmappen benötigen Sie:

- Einen Azure Active Directory-Mandanten mit einer Premium-Lizenz (P1 oder P2). Wie Sie eine Premium-Lizenz erhalten, erfahren Sie [hier](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-get-started-premium).

- Einen [Log Analytics-Arbeitsbereich](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

## <a name="roles"></a>Rollen
Sie müssen Mitglied einer der folgenden Rollen sein und über [Zugriff auf den zugrunde liegenden Log Analytics-Arbeitsbereich](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-access#manage-access-using-azure-permissions) verfügen, um die Arbeitsmappen verwalten zu können:
-   Globaler Administrator
-   Sicherheitsadministrator
-   Sicherheitsleseberechtigter
-   Report Reader (Leseberechtigter für Berichte)
-   Anwendungsadministrator


## <a name="workbook-access"></a>Arbeitsmappenzugriff 

So greifen Sie auf Arbeitsmappen zu

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie im linken Navigationsmenü auf **Azure Active Directory**.

3. Wählen Sie im Abschnitt **Überwachung** die Option **Arbeitsmappen** aus. 

    ![„Einblicke“ auswählen](./media/howto-use-azure-monitor-workbooks/41.png)

4. Wählen Sie einen Bericht oder eine Vorlage aus, oder wählen Sie auf der Symbolleiste **Öffnen** aus. 

    ![„Öffnen“ auswählen](./media/howto-use-azure-monitor-workbooks/42.png)


## <a name="sign-in-analysis"></a>Analyse der Anmeldungen

Wählen Sie für den Zugriff auf die Arbeitsmappe zur Analyse der Anmeldungen im Abschnitt **Nutzung** die Option **Anmeldungen** aus. 

Mit dieser Arbeitsmappe werden die folgenden Trends für Anmeldungen angezeigt:

- Alle Anmeldungen

- Erfolgreich

- Benutzeraktion ausstehend

- Fehler

Sie können jeden Trend nach den folgenden Kategorien filtern:

- Zeitbereich

- Apps

- Benutzer

![Analyse der Anmeldungen](./media/howto-use-azure-monitor-workbooks/43.png)


Für jeden Trend erhalten Sie eine Aufschlüsselung nach folgenden Kategorien:

- Location

    ![Anmeldungen nach Ort](./media/howto-use-azure-monitor-workbooks/45.png)

- Gerät

    ![Anmeldungen nach Gerät](./media/howto-use-azure-monitor-workbooks/46.png)


## <a name="sign-ins-using-legacy-authentication"></a>Anmeldungen mit Legacyauthentifizierung 


Wählen Sie für den Zugriff auf die Arbeitsmappe für Anmeldungen mit [Legacyauthentifizierung](../conditional-access/block-legacy-authentication.md) im Abschnitt **Nutzung** die Option **Anmeldungen mit Legacyauthentifizierung** aus. 

Mit dieser Arbeitsmappe werden die folgenden Trends für Anmeldungen angezeigt:

- Alle Anmeldungen

- Erfolgreich


Sie können jeden Trend nach den folgenden Kategorien filtern:

- Zeitbereich

- Apps

- Benutzer

- Protokolle

![Anmeldungen mit Legacyauthentifizierung](./media/howto-use-azure-monitor-workbooks/47.png)


Für jeden Trend erhalten Sie eine Aufschlüsselung nach App und Protokoll.

![Anmeldungen mit Legacyauthentifizierung nach App und Protokoll](./media/howto-use-azure-monitor-workbooks/48.png)



## <a name="sign-ins-by-conditional-access"></a>Anmeldungen mithilfe des bedingten Zugriffs 


Wählen Sie für den Zugriff auf die Arbeitsmappe für Anmeldungen nach [Richtlinien für bedingten Zugriff](../conditional-access/overview.md) im Abschnitt **Bedingter Zugriff** die Option **Anmeldungen mithilfe des bedingten Zugriffs** aus. 

Mit dieser Arbeitsmappe werden die Trends für deaktivierte Anmeldungen angezeigt. Sie können jeden Trend nach den folgenden Kategorien filtern:

- Zeitbereich

- Apps

- Benutzer

![Anmeldungen mithilfe des bedingten Zugriffs](./media/howto-use-azure-monitor-workbooks/49.png)


Für deaktivierte Anmeldungen erhalten Sie eine Aufschlüsselung nach dem Status des bedingten Zugriffs.

![Status des bedingten Zugriffs](./media/howto-use-azure-monitor-workbooks/conditional-access-status.png)








## <a name="sign-ins-by-grant-controls"></a>Anmeldungen nach Steuerelementen zur Rechteerteilung

Wählen Sie für den Zugriff auf die Arbeitsmappe für Anmeldungen nach [Steuerelementen zur Rechteerteilung](../conditional-access/controls.md) im Abschnitt **Bedingter Zugriff** die Option **Anmeldungen nach Steuerelementen zur Rechteerteilung** aus. 

Mit dieser Arbeitsmappe werden die folgenden Trends für deaktivierte Anmeldungen angezeigt:

- Anfordern von MFA
 
- Vorschreiben der Verwendung von Nutzungsbedingungen

- Vorschreiben von Datenschutzbestimmungen

- Andere


Sie können jeden Trend nach den folgenden Kategorien filtern:

- Zeitbereich

- Apps

- Benutzer

![Anmeldungen nach Steuerelementen zur Rechteerteilung](./media/howto-use-azure-monitor-workbooks/50.png)


Für jeden Trend erhalten Sie eine Aufschlüsselung nach App und Protokoll.

![Aufschlüsselung der aktuellen Anmeldungen](./media/howto-use-azure-monitor-workbooks/51.png)




## <a name="sign-ins-failure-analysis"></a>Analyse der Anmeldungsfehler

Verwenden Sie die Arbeitsmappe zur **Analyse der Anmeldungsfehler**, um Fehler in folgenden Bereichen zu beheben:

- Anmeldungen
- Richtlinien für bedingten Zugriff
- Legacyauthentifizierung 


Wählen Sie für den Zugriff auf die Arbeitsmappe für Anmeldungen nach Daten zum bedingten Zugriff im Abschnitt **Problembehandlung** die Option **Anmeldungen mit Legacyauthentifizierung** aus. 

Mit dieser Arbeitsmappe werden die folgenden Trends für Anmeldungen angezeigt:

- Alle Anmeldungen

- Erfolgreich

- Ausstehende Aktion

- Fehler


Sie können jeden Trend nach den folgenden Kategorien filtern:

- Zeitbereich

- Apps

- Benutzer

![Problembehandlung bei Anmeldungen](./media/howto-use-azure-monitor-workbooks/52.png)


Damit Sie Probleme mit Anmeldungen beheben können, bietet Azure Monitor Ihnen eine Aufschlüsselung nach den folgenden Kategorien:

- Häufigste Fehler

    ![Zusammenfassung der häufigsten Fehler](./media/howto-use-azure-monitor-workbooks/53.png)

- Anmeldungen mit Benutzeraktionen

    ![Zusammenfassung der Anmeldungen mit Benutzeraktionen](./media/howto-use-azure-monitor-workbooks/54.png)






## <a name="next-steps"></a>Nächste Schritte

[Erstellen interaktiver Berichte mit Monitor-Arbeitsmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).
