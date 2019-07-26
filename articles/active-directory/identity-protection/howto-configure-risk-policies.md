---
title: Konfigurieren von Risikorichtlinien in Azure Active Directory Identity Protection (aktualisiert) | Microsoft-Dokumentation
description: Konfigurieren von Risikorichtlinien in Azure Active Directory Identity Protection (aktualisiert)
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ce4e2958978de9339f4340755e3740730025a5f
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334033"
---
# <a name="how-to-configure-risk-policies-in-azure-active-directory-identity-protection-refreshed"></a>Anleitung: Konfigurieren von Risikorichtlinien in Azure Active Directory Identity Protection (aktualisiert)

Azure AD erkennt Risikoereignisse, die auf potenziell gefährdete Identitäten hinweisen. Durch das Konfigurieren von Risikorichtlinien können Sie automatisierte Antworten auf die Erkennungsergebnisse definieren:

- Mit der Richtlinie zum Anmelderisiko können Sie eine Reaktion auf Echtzeitrisikoereignisse konfigurieren, die während der Anmeldung eines Benutzers erkannt wurden. 
- Mit der Richtlinie zum Benutzerrisiko können Sie eine Reaktion auf alle aktiven Benutzerrisiken konfigurieren, die im Laufe der Zeit für einen Benutzer erkannt wurden.  

> [!VIDEO https://www.youtube.com/embed/zEsbbik-BTE]

## <a name="what-is-the-sign-in-risk-policy"></a>Was ist die Richtlinie zum Anmelderisiko?

Azure AD analysiert jede Anmeldung eines Benutzers. Das Ziel der Analyse besteht darin, verdächtige Aktionen zu erkennen, die mit der Anmeldung verbunden sind. Wird die Anmeldung beispielsweise mit einer anonymen IP-Adresse durchgeführt, oder wird die Anmeldung von einem unbekannten Ort aus initiiert? In Azure AD werden die verdächtigen Aktionen, die vom System erkannt werden können, auch als Risikoereignisse bezeichnet. Basierend auf den Risikoereignissen, die während einer Anmeldung erkannt wurden, berechnet Azure AD einen Wert. Der Wert steht für die Wahrscheinlichkeit (gering, mittel, hoch), dass die Anmeldung nicht vom befugten Benutzer durchgeführt wird. Die Wahrscheinlichkeit wird als **Risikostufe der Anmeldung** bezeichnet.

Die Richtlinie zum Anmelderisiko ist eine automatisierte Antwort, die Sie für eine bestimmte Risikostufe der Anmeldung konfigurieren können. In Ihrer Antwort können Sie den Zugriff auf Ihre Ressourcen blockieren oder die Durchführung einer mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) zur Bedingung machen, bevor der Zugriff gewährt wird.

Wenn ein Benutzer eine MFA-Eingabeaufforderung, die durch die Anmelderisiko-Richtlinie ausgelöst wird, erfolgreich abschließt, gibt er Identity Protection eine Rückmeldung, dass die Anmeldung vom legitimen Benutzer stammt. Somit wird das Anmelde-Risikoereignis, das die MFA-Anfrage ausgelöst hat, automatisch geschlossen, und Identity Protection verhindert, dass dieses Ereignis zur Erhöhung des Benutzerrisikos beiträgt. Die Aktivierung der Anmelderisiko-Richtlinie kann überflüssigen Code in der Ansicht mit den risikoreichen Anmeldungen reduzieren, indem sie es den Benutzern ermöglicht, bei Aufforderung zur MFA selbstständig zu korrigieren und anschließend automatisch die zugehörige risikoreiche Anmeldung zu schließen.

## <a name="how-do-i-access-the-sign-in-risk-policy"></a>Wie greife ich auf die Richtlinie zum Anmelderisiko zu?
   
Die Richtlinie zum Anmelderisiko befindet sich auf der [Azure AD Identity Protection-Seite](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy) im Abschnitt **Konfigurieren**.
   
![Richtlinie zum Anmelderisiko](./media/howto-configure-risk-policies/1014.png "Richtlinie zum Anmelderisiko")

## <a name="sign-in-risk-policy-settings"></a>Einstellungen der Richtlinie zum Anmelderisiko

Sie müssen Folgendes festlegen, wenn Sie die Richtlinie zum Anmelderisiko konfigurieren:

- Die Benutzer und Gruppen, für die die Richtlinie gilt:

   ![Benutzer und Gruppen](./media/howto-configure-risk-policies/11.png)

- Die Risikostufe der Anmeldung, die die Richtlinie auslöst:

   ![Risikostufe für die Anmeldung](./media/howto-configure-risk-policies/12.png)

