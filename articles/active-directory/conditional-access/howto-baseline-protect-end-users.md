---
title: 'Basisrichtlinie: Endbenutzerschutz (Vorschau) – Azure Active Directory'
description: Conditional Access Richtlinie, um eine mehrstufige Authentifizierung für Benutzer zu anzufordern.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: afcd9c9d3191caeabe182f499b5fd80cd8e1d8dd
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68608137"
---
# <a name="baseline-policy-end-user-protection-preview"></a>Basisrichtlinie: Endbenutzerschutz (Vorschau)

Wir gehen häufig davon aus, dass Administratorkonten die einzigen Konten sind, die durch die Multi-Factor Authentication (MFA) geschützt werden müssen. Administratoren haben umfassenden Zugriff auf vertrauliche Informationen und können Änderungen an abonnementweiten Einstellungen vornehmen. Angriffe richten sich jedoch häufig gegen Endbenutzer. Sobald Angreifer Zugang zum System haben, können sie im Namen des ursprünglichen Kontobesitzers auf vertrauliche Informationen zugreifen oder das vollständige Verzeichnis herunterladen, um einen Phishing-Angriff auf die gesamte Organisation einzuleiten. Eine gängige Methode, um alle Benutzer besser zu schützen, besteht in einer restriktiveren Kontoüberprüfung, beispielsweise durch die Multi-Factor Authentication (MFA).

Um ein ausgewogenes Verhältnis zwischen Sicherheit und Benutzerfreundlichkeit zu erzielen, sollten Benutzer nicht bei jeder Anmeldung zur Authentifizierung aufgefordert werden. Authentifizierungsanforderungen, die ein normales Benutzerverhalten darstellen, beispielsweise Anmeldungen vom selben Gerät und Standort, bergen ein geringes Risiko. Nur Anmeldungen, die als riskant eingestuft werden und böswillige Absichten erkennen lassen, sollten mithilfe der Multi-Factor Authentication abgesichert werden.

Der Schutz von Endbenutzern wird durch eine risikobasierte MFA-[Basisrichtlinie](concept-baseline-protection.md) gewährleistet, die alle Benutzer in einem Verzeichnis einschließlich der Administratorrollen schützt. Wenn Sie diese Richtlinie aktivieren, müssen sich alle Benutzer mithilfe der Authenticator-App für die MFA registrieren. Benutzer können die MFA-Registrierungsaufforderung 14 Tage lang ignorieren. Danach können sie sich erst wieder anmelden, nachdem sie sich für die MFA registriert haben. Nachdem sie sich für die MFA registriert haben, werden Benutzer nur während kritischen Anmeldeversuchen zur Multi-Factor Authentication aufgefordert. Kompromittierte Benutzerkonten werden gesperrt, bis das Kennwort zurückgesetzt und Risiken ausgeschlossen wurden.

> [!NOTE]
> Diese Richtlinie gilt für alle Benutzer einschließlich Gastkonten und wird bei der Anmeldung bei allen Anwendungen ausgewertet.

## <a name="recovering-compromised-accounts"></a>Wiederherstellen kompromittierter Konten

Der Microsoft-Dienst zur Erkennung kompromittierter Anmeldeinformationen schützt unsere Kunden, indem Kombinationen aus Benutzername/Kennwort ermittelt werden, die öffentlich verfügbar sind. Wenn eine Kombination mit einem unserer Benutzer übereinstimmt, treffen wir umgehend Maßnahmen, um das Konto zu schützen. Benutzer mit kompromittierten Anmeldeinformationen werden als gefährdet eingestuft und gesperrt. Sie können sich erst wieder anmelden, nachdem ihr Kennwort zurückgesetzt wurde.

Benutzer, die über eine Azure AD Premium-Lizenz verfügen, können den Zugriff über die Self-Service-Kennwortzurücksetzung (SSPR) wiederherstellen, wenn die Funktion in ihrem Verzeichnis aktiviert ist. Gesperrte Benutzer, die keine Premium-Lizenz besitzen, müssen sich an einen Administrator wenden, um ihr Kennwort manuell zurückzusetzen und das Benutzerereignis in Form einer Risikomarkierung zu schließen.

