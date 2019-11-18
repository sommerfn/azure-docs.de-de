---
title: Anzeigen und Suchen Ihrer letzten Anmeldeaktivität auf der Seite „Meine Anmeldungen“ (Vorschauversion) – Azure Active Directory | Microsoft-Dokumentation
description: Details zur Anzeige und Suche Ihrer letzten Anmeldeaktivität auf der Seite „Meine Anmeldungen“ im Portal „Mein Konto“.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: rhicock
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 10/28/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9d134e522575f30fd1f1be8176e0e1589faa7ac
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73586883"
---
# <a name="view-and-search-your-recent-sign-in-activity-from-the-my-sign-ins-preview-page"></a>Anzeigen und Suchen Ihrer letzten Anmeldeaktivität auf der Seite „Meine Anmeldedaten“ (Vorschauversion)

Sie können die letzten Anmeldeaktivitäten für Ihr Geschäfts-, Schul- oder Unikonto auf der Seite **Meine Anmeldungen** im Portal **Mein Konto** einsehen. Durch die Überprüfung Ihres Anmeldeverlaufs können Sie feststellen, ob ungewöhnliche Aktivitäten stattgefunden haben. Dabei wird Ihnen Folgendes angezeigt:

- Ob jemand versucht, Ihr Kennwort zu erraten.

- Ob sich ein Angreifer erfolgreich bei ihrem Konto angemeldet hat und an welchem Standort die Anmeldung erfolgt ist.

- Auf welche Apps der Angreifer versucht hat, zuzugreifen.

## <a name="view-your-recent-sign-in-activity"></a>Anzeigen Ihrer letzten Anmeldeaktivität

1. Melden Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto an, und rufen Sie die Seite https://myprofile.microsoft.com/ auf.

2. Wählen Sie im linken Navigationsbereich **Meine Anmeldungen (Vorschauversion)** aus, oder wählen Sie im Block **Meine Anmeldungen (Vorschauversion)** den Link **Letzte Aktivität überprüfen** aus.

    ![Seite „Mein Konto“ mit hervorgehobenen Links für die letzte Aktivität](media/my-account-portal/my-account-portal-sign-ins.png)

3. Erweitern Sie alle Anmeldeelemente, und überprüfen Sie diese. Stellen Sie sicher, dass Sie alle erkennen. Wenn Sie ein Anmeldeelement finden, das Ihnen unbekannt ist, empfehlen wir dringend, Ihr Kennwort zu ändern, um Ihr Konto vor einer Sicherheitsgefährdung zu schützen.

    ![Seite „Letzte Aktivität“ mit erweiterten Anmeldedetails](media/my-account-portal/my-account-portal-sign-ins-page.png)

### <a name="if-you-see-a-successful-sign-in"></a>Wenn eine erfolgreiche Anmeldung angezeigt wird