- Die Art des Zugriffs, die erzwungen werden soll, wenn Ihre Risikostufe der Anmeldung erfüllt wurde:  

   ![Access](./media/howto-configure-risk-policies/13.png)

- Zustand Ihrer Richtlinie:

   ![Erzwingen der Richtlinie](./media/howto-configure-risk-policies/14.png)

Das Dialogfeld „Richtlinienkonfiguration“ enthält eine Option zum Schätzen der Auswirkungen, die sich aus der Neukonfiguration ergeben.

![Voraussichtliche Auswirkung](./media/howto-configure-risk-policies/15.png)

## <a name="what-you-should-know-about-sign-in-risk-policies"></a>Was Sie über Richtlinien zum Anmelderisiko wissen sollten

Sie können eine Sicherheitsrichtlinie für das Anmelderisiko so konfigurieren, dass MFA erzwungen wird:

![Anfordern von MFA](./media/howto-configure-risk-policies/16.png)

Aus Sicherheitsgründen gilt diese Einstellung aber nur für Benutzer, die bereits für MFA registriert wurden. Identity Protection blockiert Benutzer mit einer MFA-Anforderung, falls sie noch nicht für MFA registriert sind.

Sie sollten wie folgt vorgehen, wenn Sie MFA für riskante Anmeldungen erzwingen möchten:

1. Aktivieren Sie die Registrierungsrichtlinie für die mehrstufige Authentifizierung für die entsprechenden Benutzer.
2. Fordern Sie die betroffenen Benutzer auf, sich über eine nicht riskante Sitzung anzumelden, um eine MFA-Registrierung durchzuführen.

Mit diesen Schritten wird sichergestellt, dass die Multi-Factor Authentication für eine risikobehaftete Anmeldung erzwungen wird.

Für die Richtlinie zum Anmelderisiko gilt Folgendes:

- Sie wird auf den gesamten Browserdatenverkehr und alle Anmeldungen angewendet, für den bzw. die eine moderne Authentifizierung genutzt wird.
- Sie wird nicht auf Anwendungen angewendet, für die ältere Sicherheitsprotokolle verwendet werden, indem der WS-Trust-Endpunkt beim Verbund-IDP, z. B. ADFS, deaktiviert wird.

Eine Übersicht über die zugehörige Benutzeroberfläche finden Sie unter:

