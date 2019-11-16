---
title: Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln bei Azure AD (Vorschauversion) – Azure Active Directory
description: Aktivieren der kennwortlosen Anmeldung mit FIDO2-Sicherheitsschlüsseln bei Azure AD (Vorschauversion)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b3aa2add128cfc11a638fe6c7e03cfb25189afc
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74081555"
---
# <a name="enable-passwordless-security-key-sign-in-to-windows-10-devices-preview"></a>Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln bei Windows 10-Geräten (Vorschauversion)

In diesem Dokument liegt der Schwerpunkt auf der Aktivierung der auf FIDO2-Sicherheitsschlüsseln basierenden kennwortlosen Authentifizierung bei Windows 10-Geräten. Am Ende dieses Artikels können Sie sich mit Ihrem Azure AD-Konto mithilfe eines FIDO2-Sicherheitsschlüssels sowohl bei webbasierten Anwendungen als auch bei Ihren in Azure AD eingebundenen Windows 10-Geräten anmelden.

|     |
| --- |
| FIDO2-Sicherheitsschlüssel sind eine öffentliche Previewfunktion von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="requirements"></a>Requirements (Anforderungen)

- [Azure Multi-Factor Authentication](howto-mfa-getstarted.md)
- [Kombinierte Registrierung von Sicherheitsinformationen (Vorschauversion)](concept-registration-mfa-sspr-combined.md)
- Kompatible [FIDO2-Sicherheitsschlüssel](concept-authentication-passwordless.md#fido2-security-keys)
- WebAuthN erfordert Windows 10, Version 1809 oder höher.
- [In Azure AD eingebundene Geräte](../devices/concept-azure-ad-join.md) erfordern Windows 10, Version 1809 oder höher.
- [Microsoft Intune](https://docs.microsoft.com/intune/fundamentals/what-is-intune) (optional)
- Bereitstellungspaket (optional)

### <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

- Eine Bereitstellung von Windows Server Active Directory Domain Services (AD DS) für in die Domäne eingebundene (rein lokale) Geräte wird **nicht unterstützt**.
- RDP-, VDI- und Citrix-Szenarien werden bei Verwendung von Sicherheitsschlüsseln **nicht unterstützt**.
- S/MIME wird bei Verwendung von Sicherheitsschlüsseln **nicht unterstützt**.
- „Ausführen als“ wird bei Verwendung von Sicherheitsschlüsseln **nicht unterstützt**.
- Die Anmeldung bei einem Server mithilfe eines Sicherheitsschlüssels wird **nicht unterstützt**.
- Wenn Sie Ihren Sicherheitsschlüssel nicht verwendet haben, um sich online bei Ihrem Gerät anzumelden, können Sie ihn nicht verwenden, um sich offline anzumelden oder die Sperrung aufzuheben.

## <a name="prepare-devices-for-preview"></a>Vorbereiten von Geräten für die Vorschauversion

Auf den in Azure AD eingebundenen Geräten, auf denen Sie Pilotversuche ausführen, muss Windows 10 (Version 1809 oder höher) ausgeführt werden. Hierfür eignet sich besonders Windows 10 (Version 1903 oder höher).

## <a name="enable-security-keys-for-windows-sign-in"></a>Aktivieren von Sicherheitsschlüsseln für die Windows-Anmeldung

Organisationen können je nach den Anforderungen der Organisation eine oder mehrere der folgenden Methoden zum Aktivieren der Verwendung von Sicherheitsschlüsseln für die Windows-Anmeldung verwenden.

- [Aktivieren mit Intune](#enable-with-intune)
   - [Zielgerichtete Intune-Bereitstellung](#targeted-intune-deployment)
- [Aktivieren mit einem Bereitstellungspaket](#enable-with-a-provisioning-package)

### <a name="enable-with-intune"></a>Aktivieren mit Intune

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu **Microsoft Intune** > **Geräteregistrierung** > **Windows-Registrierung** > **Windows Hello for Business** > **Eigenschaften**.
1. Legen Sie unter **Einstellungen** die Option **Use security keys for sign-in** (Sicherheitsschlüssel zur Anmeldung verwenden) auf **Aktiviert** fest.

Die Konfiguration von Sicherheitsschlüsseln für die Anmeldung ist nicht von der Konfiguration von Windows Hello for Business abhängig.

#### <a name="targeted-intune-deployment"></a>Zielgerichtete Intune-Bereitstellung

Verwenden Sie zum Festlegen bestimmter Gerätegruppen als Ziel die folgenden benutzerdefinierten Einstellungen über Intune, um den Anmeldeinformationsanbieter zu aktivieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu **Microsoft Intune** > **Gerätekonfiguration** > **Profile** > **Profil erstellen**.
1. Konfigurieren Sie das neue Profil mit den folgenden Einstellungen:
   1. Name: Sicherheitsschlüssel für die Windows-Anmeldung
   1. Beschreibung: Aktiviert FIDO-Sicherheitsschlüssel für die Windows-Anmeldung
   1. Plattform: Windows 10 und höher
   1. Profiltyp: Benutzerdefiniert
   1. Benutzerdefinierte OMA-URI-Einstellungen:
      1. Name: Aktivieren der FIDO-Sicherheitsschlüssel für die Windows-Anmeldung
      1. OMA-URI: ./Device/Vendor/MSFT/PassportForWork/SecurityKey/UseSecurityKeyForSignin
      1. Datentyp: Integer
      1. Wert: 1
1. Diese Richtlinie kann bestimmten Benutzern, Geräten oder Gruppen zugewiesen werden. Weitere Informationen finden Sie im Artikel [Assign user and device profiles in Microsoft Intune (Zuweisen von Benutzer- und Geräteprofilen in Microsoft Intune)](https://docs.microsoft.com/intune/device-profile-assign).

![Screenshot: Richtlinienerstellung für die benutzerdefinierte Gerätekonfiguration in Intune](./media/howto-authentication-passwordless-security-key/intune-custom-profile.png)

### <a name="enable-with-a-provisioning-package"></a>Aktivieren mit einem Bereitstellungspaket

Für nicht von Intune verwaltete Geräte können Sie ein Bereitstellungspaket installieren, um die Funktion zu aktivieren. Die Windows Configuration Designer-App kann aus dem [Microsoft Store](https://www.microsoft.com/en-us/p/windows-configuration-designer/9nblggh4tx22) heruntergeladen und installiert werden.

1. Starten Sie Windows Configuration Designer.
1. Klicken Sie auf **Datei** > **Neues Projekt**.
1. Geben Sie dem Projekt einen Namen, und notieren Sie sich den Pfad, in dem Ihr Projekt erstellt wird.
1. Klicken Sie auf **Weiter**.
1. Lassen Sie das **Bereitstellungspaket** als **Selected project workflow** (Ausgewählter Projektworkflow), und klicken Sie auf **Weiter**.
1. Klicken Sie unter **Einstellungen auswählen, die angezeigt und konfiguriert werden sollen** auf **Alle Windows-Desktopeditionen**, und klicken Sie auf **Weiter**.
1. Wählen Sie **Fertig stellen** aus.
1. Navigieren Sie in Ihrem neu erstellten Projekt zu **Runtimeeinstellungen** > **Windows Hello For Business** > **Sicherheitsschlüssel** > **Use Security Key For Sign In** (Sicherheitsschlüssel für die Anmeldung verwenden).
1. Legen Sie **Use Security Key For Sign In** (Sicherheitsschlüssel für die Anmeldung verwenden) auf **Aktiviert** fest.
1. Klicken Sie auf **Exportieren** > **Bereitstellungspaket**.
1. Lassen Sie die Standardwerte im Fenster **Erstellen** unter **Beschreibung des Bereitstellungspakets** unverändert, und klicken Sie auf **Weiter**.
1. Lassen Sie die Standardwerte im Fenster **Erstellen** unter **Sicherheitsdetails für das Bereitstellungspaket auswählen** unverändert, und klicken Sie auf **Weiter**.
1. Notieren Sie sich den Pfad oder ändern Sie diesen im Fenster **Erstellen** unter **Speicherort des Bereitstellungspakets auswählen**, und klicken Sie dann auf **Weiter**.
1. Klicken Sie auf der Seite **Bereitstellungspaket erstellen** auf **Erstellen**.
1. Speichern Sie die zwei erstellten Dateien („ppkg“ und „cat“) an einem Speicherort für die spätere Anwendung auf Computer.
1. Befolgen Sie die Anweisungen im Artikel [Anwenden eines Bereitstellungspakets](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package), um das von Ihnen erstellte Bereitstellungspaket anzuwenden.

> [!NOTE]
> Auf Geräten unter Windows 10 (Version 1809) muss auch der Modus für freigegebene PCs (EnableSharedPCMode) aktiviert werden. Informationen zum Aktivieren dieser Funktion finden Sie im Artikel [Einrichten eines freigegebenen oder Gast-PCs unter Windows 10](https://docs.microsoft.com/windows/configuration/set-up-shared-or-guest-pc).

## <a name="sign-in-to-windows-with-a-fido2-security-key"></a>Anmelden bei Windows mit einem FIDO2-Sicherheitsschlüssel

Im nachstehenden Beispiel hat der Benutzer Bala Sandhu den FIDO2-Sicherheitsschlüssel bereits anhand der im vorherigen Artikel [Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln](howto-authentication-passwordless-security-key.md#user-registration-and-management-of-fido2-security-keys) aufgeführten Schritte bereitgestellt. Bala kann auf dem Windows 10-Sperrbildschirm den Anbieter für Sicherheitsschlüsselanmeldeinformationen auswählen und den Sicherheitsschlüssel für die Anmeldung bei Windows einfügen.

![Anmelden mit Sicherheitsschlüssel auf dem Windows 10-Sperrbildschirm](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Verwalten eines biometrischen Sicherheitsschlüssels bzw. einer PIN oder Zurücksetzen eines Sicherheitsschlüssels

* Windows 10, Version 1903 oder höher
   * Benutzer können die **Windows-Einstellungen** auf ihrem Gerät öffnen und zu **Konten** > **Sicherheitsschlüssel** navigieren.
   * Benutzer können ihre PIN ändern, Biometrie aktualisieren oder ihren Sicherheitsschlüssel zurücksetzen.

## <a name="troubleshooting-and-feedback"></a>Problembehandlung und Feedback

Wenn Sie Feedback geben möchten oder Probleme beim Anzeigen der Vorschau dieses Features auftreten, teilen Sie uns dies über die Windows-Feedback-Hub-App mit.

1. Starten Sie **Feedback-Hub**, und stellen Sie sicher, dass Sie angemeldet sind.
1. Senden Sie Ihr Feedback unter der folgenden Kategorisierung:
   1. Kategorie: Sicherheit und Datenschutz
   1. Unterkategorie: FIDO
1. Wenn Sie Protokolle erfassen möchten, verwenden Sie die folgende Option: **Problem reproduzieren**

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="does-this-work-in-my-on-premises-environment"></a>Funktioniert das in meiner lokalen Umgebung?

In einer rein lokalen Active Directory Domain Services-Umgebung (AD DS-Umgebung) funktioniert dieses Feature nicht.

### <a name="my-organization-requires-two-factor-authentication-to-access-resources-what-can-i-do-to-support-this-requirement"></a>Meine Organisation verlangt die Verwendung der zweistufigen Authentifizierung für den Zugriff auf Ressourcen. Was kann ich tun, um diese Anforderung zu unterstützen?

Sicherheitsschlüssel sind in einer Vielzahl von Formfaktoren verfügbar. Wenden Sie sich an den jeweiligen Gerätehersteller, um zu erfahren, wie die Geräte mit einer PIN oder biometrisch als zweitem Faktor aktiviert werden können.

### <a name="can-admins-set-up-security-keys"></a>Können Administratoren Sicherheitsschlüssel einrichten?

Wir arbeiten im Rahmen der allgemeinen Verfügbarkeit (General Availability, GA) dieses Features an dieser Funktion.

### <a name="where-can-i-go-to-find-compliant-security-keys"></a>Wo kann ich kompatible Sicherheitsschlüssel finden?

[FIDO2-Sicherheitsschlüssel](concept-authentication-passwordless.md#fido2-security-keys)

### <a name="what-do-i-do-if-i-lose-my-security-key"></a>Was ist zu tun, wenn ich meinen Sicherheitsschlüssel verliere?

Sie können im Azure-Portal Schlüssel entfernen, indem Sie zur Sicherheitsinformationsseite navigieren und den Sicherheitsschlüssel entfernen.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über die Geräteregistrierung](../devices/overview.md)

[Erfahren Sie mehr über Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
