---
title: Konfigurieren der Richtlinie zum Benutzerrisiko in Azure Active Directory Identity Protection | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Azure AD Identity Protection-Richtlinie zum Benutzerrisiko konfigurieren.
services: active-directory
keywords: Azure Active Directory Identity Protection, Cloud App Discovery, Verwalten von Anwendungen, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 942f7e43a549b5aa1a21284949ffc12ef3c8d75f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108928"
---
# <a name="how-to-configure-the-user-risk-policy"></a>Anleitung: Konfigurieren der Richtlinie zum Benutzerrisiko

Anhand des Benutzerrisikos erkennt Azure AD die Wahrscheinlichkeit, mit der ein Benutzerkonto kompromittiert wurde. Als Administrator können Sie für das Benutzerrisiko eine Richtlinie für bedingten Zugriff konfigurieren, um automatisch auf eine bestimmte Benutzerrisikostufe zu reagieren.
 
In diesem Artikel erhalten Sie die erforderlichen Informationen, die Sie zum Konfigurieren einer Richtlinie für das Benutzerrisiko benötigen.


## <a name="what-is-a-user-risk-policy"></a>Was ist eine Richtlinie zum Benutzerrisiko?

Azure AD analysiert jede Anmeldung eines Benutzers. Das Ziel der Analyse besteht darin, verdächtige Aktionen zu erkennen, die mit der Anmeldung verbunden sind. In Azure AD werden die verdächtigen Aktionen, die vom System erkannt werden können, auch als Risikoereignisse bezeichnet. Einige Risikoereignisse können in Echtzeit erkannt werden, aber es gibt auch Risikoereignisse, für die mehr Zeit benötigt wird. Um beispielsweise eine Fahrt zu ungewöhnlichen Orten zu erkennen, benötigt das System einen anfänglichen Lernzeitraum von 14 Tagen, um das Regelverhalten eines Benutzers kennenzulernen. Es gibt mehrere Optionen zum Lösen von erkannten Risikoereignissen. Beispielsweise können Sie einzelne Risikoereignisse manuell lösen oder lösen lassen, indem Sie eine Richtlinie für bedingten Zugriff für das Anmelderisiko oder Benutzerrisiko verwenden.

Alle Risikoereignisse, die für einen Benutzer erkannt und nicht gelöst wurden, werden als aktive Risikoereignisse bezeichnet. Die aktiven Risikoereignisse, die einem Benutzer zugeordnet sind, werden als Benutzerrisiko bezeichnet. Basierend auf dem Benutzerrisiko berechnet Azure AD eine Wahrscheinlichkeit (niedrig, mittel, hoch), mit der ein Benutzer kompromittiert wurde. Die Wahrscheinlichkeit wird als Benutzerrisikostufe bezeichnet.

![Benutzerrisiken](./media/howto-user-risk-policy/1031.png)

Die Richtlinie zum Benutzerrisiko ist eine automatisierte Antwort, die Sie für eine bestimmte Benutzerrisikostufe konfigurieren können. Mit einer Richtlinie zum Benutzerrisiko können Sie den Zugriff auf Ihre Ressourcen blockieren oder eine Kennwortänderung erzwingen, um für ein Benutzerkonto den bereinigten Zustand wiederherzustellen.


## <a name="how-do-i-access-the-user-risk-policy"></a>Wie greife ich auf die Richtlinie zum Benutzerrisiko zu?
   
Die Richtlinie zum Anmelderisiko befindet sich auf der [Azure AD Identity Protection-Seite](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy) im Abschnitt **Konfigurieren**.
   
![Richtlinie zum Benutzerrisiko](./media/howto-user-risk-policy/1014.png)



## <a name="policy-settings"></a>Richtlinieneinstellungen

Sie müssen Folgendes festlegen, wenn Sie die Richtlinie zum Anmelderisiko konfigurieren:

- Die Benutzer und Gruppen, für die die Richtlinie gilt:

    ![Benutzer und Gruppen](./media/howto-user-risk-policy/11.png)

