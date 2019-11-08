---
title: Aktivieren der kennwortlosen Anmeldung mit der Microsoft Authenticator-App (Vorschauversion) – Azure Active Directory
description: Aktivieren der kennwortlosen Anmeldung bei Azure AD mit der Microsoft Authenticator-App (Vorschauversion)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: acdcbef74d95fbbc4cfb49176d1f98621e746ff6
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73474360"
---
# <a name="enable-passwordless-sign-in-with-the-microsoft-authenticator-app-preview"></a>Aktivieren der kennwortlosen Anmeldung mit der Microsoft Authenticator-App (Vorschauversion)

Mit der Microsoft Authenticator-App können sich Benutzer bei jedem beliebigen Azure AD-Konto anmelden, ohne ein Kennwort zu verwenden. Ähnlich wie die Technologie von [Windows Hello for Business](/windows/security/identity-protection/hello-for-business/hello-identity-verification) nutzt Microsoft Authenticator die schlüsselbasierte Authentifizierung, um die Verwendung von Benutzeranmeldeinformationen zu ermöglichen, die an ein Gerät gebunden sind und auf biometrischen Daten oder einer PIN beruhen. Diese Authentifizierungsmethode kann auf jeder Geräteplattform (einschließlich Plattformen für mobile Geräte) und mit jeder App oder Website verwendet werden, die in Microsoft-Authentifizierungsbibliotheken integriert ist. 

![Beispiel für eine browserbasierte Anmeldung, die der Benutzer bestätigen muss](./media/howto-authentication-passwordless-phone/phone-sign-in-microsoft-authenticator-app.png)

Einem Benutzer, der die Anmeldung per Telefon über die Microsoft Authenticator-App aktiviert hat, wird nach dem Eingeben eines Benutzernamens keine Aufforderung zur Eingabe eines Kennworts angezeigt, sondern eine Meldung, in der er aufgefordert wird, in der App auf eine Zahl zu tippen. In der App muss der Benutzer auf die richtige Nummer tippen, „Genehmigen“ auswählen und dann seine PIN eingeben oder seinen biometrischen Schlüssel verwenden, um die Authentifizierung abzuschließen.

> [!NOTE]
> Diese Funktion ist seit März 2017 in der Microsoft Authenticator-App verfügbar. Daher ist es möglich, dass Benutzer bei aktivierter Richtlinie für ein Verzeichnis sofort auf diesen Flow stoßen und eine Fehlermeldung angezeigt wird, wenn die Benutzer nicht durch die Richtlinie aktiviert wurden. Bedenken Sie dies, und breiten Sie Ihre Benutzer auf die Umstellung vor.

## <a name="prerequisites"></a>Voraussetzungen

- Azure Multi-Factor Authentication mit Pushbenachrichtigungen als Überprüfungsmethode zulässig 
- Aktuelle Version von Microsoft Authenticator auf Geräten mit iOS 8.0 oder höher oder Android 6.0 oder höher installieren.

> [!NOTE]
> Wenn Sie die vorherige Vorschauversion der kennwortlosen Anmeldung mit der Microsoft Authenticator-App mithilfe von Azure AD PowerShell aktiviert haben, wurde sie für Ihr gesamtes Verzeichnis aktiviert. Wenn Sie die Aktivierung mit dieser neuen Methode vornehmen, wird die PowerShell-Richtlinie durch diese ersetzt. Es wird empfohlen, für alle Benutzer in Ihrem Mandanten die Aktivierung über die neuen Authentifizierungsmethoden vorzunehmen. Andernfalls können sich Benutzer, die nicht in der neuen Richtlinie enthalten sind, nicht mehr ohne Kennwort anmelden. 

## <a name="enable-passwordless-authentication-methods"></a>Aktivieren von Methoden zur kennwortlosen Authentifizierung

### <a name="enable-the-combined-registration-experience"></a>Aktivieren der kombinierten Registrierung