### <a name="steps-to-unblock-a-user"></a>So entsperren Sie einen Benutzer

Vergewissern Sie sich anhand der Anmeldeprotokolle des Benutzers, dass er durch die Richtlinie gesperrt wurde.

1. Ein Administrator muss sich beim **Azure-Portal** anmelden und zu **Azure Active Directory** > **Benutzer** navigieren, auf den Namen des Benutzers klicken und zu „Anmeldungen“ wechseln.
1. Um die Kennwortzurücksetzung für einen gesperrten Benutzer zu initiieren, navigiert ein Administrator zu **Azure Active Directory** > **Benutzer mit Risikomarkierung**.
1. Klicken Sie auf den Benutzer mit dem gesperrten Konto, um Informationen über seine letzte Anmeldeaktivität anzuzeigen.
1. Klicken Sie auf „Kennwort zurücksetzen“, um ein temporäres Kennwort zuzuweisen, das bei der nächsten Anmeldung geändert werden muss.
1. Klicken Sie auf „Alle Ereignisse schließen“, um die Risikobewertung des Benutzers zurückzusetzen.

Der Benutzer kann sich jetzt anmelden, sein Kennwort zurücksetzen und auf die Anwendung zugreifen.

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

Die Richtlinie **Endbenutzerschutz** betrifft alle Benutzer in Ihrem Verzeichnis. Daher sind verschiedene Aspekte zu berücksichtigen, um eine reibungslose Bereitstellung sicherzustellen. Beispielsweise müssen Benutzer und Dienstprinzipale in Azure AD identifiziert werden, die keine MFA ausführen dürfen oder sollen, sowie die von Ihrer Organisation verwendeten Anwendungen und Clients, die keine moderne Authentifizierung unterstützen.

### <a name="legacy-protocols"></a>Ältere Protokolle

Ältere Authentifizierungsprotokolle (IMAP, SMTP, POP3 usw.) werden von E-Mail-Clients für Authentifizierungsanforderungen verwendet. Diese Protokolle unterstützen keine Multi-Factor Authentication.  Die meisten der von Microsoft festgestellten Kontosicherheitsverletzungen werden durch Angriffe verursacht, die sich gegen ältere Protokolle richten und versuchen, die MFA zu umgehen. Um sicherzustellen, dass die MFA bei einer Kontoanmeldung obligatorisch ist und von Angreifern nicht umgangen werden kann, blockiert diese Richtlinie alle Authentifizierungsanforderungen, die über ältere Protokolle für Administratorkonten ausgeführt werden.

> [!WARNING]
> Vergewissern Sie sich vor der Aktivierung dieser Richtlinie, dass Ihre Benutzer keine älteren Authentifizierungsprotokolle verwenden. Weitere Informationen finden Sie im Artikel [Gewusst wie: Legacy-Authentifizierung für Azure AD mit Conditional Access blockieren](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) für weitere Informationen.

## <a name="enable-the-baseline-policy"></a>Aktivieren der Basisrichtlinie

Die **Basisrichtlinie: Endbenutzerschutz (Vorschau)** ist vorkonfiguriert und wird im Azure-Portal auf dem Blatt „Bedingter Zugriff“ ganz oben angezeigt.

Gehen Sie wie folgt vor, um diese Richtlinie zu aktivieren und Ihre Benutzer zu schützen:

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Conditional Access-Administrator an.
1. Navigieren Sie zu **Azure Active Directory** > **Bedingter Zugriff**.
1. Wählen Sie in der Liste der Richtlinien **Basisrichtlinie: Endbenutzerschutz (Vorschau)** aus.
1. Legen Sie **Richtlinie aktivieren** auf **Richtlinie sofort verwenden** fest.
1. Klicken Sie auf  **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter

* [Conditional Access grundlegende Schutzrichtlinien](concept-baseline-protection.md)
* [Fünf Schritte zum Sichern Ihrer Identitätsinfrastruktur](../../security/fundamentals/steps-secure-identity.md)
* [Was ist Conditional Access in Azure Active Directory?](overview.md)
