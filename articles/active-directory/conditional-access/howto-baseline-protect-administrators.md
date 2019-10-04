---
title: 'Basisrichtlinie: MFA für Administratoren verlangen – Azure Active Directory'
description: Richtlinie für bedingten Zugriff, um eine mehrstufige Authentifizierung für Administratoren zu erfordern
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
ms.openlocfilehash: b00b061b1763d4b4e7236d8dc9ac1eedf7f923bc
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69533019"
---
# <a name="baseline-policy-require-mfa-for-admins-preview"></a>Basisrichtlinie: Benötigt MFA für Admins (Vorschau)

Benutzer mit Zugriff auf privilegierte Konten haben uneingeschränkten Zugriff auf Ihre Umgebung. Aufgrund der weitreichenden Befugnisse, die diese Konten haben, sollten Sie sie mit Bedacht verwalten. Eine gängige Methode, um den Schutz von privilegierten Konten zu verbessern, ist eine stärkere Form der Kontoüberprüfung bei der Anmeldung. In Azure Active Directory können Sie eine striktere Kontoüberprüfung erreichen, indem Sie eine mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) anfordern.

**MFA für Administratoren erforderlich (Vorschau)** ist eine [Basisrichtlinie](concept-baseline-protection.md), die MFA jedes Mal erfordert, wenn sich eine der folgenden privilegierten Administratorrollen anmeldet:

* Globaler Administrator
* SharePoint-Administrator
* Exchange-Administrator
* Administrator für den bedingten Zugriff
* Sicherheitsadministrator
* Helpdeskadministrator/Kennwortadministrator
* Rechnungsadministrator
* Benutzeradministrator

Beim Aktivieren der Richtlinie „MFA für Administratoren verlangen“ müssen sich die obigen neun Administratorrollen mit der Authenticator-App für MFA registrieren. Sobald die MFA-Registrierung abgeschlossen ist, müssen Administratoren bei jeder Anmeldung MFA ausführen.

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

Da die Richtlinie **„MFA für Administratoren erforderlich“ (Vorschau)** für alle kritischen Administratoren gilt, müssen mehrere Überlegungen angestellt werden, um eine reibungslose Bereitstellung zu gewährleisten. Beispielsweise müssen Benutzer und Dienstprinzipale in Azure AD identifiziert werden, die MFA nicht ausführen dürfen oder sollen, sowie die von Ihrer Organisation verwendeten Anwendungen und Clients, die keine moderne Authentifizierung unterstützen.

### <a name="legacy-protocols"></a>Ältere Protokolle

Ältere Authentifizierungsprotokolle (IMAP, SMTP, POP3 usw.) werden von E-Mail-Clients für Authentifizierungsanforderungen verwendet. Diese Protokolle unterstützen keine Multi-Factor Authentication. Die meisten der von Microsoft festgestellten Kontosicherheitsverletzungen werden durch Angriffe verursacht, die sich gegen ältere Protokolle richten und versuchen, die MFA zu umgehen. Um sicherzustellen, dass die MFA bei der Anmeldung mit einem Administratorkonto obligatorisch ist und von Angreifern nicht umgangen werden kann, blockiert diese Richtlinie alle Authentifizierungsanforderungen, die über ältere Protokolle für Administratorkonten ausgeführt werden.

> [!WARNING]
> Stellen Sie vor dem Aktivieren dieser Richtlinie sicher, dass Ihre Administratoren keine älteren Authentifizierungsprotokolle verwenden. Weitere Informationen finden Sie im Artikel [Gewusst wie: Legacy-Authentifizierung für Azure AD mit Conditional Access blockieren](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) für weitere Informationen.

## <a name="enable-the-baseline-policy"></a>Aktivieren der Basisrichtlinie

Die **Basisrichtlinie: MFA für Administratoren benötigt (Vorschau)** ist vorkonfiguriert und wird oben angezeigt, wenn Sie zur Ansicht Bedingter Zugriff im Microsoft Azure-Portal navigieren.

Gehen Sie wie folgt vor, um diese Richtlinie zu aktivieren und Ihre Administratoren zu schützen:

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Bedingter Zugriff**.
1. Wählen Sie in der Liste der Richtlinien den Eintrag **Basisrichtlinie: Benötigt MFA für Admins (Vorschau)** .
1. Legen Sie **Richtlinie aktivieren** auf **Richtlinie sofort verwenden** fest.
1. Klicken Sie auf **Speichern**.

> [!WARNING]
> Es gab eine Option, **Richtlinie in Zukunft automatisch aktivieren**, wenn diese Richtlinie in der Vorschau war. Wir haben diese Option entfernt, um plötzliche Benutzerauswirkungen zu minimieren. Wenn Sie diese Option ausgewählt haben, als sie verfügbar war, ist **Richtlinie Nicht verwenden** jetzt automatisch ausgewählt. Wenn sie diese Basisrichtlinie verwenden möchten, schauen Sie sich die obigen Schritte an, um sie zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter

* [Conditional Access grundlegende Schutzrichtlinien](concept-baseline-protection.md)
* [Fünf Schritte zum Sichern Ihrer Identitätsinfrastruktur](../../security/fundamentals/steps-secure-identity.md)
* [Was ist Conditional Access in Azure Active Directory?](overview.md)