Die Registrierungsfunktionen für Methoden zur kennwortlosen Authentifizierung sind von der Vorschauversion der kombinierten Registrierung abhängig. Führen Sie die im Artikel [Aktivieren der kombinierten Registrierung von Sicherheitsinformationen (Vorschauversion)](howto-registration-mfa-sspr-combined.md) aufgeführten Schritte aus, um die Vorschauversion der kombinierten Registrierung zu aktivieren.

### <a name="enable-passwordless-phone-sign-in-authentication-methods"></a>Aktivieren von Authentifizierungsmethoden für die kennwortlose Anmeldung per Telefon

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Navigieren Sie zu **Azure AD-Sicherheit** > **Authentifizierungsmethoden** > **Authentifizierungsmethodenrichtlinie (Vorschau)** .
1. Wählen Sie unter **Kennwortlose Anmeldung per Telefon** die folgenden Optionen aus:
   1. **Aktivieren**: „Ja“ oder „Nein“
   1. **Ziel**: „Alle Benutzer“ oder „Benutzer auswählen“
1. Wählen Sie **Speichern** aus, um die neue Richtlinie festzulegen.

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Benutzerregistrierung und Verwaltung der Microsoft Authenticator-App

1. Rufen Sie [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) auf.
1. Melden Sie sich an, falls Sie dies noch nicht getan haben.
1. Fügen Sie eine Authentifikator-App hinzu. Klicken Sie dazu auf **Methode hinzufügen**, wählen Sie **Authenticator-App** aus, und klicken Sie dann auf **Hinzufügen**.
1. Folgen Sie den Anweisungen zum Installieren und Konfigurieren der Microsoft Authenticator-App auf Ihrem Gerät.
1. Klicken Sie auf **Fertig**, um den Flow zum Einrichten der Authenticator-App abzuschließen. 
1. Wählen Sie in **Microsoft Authenticator** im Dropdownmenü „Konto“ die Option **Anmeldung per Telefon aktivieren** aus.
1. Folgen Sie den Anweisungen in der App, um die Registrierung für die kennwortlose Anmeldung per Telefon abzuschließen. 

Organisationen können ihre Benutzer auf den Artikel [Anmelden an Ihren Konten per Microsoft Authenticator-App](../user-help/microsoft-authenticator-app-phone-signin-faq.md) verweisen, der weitere Unterstützung für die Einrichtung in der Microsoft Authenticator-App und die Aktivierung der Anmeldung per Telefon bietet.

## <a name="sign-in-with-passwordless-credential"></a>Anmelden mit kennwortlosen Anmeldeinformationen

In der Public Preview-Version ist es nicht möglich, die Erstellung oder Verwendung dieser neuen Anmeldeinformationen durch Benutzer zu erzwingen. Ein Benutzer kann sich erst ohne Kennwort anmelden, nachdem ein Administrator das Feature für den jeweiligen Mandanten aktiviert **und** der Benutzer die Microsoft Authenticator-App aktualisiert hat, um die Anmeldung per Telefon zu aktivieren.

Nachdem Sie Ihren Benutzernamen im Web eingegeben und **Weiter** ausgewählt haben, wird eine Zahl angezeigt, und Sie werden in der Microsoft Authenticator-App aufgefordert, die entsprechende Zahl für die Authentifizierung auszuwählen, statt Ihr Kennwort zu verwenden. 

![Beispiel für eine Browseranmeldung mit der Microsoft Authenticator-App](./media/howto-authentication-passwordless-phone/web-sign-in-microsoft-authenticator-app.png)

## <a name="known-issues"></a>Bekannte Probleme

### <a name="user-is-not-enabled-by-policy-but-still-has-passwordless-phone-sign-in-method-in-microsoft-authenticator"></a>Der Benutzer ist nicht durch eine Richtlinie aktiviert, hat aber in Microsoft Authenticator immer noch die Methode zur kennwortlosen Anmeldung per Telefon eingerichtet