- Die Risikostufe der Anmeldung, die die Richtlinie auslöst:

    ![Benutzerrisikostufe](./media/howto-user-risk-policy/12.png)

- Die Art des Zugriffs, die erzwungen werden soll, wenn Ihre Risikostufe der Anmeldung erfüllt wurde:  

    ![Access](./media/howto-user-risk-policy/13.png)

- Zustand Ihrer Richtlinie:

    ![Erzwingen der Richtlinie](./media/howto-user-risk-policy/14.png)

Das Dialogfeld „Richtlinienkonfiguration“ enthält eine Option zum Schätzen der Auswirkungen, die sich aus Ihrer Konfiguration ergeben.

![Voraussichtliche Auswirkung](./media/howto-user-risk-policy/15.png)

## <a name="what-you-should-know"></a>Wichtige Informationen

Sie können eine Sicherheitsrichtlinie für das Benutzerrisiko festlegen, um Benutzer je nach Risikostufe bei der Anmeldung zu blockieren.

![Blockieren](./media/howto-user-risk-policy/16.png)


Das Sperren einer Anmeldung bewirkt Folgendes:

* Verhindert die Generierung neuer Benutzerrisikoereignisse für den betroffenen Benutzer
* Ermöglicht Administratoren die manuelle Bereinigung der Risikoereignisse, die die Identität des Benutzers beeinträchtigen, und stellen wieder einen sicheren Identitätszustand her

## <a name="best-practices"></a>Bewährte Methoden

Wenn Sie den Schwellenwert **Hoch** auswählen, wird die Richtlinie weniger häufig ausgelöst, und die Auswirkungen für Benutzer verringern sich.
Benutzer mit der Einstufung **Niedrig** und **Mittel**, die als risikobehaftet gekennzeichnet sind, werden aber aus der Richtlinie ausgeschlossen. Dies trägt unter Umständen nicht zum Schutz von Identitäten oder Geräten bei, die zuvor vermutlich oder mit Sicherheit kompromittiert waren.

Gehen Sie beim Festlegen der Richtlinie wie folgt vor:

* Schließen Sie die Benutzer aus, die voraussichtlich viele falsch positive Ergebnisse generieren (Entwickler, Sicherheitsanalysten).
* Schließen Sie die Benutzer nach Gebietsschemas aus, für die die Aktivierung der Richtlinie keinen praktischen Nutzen hat (z. B. kein Zugriff auf den Helpdesk).
* Verwenden Sie während des anfänglichen Rollouts der Richtlinie (oder wenn Sie die Auswirkungen für Endbenutzer verringern möchten) den Schwellenwert **Hoch** .
* Verwenden Sie den Schwellenwert **Niedrig** , falls für Ihre Organisation eine höhere Sicherheitsebene erforderlich ist. Wenn Sie für den Schwellenwert **Niedrig** auswählen, erhöht sich der Aufwand für die Benutzer bei der Anmeldung, aber die Sicherheit wird erhöht.

Die empfohlene Standardvorgehensweise für die meisten Organisationen ist die Konfiguration einer Regel für den Schwellenwert **Mittel** , um eine gute Balance zwischen Benutzerfreundlichkeit und Sicherheit zu erzielen.

Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

* [Wiederherstellung für kompromittierte Konten](flows.md#compromised-account-recovery)  
* [Blockierter Ablauf bei kompromittierten Konten](flows.md#compromised-account-blocked)  

**Um den entsprechenden Konfigurationsdialog zu öffnen**:

- Klicken Sie auf dem Blatt **Azure AD Identity Protection** im Abschnitt **Konfigurieren** auf **Richtlinie zum Benutzerrisiko**.

    ![Richtlinie zum Benutzerrisiko](./media/howto-user-risk-policy/1009.png "Richtlinie zum Benutzerrisiko")




## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über Azure AD Identity Protection finden Sie [in diesem Artikel](overview.md).