Sie sollten ihre eigene Aktivität als unauffällige Aktivität erkennen. Wenn Sie jedoch eine erfolgreiche Anmeldung von einem fremden Standort, Browser oder Betriebssystem feststellen, kann dies bedeuten, dass ein Angreifer Zugriff auf Ihr Konto erhalten hat. In dieser Situation empfehlen wir Ihnen, Ihr Kennwort sofort zu ändern und dann auf der Seite [Sicherheitsinformation](https://mysignins.microsoft.com/security-info) Ihre Sicherheitseinstellungen zu aktualisieren.

Bevor Sie festlegen, dass etwas nicht richtig ist, stellen Sie sicher, dass Sie kein falsch-positives Ergebnis sehen (wobei das Element fragwürdig aussieht, aber in Ordnung ist). So ermitteln wir beispielsweise Ihren ungefähren Standort und die Zuordnung basierend auf Ihrer IP-Adresse. Mobilfunknetze sind besonders schwer zu lokalisieren, da sie den Datenverkehr gelegentlich über entfernte Standorte leiten. Wenn Sie sich also mit Ihrem mobilen Gerät im Bundesstaat Washington angemeldet haben, kann als Standort für die Anmeldung Kalifornien angegeben sein. Aus diesem Grund empfehlen wir Ihnen dringend, weitere Details als nur den Standort zu überprüfen. Sie sollten auch sicherstellen, dass Betriebssystem, Browser und App ebenfalls für Sie Sinn ergeben.

### <a name="if-you-see-an-unsuccessful-sign-in"></a>Wenn eine nicht erfolgreiche Anmeldung angezeigt wird

Eine nicht erfolgreiche Anmeldung ohne Sitzungsaktivität bedeutet, dass die primäre Authentifizierungsmethode (Benutzername/Kennwort) nicht erfolgreich war. Dies könnte bedeuten, dass Sie Ihren Benutzernamen oder Ihr Kennwort falsch eingegeben haben, aber auch, dass ein Angreifer versucht hat, Ihr Kennwort zu erraten. Wenn Sie denken, dass ein Angreifer erfolglos versucht hat, Ihr Kennwort zu erraten, müssen Sie nicht unbedingt Ihr Kennwort ändern, aber wir empfehlen Ihnen dringend, sich für Azure Multi-Factor Authentication (MFA) zu registrieren. Mit MFA ist das Kennwort auch dann nicht ausreichend für die Anmeldung, wenn der Hacker Ihr Kennwort letztendlich erraten hat.

Wenn eine nicht erfolgreiche Anmeldung angezeigt wird, bei der für die Sitzungsaktivität der Hinweis **Zusätzliche Überprüfung fehlgeschlagen, ungültiger Code** angegeben ist, bedeutet dies, dass Ihre primäre Authentifizierungsmethode (Benutzername/Kennwort) erfolgreich war, die MFA aber fehlgeschlagen ist. Wenn es sich hierbei um einen Angreifer handelt, wurde Ihr Kennwort zwar richtig erraten, aber die MFA-Prüfung trotzdem nicht bestanden. In diesem Fall empfehlen wir Ihnen, Ihr Kennwort trotzdem zu ändern, da der Angreifer dieses Element richtig erraten hat, und dann auf der Seite [Sicherheitsinformation](https://mysignins.microsoft.com/security-info) Ihre Sicherheitseinstellungen zu aktualisieren.

## <a name="search-for-specific-sign-in-activity"></a>Suchen nach einer spezifischen Anmeldeaktivität

Sie können Ihre letzten Anmeldeaktivitäten anhand der verfügbaren Informationen suchen. Beispielsweise können Sie nach der aktuellen Anmeldeaktivität anhand von Betriebssystem, Standort, App usw. suchen.

1. Geben Sie auf der Seite **Letzte Aktivität überprüfen** die gesuchten Informationen in die Leiste **Suchen** ein. Geben Sie z. B. `My Account` ein, um nach allen von der App „Mein Konto“ gesammelten Aktivitäten zu suchen.

2. Wählen Sie die Schaltfläche **Suche** aus, um die Suche zu starten.

    ![Seite „Letzte Aktivität“ mit hervorgehobener Suchleiste, Schaltfläche „Suche“ und den Ergebnissen](media/my-account-portal/my-account-portal-sign-ins-page-search.png)

## <a name="next-steps"></a>Nächste Schritte

Nach dem Anzeigen der letzten Anmeldeaktivität haben Sie folgende Möglichkeiten:

- Anzeigen oder Verwalten der [Sicherheitsinformationen](user-help-security-info-overview.md)

- Anzeigen oder Verwalten Ihrer verbundenen [Geräte](my-account-portal-devices-page.md)

- Anzeigen oder Verwalten Ihrer [Organisationen](my-account-portal-organizations-page.md)

- Anzeigen von Informationen zur [Nutzung der datenschutzrelevanten Informationen](my-account-portal-privacy-page.md) in Ihrer Organisation