Es ist möglich, dass ein Benutzer irgendwann in der aktuellen Microsoft Authenticator-App oder auf einem früheren Gerät Anmeldeinformationen für die kennwortlose Anmeldung per Telefon erstellt hat. Sobald ein Administrator die Authentifizierungsmethodenrichtlinie für die kennwortlose Anmeldung per Telefon aktiviert, wird allen Benutzern mit registrierten Anmeldeinformationen die neue Anmeldeaufforderung angezeigt, unabhängig davon, ob sie für die Verwendung der Richtlinie aktiviert wurden oder nicht. Wenn der Benutzer nicht berechtigt ist, die Anmeldeinformationen per Richtlinie zu verwenden, wird nach Abschluss des Authentifizierungsflows ein Fehler angezeigt. 

Der Administrator kann dem Benutzer die Verwendung der kennwortlosen Anmeldung per Telefon durch die entsprechende Aktivierung ermöglichen. Andernfalls muss der Benutzer die Methode entfernen. Wenn der Benutzer nicht mehr über das registrierte Gerät verfügt, kann er zu [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) navigieren und es entfernen. Wenn der Benutzer noch den Authenticator für MFA verwendet, kann er in Microsoft Authenticator die Option **Anmeldung per Telefon deaktivieren** auswählen.  

### <a name="ad-fs-integration"></a>AD FS-Integration

Wenn ein Benutzer die Microsoft Authenticator-Anmeldeinformationen ohne Kennwort aktiviert hat, wird die Authentifizierung für diesen Benutzer standardmäßig immer auf das Senden einer Genehmigungsbenachrichtigung festgelegt. Diese Logik verhindert, dass Benutzer in einem Hybridmandanten zur Überprüfung der Anmeldung zu Active Directory-Verbunddienste (AD FS) weitergeleitet werden, ohne zusätzlich auf „Stattdessen Ihr Kennwort verwenden“ zu klicken. Bei diesem Prozess werden auch alle lokalen Richtlinien für bedingten Zugriff und Passthrough-Authentifizierungsflüsse umgangen. 

Wenn bei einem Benutzer die Überprüfung der kennwortlosen Anmeldung per Telefon aussteht und er versucht, sich erneut anzumelden, wird der Benutzer möglicherweise zu AD FS weitergeleitet, um stattdessen ein Kennwort einzugeben.  

### <a name="azure-mfa-server"></a>Azure MFA-Server

Endbenutzer, für die MFA über den lokalen Azure MFA-Server einer Organisation aktiviert ist, können weiterhin einen einzelnen Satz von Anmeldeinformationen für die Anmeldung per Smartphone ohne Kennwort erstellen und verwenden. Wenn der Benutzer versucht, mehrere Installationen (fünf oder mehr) von Microsoft Authenticator mit den Anmeldeinformationen zu aktualisieren, kann diese Änderung zu einem Fehler führen.  

### <a name="device-registration"></a>Geräteregistrierung

Eine der Voraussetzungen für die Erstellung dieser neuen, sicheren Anmeldeinformationen ist, dass das Gerät, auf dem die Microsoft Authenticator-App installiert ist, auch im Azure AD-Mandanten für einen einzelnen Benutzer registriert sein muss. Aufgrund der aktuellen Einschränkungen, die für die Registrierung von Geräten gelten, kann ein Gerät nur in einem einzigen Mandanten registriert werden. Dies bedeutet, dass nur ein Geschäfts-, Schul- oder Unikonto in der Microsoft Authenticator-App für die Anmeldung per Smartphone aktiviert werden kann.

> [!NOTE]
> Die Geräteregistrierung ist nicht dasselbe wie die Geräteverwaltung oder „MDM“. Sie ordnet nur eine Geräte-ID und eine Benutzer-ID zusammen im Azure AD-Verzeichnis zu.  

## <a name="next-steps"></a>Nächste Schritte

[Was bedeutet „kennwortlos“?](concept-authentication-passwordless.md)

[Informationen zur Geräteregistrierung](../devices/overview.md#getting-devices-in-azure-ad)

[Informationen zu Microsoft Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