* [Wiederherstellung risikobehafteter Anmeldungen](flows.md#risky-sign-in-recovery)
* [Risikobehaftete Anmeldung blockiert](flows.md#risky-sign-in-blocked)  
* [Anmeldeverfahren von Azure AD Identity Protection](flows.md)  

## <a name="what-is-a-user-risk-policy"></a>Was ist eine Richtlinie zum Benutzerrisiko?

Azure AD analysiert jede Anmeldung eines Benutzers. Das Ziel der Analyse besteht darin, verdächtige Aktionen zu erkennen, die mit der Anmeldung verbunden sind. In Azure AD werden die verdächtigen Aktionen, die vom System erkannt werden können, auch als Risikoereignisse bezeichnet. Einige Risikoereignisse können in Echtzeit erkannt werden, aber es gibt auch Risikoereignisse, für die mehr Zeit benötigt wird. Um beispielsweise eine Fahrt zu ungewöhnlichen Orten zu erkennen, benötigt das System einen anfänglichen Lernzeitraum von 14 Tagen, um das Regelverhalten eines Benutzers kennenzulernen. Es gibt mehrere Optionen zum Lösen von erkannten Risikoereignissen. Beispielsweise können Sie einzelne Risikoereignisse manuell lösen oder lösen lassen, indem Sie eine Richtlinie für bedingten Zugriff für das Anmelderisiko oder Benutzerrisiko verwenden.

Alle Risikoereignisse, die für einen Benutzer erkannt und nicht gelöst wurden, werden als aktive Risikoereignisse bezeichnet. Die aktiven Risikoereignisse, die einem Benutzer zugeordnet sind, werden als Benutzerrisiko bezeichnet. Basierend auf dem Benutzerrisiko berechnet Azure AD eine Wahrscheinlichkeit (niedrig, mittel, hoch), mit der ein Benutzer kompromittiert wurde. Die Wahrscheinlichkeit wird als Benutzerrisikostufe bezeichnet.

![Benutzerrisiken](./media/howto-configure-risk-policies/11031.png)

Die Richtlinie zum Benutzerrisiko ist eine automatisierte Antwort, die Sie für eine bestimmte Benutzerrisikostufe konfigurieren können. Mit einer Richtlinie zum Benutzerrisiko können Sie den Zugriff auf Ihre Ressourcen blockieren oder eine Kennwortänderung erzwingen, um für ein Benutzerkonto den bereinigten Zustand wiederherzustellen.

## <a name="how-do-i-access-the-user-risk-policy"></a>Wie greife ich auf die Richtlinie zum Benutzerrisiko zu?
   
Die Richtlinie zum Benutzerrisiko befindet sich auf der [Azure AD Identity Protection-Seite](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy) im Abschnitt **Konfigurieren**.
   
![Richtlinie zum Benutzerrisiko](./media/howto-configure-risk-policies/11014.png)

## <a name="user-risk-policy-settings"></a>Einstellungen der Richtlinie zum Benutzerrisiko

Sie müssen Folgendes festlegen, wenn Sie die Richtlinie zum Benutzerrisiko konfigurieren:

- Die Benutzer und Gruppen, für die die Richtlinie gilt:

   ![Benutzer und Gruppen](./media/howto-configure-risk-policies/111.png)

- Die Risikostufe der Anmeldung, die die Richtlinie auslöst:

   ![Benutzerrisikostufe](./media/howto-configure-risk-policies/112.png)

- Die Art des Zugriffs, die erzwungen werden soll, wenn Ihre Risikostufe der Anmeldung erfüllt wurde:  

   ![Access](./media/howto-configure-risk-policies/113.png)

- Zustand Ihrer Richtlinie:

   ![Erzwingen der Richtlinie](./media/howto-configure-risk-policies/114.png)

Das Dialogfeld „Richtlinienkonfiguration“ enthält eine Option zum Schätzen der Auswirkungen, die sich aus Ihrer Konfiguration ergeben.

![Voraussichtliche Auswirkung](./media/howto-configure-risk-policies/115.png)

## <a name="what-you-should-know-about-user-risk-polices"></a>Was Sie über Richtlinien zum Benutzerrisiko wissen sollten

Sie können eine Sicherheitsrichtlinie für das Benutzerrisiko festlegen, um Benutzer je nach Risikostufe bei der Anmeldung zu blockieren.

![Blockieren](./media/howto-configure-risk-policies/116.png)

Das Sperren einer Anmeldung bewirkt Folgendes:

* Verhindert die Generierung neuer Benutzerrisikoereignisse für den betroffenen Benutzer
* Ermöglicht Administratoren die manuelle Bereinigung der Risikoereignisse, die die Identität des Benutzers beeinträchtigen, und stellen wieder einen sicheren Identitätszustand her

## <a name="best-practices"></a>Bewährte Methoden

Wenn Sie den Schwellenwert **Hoch** auswählen, wird die Richtlinie weniger häufig ausgelöst, und die Auswirkungen für Benutzer verringern sich.  

Hierbei werden aber Anmeldungen mit dem Status **Niedrig** und **Mittel**, die als risikobehaftet gekennzeichnet sind, aus der Richtlinie ausgeschlossen. Für einen Angreifer wird das Ausnutzen einer kompromittierten Identität so ggf. nicht blockiert.

Gehen Sie beim Festlegen der Richtlinie wie folgt vor:

- Schließen Sie die Benutzer aus, die nicht über eine mehrstufige Authentifizierung verfügen bzw. für die dies nicht möglich ist.
- Schließen Sie die Benutzer nach Gebietsschemas aus, für die die Aktivierung der Richtlinie keinen praktischen Nutzen hat (z. B. kein Zugriff auf den Helpdesk).
- Schließen Sie die Benutzer aus, die voraussichtlich viele falsch positive Ergebnisse generieren (Entwickler, Sicherheitsanalysten).
- Verwenden Sie während des anfänglichen Rollouts der Richtlinie (oder wenn Sie die Auswirkungen für Endbenutzer verringern möchten) den Schwellenwert **Hoch**.
- Verwenden Sie den Schwellenwert **Niedrig**, falls für Ihre Organisation eine höhere Sicherheitsebene erforderlich ist. Wenn Sie für den Schwellenwert **Niedrig** auswählen, erhöht sich der Aufwand für die Benutzer bei der Anmeldung, aber die Sicherheit wird erhöht.

Die empfohlene Standardvorgehensweise für die meisten Organisationen ist die Konfiguration einer Regel für den Schwellenwert **Mittel** , um eine gute Balance zwischen Benutzerfreundlichkeit und Sicherheit zu erzielen.

## <a name="next-steps"></a>Nächste Schritte

 [Channel 9: Show zu Azure AD und Identität: Schutz der Identität – Vorschauversion](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
