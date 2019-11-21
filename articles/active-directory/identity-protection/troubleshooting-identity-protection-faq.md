---
title: Häufig gestellte Fragen (FAQs) zu Identity Protection in Azure Active Directory
description: Häufig gestellte Fragen zu Azure AD Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: troubleshooting
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d22973867782ddb64ced2ac95e84c0b27a3addd
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72886897"
---
# <a name="frequently-asked-questions-identity-protection-in-azure-active-directory"></a>Häufig gestellte Fragen zu Identity Protection in Azure Active Directory

## <a name="dismiss-user-risk-known-issues"></a>Ignorieren eines Benutzerrisikos – Bekannte Probleme

Über **Benutzerrisiko ignorieren** in Identity Protection (klassisch) wird der Akteur im Risikoverlauf des Benutzers in Identity Protection auf **Azure AD** festgelegt.

Über **Benutzerrisiko ignorieren** in Identity Protection wird der Akteur im Risikoverlauf des Benutzers in Identity Protection auf **\<Administratorname mit Link zum Blatt des Benutzers\>** festgelegt.

Derzeit verursacht ein bekanntes Problem Wartezeit beim Ablauf des Schließvorgangs für Benutzerrisiken. Wenn Sie eine „Benutzerrisiko-Richtlinie“ haben, wird diese Richtlinie innerhalb von Minuten, nachdem auf „Benutzerrisiko ignorieren“ geklickt wurde, nicht mehr auf verworfene Benutzer angewendet. Allerdings gibt es bekannte Verzögerungen beim Aktualisieren des „Risikozustands“ von verworfenen Benutzern in der Benutzerumgebung. Aktualisieren Sie als Problemumgehung die Seite im Browser, um den aktuellen „Risikozustand“ eines Benutzers anzuzeigen.

## <a name="risky-users-report-known-issues"></a>Bericht „Riskante Benutzer“ – Bekannte Probleme

Bei Abfragen für das Feld **Benutzername** wird die Groß-/Kleinschreibung beachtet, bei Abfragen für das Feld **Name** hingegen nicht.

Durch Umschalten von **Datum anzeigen als** wird die Spalte **LETZTE AKTUALISIERUNG DES RISIKOS** ausgeblendet. Um die Spalte zu lesen, klicken Sie oben auf dem Blatt „Riskante Benutzer“ auf **Spalten**.

Über **Alle Ereignisse schließen** im klassischen Identity Protection wird der Status der Risikoerkennungen auf **Geschlossen (gelöst)** festgelegt.

## <a name="risky-sign-ins-report-known-issues"></a>Bericht „Riskante Anmeldungen“ – Bekannte Probleme

Bei einer Risikoerkennung führt **Auflösen** dazu, dass der Status auf **Der Benutzer hat die durch eine risikobasierte Richtlinie ausgelöste MFA erfolgreich durchgeführt** festgelegt wird.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="why-cant-i-set-my-own-risk-levels-for-each-risk-detection"></a>Warum kann ich nicht meine eigenen Risikostufen für die einzelnen Risikoerkennungen festlegen?

Die Risikostufen in Identity Protection basieren auf der Genauigkeit der Erkennung und werden durch unser überwachtes Machine Learning-System unterstützt. Um festzulegen, welche Werte den Benutzern angezeigt werden, kann der Administrator bestimmte Benutzer/Gruppen in die Richtlinien für Benutzer- und Anmelderisiken einschließen oder sie davon ausschließen.

### <a name="why-does-the-location-of-a-sign-in-not-match-where-the-user-truly-signed-in-from"></a>Warum stimmt der Standort für eine Anmeldung nicht mit dem tatsächlichen Standort überein, von dem aus sich der Benutzer angemeldet hat?

Die IP-Zuordnung bei der Geolocation stellt branchenweit eine Herausforderung dar. Wenn der im Anmeldebericht aufgeführte Standort nicht mit dem tatsächlichen Standort übereinstimmt, wenden Sie sich an den Microsoft-Support. 

### <a name="how-do-the-feedback-mechanisms-in-identity-protection-work"></a>Wie funktionieren die Feedbackmechanismen in Identity Protection?

