---
title: Anforderungsprozess und E-Mail-Benachrichtigungen in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Hier finden Sie Informationen zum Anforderungsprozess für ein Zugriffspaket in der Azure Active Directory-Berechtigungsverwaltung und darüber, wann entsprechende E-Mail-Benachrichtigungen gesendet werden.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/30/2019
ms.author: ajburnle
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: e34d2c69cc808552a3b0c604804f3cd2597b379b
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73199952"
---
# <a name="request-process-and-email-notifications-in-azure-ad-entitlement-management"></a>Anforderungsprozess und E-Mail-Benachrichtigungen in der Azure AD-Berechtigungsverwaltung

Wenn ein Benutzer eine Anforderung für ein Zugriffspaket sendet, wird ein Prozess gestartet, um diese Zugriffsanforderung zu übermitteln. Die Azure AD-Berechtigungsverwaltung sendet E-Mail-Benachrichtigungen an genehmigende Personen und Anforderer, wenn wichtige Ereignisse während des Prozesses auftreten. In diesem Artikel finden Sie eine Beschreibung des Anforderungsprozesses und der E-Mail-Benachrichtigungen, die gesendet werden.

## <a name="request-process"></a>Anforderungsprozess

Ein Benutzer, der Zugriff auf ein Zugriffspaket benötigt, kann eine Zugriffsanforderung senden. Abhängig von der Konfiguration der Richtlinie muss die Anforderung möglicherweise genehmigt werden. Wenn eine Anforderung genehmigt wird, startet ein Prozess, um dem Benutzer Zugriff auf jede Ressource im Zugriffspaket zuzuweisen. Das folgende Diagramm zeigt eine Übersicht über den Prozess und die unterschiedlichen Statuswerte:

![Diagramm des Genehmigungsprozesses](./media/entitlement-management-process/request-process.png)

| State | BESCHREIBUNG |
| --- | --- |
| Gesendet | Ein Benutzer sendet eine Anforderung. |
| Ausstehende Genehmigung | Wenn die Richtlinie für ein Zugriffspaket eine Genehmigung erfordert, ändert sich der Status der Anforderung in „Genehmigung ausstehend“. |
| Abgelaufen | Wenn keine der genehmigenden Personen die Anforderung innerhalb des Zeitlimits für die Genehmigungsanforderung genehmigt, läuft die Anforderung ab. In dem Fall muss der Benutzer seine Anforderung erneut senden. |
| Verweigert | Eine genehmigende Person lehnt eine Anforderung ab. |
| Genehmigt | Eine genehmigende Person genehmigt eine Anforderung. |
| Übermitteln | Dem Benutzer wurde **noch kein** Zugriff auf alle Ressourcen im Zugriffspaket zugewiesen. Wenn es sich um einen externen Benutzer handelt, hat der Benutzer möglicherweise noch nicht auf das Ressourcenverzeichnis zugegriffen und die Zustimmungsaufforderung akzeptiert. |
| Geliefert | Dem Benutzer wurde Zugriff auf alle Ressourcen im Zugriffspaket zugewiesen. |
| Zugriff verlängern | Wenn in der Richtlinie Verlängerungen zulässig sind, hat der Benutzer die Zuweisung verlängert. |
| Zugriff abgelaufen | Der Benutzerzugriff auf das Zugriffspaket ist abgelaufen. Um wieder Zugriff zu erhalten, muss der Benutzer eine Anforderung senden. |

## <a name="email-notifications"></a>E-Mail-Benachrichtigungen

Wenn Sie eine genehmigende Person sind, werden Ihnen E-Mail-Benachrichtigungen gesendet, wenn Sie eine Zugriffsanforderung genehmigen müssen und wenn eine Zugriffsanforderung abgeschlossen wurde. Wenn Sie ein Anforderer sind, werden Ihnen E-Mail-Benachrichtigungen gesendet, die den Status Ihrer Anforderung angeben.