**Gefährdung bestätigen** (bei Anmeldung): Informiert Azure AD Identity Protection darüber, dass die Anmeldung nicht vom Identitätsinhaber durchgeführt wurde und weist auf eine Gefährdung hin.

- Nach Erhalt dieses Feedbacks wird der Anmelde- und Benutzerrisikostatus auf **Als gefährdet bestätigt** und die Risikostufe auf **Hoch** festgelegt.

- Darüber hinaus geben wir die Informationen an unsere Machine Learning-Systeme weiter, um die künftige Risikobewertung zu verbessern.

    > [!NOTE]
    > Wenn für den Benutzer bereits eine Bereinigung durchgeführt wurde, klicken Sie nicht auf **Gefährdung bestätigen**, weil dadurch der Anmelde- und Benutzerrisikostatus auf **Als gefährdet bestätigt** und die Risikostufe auf **Hoch** festgelegt wird.

**Sicherheit bestätigen** (bei Anmeldung): Informiert Azure AD Identity Protection darüber, dass die Anmeldung vom Identitätsinhaber durchgeführt wurde und keine Gefährdung vorliegt.

- Nach Erhalt dieses Feedbacks wird der Anmelde- und Benutzerrisikostatus auf **Als sicher bestätigt** und die Risikostufe auf **-** festgelegt.

- Darüber hinaus geben wir die Informationen an unsere Machine Learning-Systeme weiter, um die künftige Risikobewertung zu verbessern.

    > [!NOTE]
    > Wenn Sie davon ausgehen, dass der Benutzer nicht gefährdet ist, verwenden Sie anstelle von **Als sicher bestätigt** auf Anmeldeebene die Option **Benutzerrisiko ignorieren** auf Benutzerebene. Durch **Benutzerrisiko verwerfen** auf Benutzerebene werden das Benutzerrisiko und alle vergangenen riskanten Anmeldungen und Risikoerkennungen geschlossen.

### <a name="why-am-i-seeing-a-user-with-a-low-or-above-risk-score-even-if-no-risky-sign-ins-or-risk-detections-are-shown-in-identity-protection"></a>Warum sehe ich einen Benutzer mit einer niedrigen (oder höheren) Risikobewertung, auch wenn in Identity Protection keine riskanten Anmeldungen oder Risikoerkennungen angezeigt werden?

Da das Benutzerrisiko naturgemäß kumulativ zunimmt und nicht abläuft, kann für einen Benutzer selbst dann ein niedriges oder höheres Benutzerrisiko vorliegen, wenn keine aktuellen riskanten Anmeldungen oder Risikoerkennungen in Identity Protection angezeigt werden. Diese Situation kann eintreten, wenn die einzige schädliche Aktivität für einen Benutzer außerhalb des Zeitraums stattgefunden hat, für den Details zu riskanten Anmeldungen und Risikoerkennungen gespeichert werden. Es gibt keine Ablaufzeiträume für das Benutzerrisiko, weil böswillige Akteure nach der Kompromittierung einer Identität mitunter mehr als 140 Tage in der Kundenumgebung verbleiben, bevor sie ihren Angriff starten. Hier können Kunden die Risikozeitachse für einen Benutzer überprüfen, um die Gründe für ein Benutzerrisiko zu verstehen: `Azure Portal > Azure Active Directory > Risky users’ report > Click on an at-risk user > Details’ drawer > Risk history tab`

### <a name="why-does-a-sign-in-have-a-sign-in-risk-aggregate-score-of-high-when-the-detections-associated-with-it-are-of-low-or-medium-risk"></a>Warum liegt die Bewertung „Hoch“ für „Anmelderisiko (aggregiert)“ vor, wenn die zugehörigen Erkennungen ein niedriges oder mittleres Risiko aufweisen?

Die hohe aggregierte Risikobewertung könnte auf anderen Faktoren der Anmeldung oder darauf basieren, dass für diese Anmeldung mehrere Erkennungen ausgelöst wurden. Umgekehrt kann die aggregierte Risikobewertung „Mittel“ vorliegen, obwohl die mit der Anmeldung verknüpften Erkennungen ein hohes Risiko aufweisen. 