Die folgenden Diagramme zeigen, wann diese E-Mail-Benachrichtigungen an die genehmigenden Personen oder den Anforderer gesendet werden. Die entsprechende Nummer für die in den Diagrammen angezeigten E-Mail-Benachrichtigungen finden Sie in der [Tabelle der E-Mail-Benachrichtigungen](entitlement-management-process.md#email-notifications-table).

### <a name="primary-approvers-and-alternate-approvers"></a>Primäre genehmigende Person und alternative genehmigende Personen
Das folgende Diagramm zeigt den Ablauf für primäre genehmigende Personen und alternative genehmigende Personen sowie die E-Mail-Benachrichtigungen, die sie während des Anforderungsprozesses erhalten:

![Ablauf für primäre genehmigende Personen und alternative genehmigende Personen](./media/entitlement-management-process/primary-approvers-and-alternate-with-escalation-flow.png)

### <a name="requestors"></a>Anforderer
Das folgende Diagramm zeigt den Ablauf für die Anforderer sowie die E-Mail-Benachrichtigungen, die sie während des Anforderungsprozesses erhalten:

![Ablauf für Anforderer](./media/entitlement-management-process/requestor-approval-and-expiration-request-flow.png)

### <a name="email-notifications-table"></a>Tabelle der E-Mail-Benachrichtigungen
Die folgende Tabelle enthält weitere Details zu den einzelnen E-Mail-Benachrichtigungen. Sie können zum Verwalten dieser E-Mails Regeln verwenden. Beispielsweise können Sie in Outlook Regeln erstellen, um die E-Mails in einen Ordner zu verschieben, wenn die Betreffzeile Wörter aus dieser Tabelle enthält:

| # | E-Mail-Betreff | Sendeszenario | Gesendet an |
| --- | --- | --- | --- |
| 1 | Aktion erforderlich: Weitergeleitete Anforderung bis zum *[Datum]* genehmigen oder verweigern | Diese E-Mail wird an alternative genehmigende Personen der Stufe 1 gesendet (nachdem die Anforderung eskaliert wurde), um Maßnahmen zu ergreifen. | Alternative genehmigende Person der Stufe 1 |
| 2 | Aktion erforderlich: Anforderung bis zum *[Datum]* genehmigen oder ablehnen | Diese E-Mail wird an primäre genehmigende Personen der Stufe 1 gesendet (wenn die Eskalation deaktiviert wurde), um Maßnahmen zu ergreifen. | Primäre genehmigende Person der Stufe 1 |
| 3 | Erinnerung: Anforderung von *[Anforderer]* bis *[Datum]* genehmigen oder ablehnen | Diese Erinnerungs-E-Mail wird an primäre genehmigende Personen der Stufe 1 gesendet (wenn die Eskalation deaktiviert wurde), um Maßnahmen zu ergreifen. Diese Erinnerung wird nur gesendet, wenn bisher keine Maßnahmen ergriffen wurden. | Primäre genehmigende Person der Stufe 1 |
| 4 | Anforderung bis zum *[Datum]* um *[Uhrzeit]* genehmigen oder ablehnen | Diese E-Mail wird an primäre genehmigende Personen der Stufe 1 gesendet (wenn die Eskalation aktiviert wurde), um Maßnahmen zu ergreifen. | Primäre genehmigende Person der Stufe 1 |
| 5 | Erinnerung – Aktion erforderlich: Anforderung von *[Anforderer]* bis *[Datum]* genehmigen oder ablehnen | Diese Erinnerungs-E-Mail wird an primäre genehmigende Personen der Stufe 1 gesendet (wenn die Eskalation aktiviert wurde), um Maßnahmen zu ergreifen. Diese Erinnerung wird nur gesendet, wenn bisher keine Maßnahmen ergriffen wurden. | Primäre genehmigende Person der Stufe 1 |
| 6 | Anforderung für *[Zugriffspaket]* ist abgelaufen | Diese E-Mail wird an primäre genehmigende Personen der Stufe 1 und/oder alternative genehmigende Personen der Stufe 1 einer einstufigen oder mehrstufigen Anforderung gesendet, nachdem die Anforderung abgelaufen ist. | Primäre genehmigende Person der Stufe 1, alternative genehmigende Person der Stufe 1 |
| 7 | Anforderung von *[Anforderer]* für *[Zugriffspaket]* genehmigt | Diese E-Mail wird an primäre genehmigende Personen der Stufe 1 und/oder alternative genehmigende Personen der Stufe 1 gesendet, wenn eine Anforderung abgeschlossen wurde. | Primäre genehmigende Person der Stufe 1, alternative genehmigende Person der Stufe 1 |
| 8 | Anforderung von *[Anforderer]* für *[Zugriffspaket]* genehmigt | Diese E-Mail wird für eine zweistufige Anforderung an primäre genehmigende Personen der Stufe 1 und/oder alternative genehmigende Personen der Stufe 1 gesendet, wenn nur Stufe 1 der Anforderung genehmigt wurde. | Primäre genehmigende Person der Stufe 1, alternative genehmigende Person der Stufe 1 |
| 9 | Anforderung für *[Zugriffspaket]* abgelehnt | Diese E-Mail wird nur dann an den Anforderer gesendet, wenn die Anforderung abgelehnt wurde. | Anforderer |
| 10 | Ihre Anforderung für *[Zugriffspaket]* ist abgelaufen | Diese E-Mail wird am Ende von Stufe 1 einer einstufigen oder mehrstufigen Anforderung an den Anforderer gesendet, nachdem die Anforderung abgelaufen ist. | Anforderer |
| 18 | Sie haben jetzt Zugriff auf *[Zugriffspaket]* | Diese E-Mail wird an die Endbenutzer gesendet, sobald diese zugreifen können. | Anforderer |
| 19 | Zugriff für *[Zugriffspaket]* bis *[Datum]* verlängern | Diese E-Mail wird an die Endbenutzer gesendet, bevor ihr Zugriff abläuft. | Anforderer |
| 20 | Zugriff auf *[Zugriffspaket]* beendet | Diese E-Mail wird an die Endbenutzer gesendet, nachdem ihr Zugriff abgelaufen ist. | Anforderer |

### <a name="access-request-emails"></a>E-Mails zu Zugriffsanforderungen

Wenn ein Anforderer eine Zugriffsanforderung für ein Zugriffspaket sendet, das eine Genehmigung erfordert, erhalten alle in der Richtlinie festgelegten genehmigenden Personen eine E-Mail-Benachrichtigung mit Details zur Anforderung. Zu den Details gehören der Name des Anforderers, die Organisation, das Start- und Enddatum für den Zugriff, (sofern angegeben) eine geschäftlichen Begründung sowie der Zeitpunkt der Anforderungsübermittlung und des Anforderungsablaufs.

Die E-Mail enthält einen Link, über den die genehmigenden Personen zu „Mein Zugriff“ wechseln können, um die Zugriffsanforderung zu genehmigen oder abzulehnen. Hier sehen Sie ein Beispiel einer E-Mail-Benachrichtigung, die an eine genehmigende Person gesendet wird, wenn ein Anforderer eine Zugriffsanforderung übermittelt:

![E-Mail für die Genehmigung der Anforderung zum Zugreifen auf ein Paket](./media/entitlement-management-shared/approver-request-email.png)

Die primären genehmigenden Personen erhalten ebenfalls eine E-Mail-Benachrichtigung mit einer Erinnerung, Maßnahmen in Bezug auf die Anforderung zu ergreifen und eine Entscheidung zu treffen. Hier sehen Sie ein Beispiel einer E-Mail-Benachrichtigung, die primäre genehmigende Personen als Erinnerung erhalten, um Maßnahmen zu ergreifen:

![Erinnerungs-E-Mail für Zugriffsanforderung](./media/entitlement-management-process/approver-access-request-reminder-email.png)

### <a name="alternate-approver-request-emails"></a>E-Mails an alternative genehmigende Personen für Anforderungen

Wenn (per Weiterleitungsrichtlinie) eine Weiterleitung an alternative genehmigende Personen aktiviert ist, wird die Anforderung weitergeleitet, wenn die Anforderung noch aussteht. Die alternative genehmigende Person erhält eine E-Mail-Benachrichtigung, um die Anforderung zu genehmigen oder abzulehnen. Hier sehen Sie ein Beispiel einer E-Mail-Benachrichtigung, die die alternative genehmigende Person erhält:

![E-Mail an alternative genehmigende Personen für Anforderungen](./media/entitlement-management-process/alternate-approver-email-fwd-request.png)

Sowohl die primären genehmigenden Personen als auch die alternativen genehmigenden Personen können die Anforderung genehmigen oder ablehnen.

### <a name="approved-or-denied-emails"></a>E-Mails über Genehmigung oder Ablehnung

Anforderer werden benachrichtigt, wenn ihre Zugriffsanforderung genehmigt wurde und für den Zugriff verfügbar ist oder wenn die Zugriffsanforderung abgelehnt wurde. Wenn eine genehmigende Person eine von einem Anforderer gesendete Zugriffsanforderung empfängt, kann sie die Zugriffsanforderung genehmigen oder ablehnen. Die genehmigende Person muss eine geschäftliche Begründung der Entscheidung hinzufügen. Hier sehen Sie ein Beispiel einer E-Mail-Benachrichtigung, die primäre oder alternative genehmigende Personen erhalten, nachdem eine Anforderung genehmigt wurde:

![E-Mail zur Überprüfung einer Zugriffsanforderung](./media/entitlement-management-process/approver-request-email-approved.png)

Wenn eine Zugriffsanforderung genehmigt und der Zugriff erteilt wurde, wird der Anforderer per E-Mail benachrichtigt, dass er nun Zugriff auf das Zugriffspaket hat. Hier sehen Sie ein Beispiel einer E-Mail-Benachrichtigung, die an einen Anforderer gesendet wird, wenn ihm Zugriff auf ein Zugriffspaket gewährt wurde:

![E-Mail über abgelaufene Zugriffsanforderung](./media/entitlement-management-process/requestor-email-approved.png)

Wenn eine Zugriffsanforderung abgelehnt wird, erhält der Anforderer eine E-Mail-Benachrichtigung. Hier sehen Sie ein Beispiel einer E-Mail-Benachrichtigung, die an einen Anforderer gesendet wird, wenn die Zugriffsanforderung abgelehnt wurde:

![E-Mail an Anforderer bei abgelehnter Anforderung](./media/entitlement-management-process/requestor-email-denied.png)

### <a name="expired-access-request-emails"></a>E-Mails über abgelaufene Zugriffsanforderungen

Zugriffsanforderungen können ablaufen, wenn keiner der genehmigenden Personen die Anforderung genehmigt oder abgelehnt hat. 

Wenn die Anforderung das konfigurierte Ablaufdatum erreicht und abläuft, kann sie von den genehmigenden Personen nicht mehr genehmigt oder abgelehnt werden. Hier sehen Sie ein Beispiel einer E-Mail-Benachrichtigung, die an alle primäre und alternative genehmigende Person gesendet wird:

 ![E-Mail über abgelaufene Zugriffsanforderung an genehmigende Personen](./media/entitlement-management-process/approver-request-email-expired.png)

 Der Anforderer erhält ebenfalls eine E-Mail-Benachrichtigung, um ihn darüber zu informieren, dass die Zugriffsanforderung abgelaufen ist und er die Zugriffsanforderung erneut senden muss. Hier sehen Sie ein Beispiel einer E-Mail-Benachrichtigung, die ein Anforderer erhält, wenn die Zugriffsanforderung abgelaufen ist:

![E-Mail über abgelaufene Zugriffsanforderung an Anforderer](./media/entitlement-management-process/requestor-email-request-expired.png)

## <a name="next-steps"></a>Nächste Schritte

- [Anfordern des Zugriffs auf ein Zugriffspaket](entitlement-management-request-access.md)
- [Genehmigen oder Ablehnen von Zugriffsanforderungen](entitlement-management-request-approve.md)
