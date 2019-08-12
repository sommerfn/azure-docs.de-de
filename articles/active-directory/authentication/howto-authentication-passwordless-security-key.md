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
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: a063891339a46366490447b7c7a7a1a14fd81be6
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68828790"
---
# <a name="enable-passwordless-security-key-sign-in-for-azure-ad-preview"></a>Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln bei Azure AD (Vorschauversion)

## <a name="requirements"></a>Requirements (Anforderungen)

* Azure Multi-Factor Authentication
* Kombinierte Registrierung (Vorschauversion) mit für SSPR aktivierte Benutzer
* Kompatible FIDO2-Sicherheitsschlüssel für die Vorschauversion der FIDO2-Sicherheitsschlüssel
* Microsoft Edge unter Windows 10 (Version 1809 oder höher) für die Webauthentifizierung (WebAuthN)
* Windows 10 (Version 1809 oder höher) in Azure AD eingebunden für die auf FIDO2 basierende Windows-Anmeldung

## <a name="prepare-devices-for-preview"></a>Vorbereiten von Geräten für die Vorschauversion

Auf den von Ihnen verwendeten Geräten muss Windows 10 (Version 1809 oder höher) ausgeführt werden. Hierfür eignet sich besonders Windows 10 (Version 1903 oder höher).

## <a name="enable-security-keys-for-windows-sign-in"></a>Aktivieren von Sicherheitsschlüsseln für die Windows-Anmeldung

Organisationen können eine oder mehrere der folgenden Methoden zum Aktivieren der Verwendung von Sicherheitsschlüsseln für die Windows-Anmeldung verwenden.

### <a name="enable-credential-provider-via-intune"></a>Aktivieren eines Anmeldeinformationsanbieters über Intune

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu **Microsoft Intune** > **Geräteregistrierung** > **Windows-Registrierung** > **Windows Hello for Business** > **Eigenschaften**.
1. Legen Sie unter **Einstellungen** die Option **Use security keys for sign-in** (Sicherheitsschlüssel zur Anmeldung verwenden) auf **Aktiviert** fest.

Die Konfiguration der Sicherheitsschlüssel für die Anmeldung ist nicht von der Konfiguration von Windows Hello for Business abhängig.

#### <a name="enable-targeted-intune-deployment"></a>Aktivieren der zielgerichteten Intune-Bereitstellung

Verwenden Sie zum Festlegen bestimmter Gerätegruppen als Ziel die folgenden benutzerdefinierten Einstellungen über Intune, um den Anmeldeinformationsanbieter zu aktivieren. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu **Microsoft Intune** > **Gerätekonfiguration** > **Profile** > **Profil erstellen**.
1. Konfigurieren Sie das neue Profil mit den folgenden Einstellungen:
   1. Name: Sicherheitsschlüssel für die Windows-Anmeldung
   1. Beschreibung: Aktiviert FIDO-Sicherheitsschlüssel für die Windows-Anmeldung
   1. Plattform: Windows 10 und höher
   1. Plattformtyp: Benutzerdefiniert
   1. Benutzerdefinierte OMA-URI-Einstellungen:
      1. Name: Aktivieren der FIDO-Sicherheitsschlüssel für die Windows-Anmeldung
      1. OMA-URI: ./Device/Vendor/MSFT/PassportForWork/SecurityKey/UseSecurityKeyForSignin
      1. Datentyp: Integer
      1. Wert: 1 
1. Diese Richtlinie kann bestimmten Benutzern, Geräten oder Gruppen zugewiesen werden. Weitere Informationen finden Sie im Artikel [Assign user and device profiles in Microsoft Intune (Zuweisen von Benutzer- und Geräteprofilen in Microsoft Intune)](https://docs.microsoft.com/intune/device-profile-assign).

![Screenshot: Richtlinienerstellung für die benutzerdefinierte Gerätekonfiguration in Intune](./media/howto-authentication-passwordless-security-key/intune-custom-profile.png)

### <a name="enable-credential-provider-via-provisioning-package"></a>Aktivieren des Anmeldeinformationsanbieters mittels eines Bereitstellungspakets

Für nicht von Intune verwaltete Geräte können Sie ein Bereitstellungspaket installieren, um die Funktion zu aktivieren. Die Windows Configuration Designer-App kann aus dem [Microsoft Store](https://www.microsoft.com/store/apps/9nblggh4tx22) heruntergeladen und installiert werden.

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

## <a name="obtain-fido2-security-keys"></a>Abrufen von FIDO2-Sicherheitsschlüsseln

Weitere Informationen zu unterstützten Schlüsseln und Herstellern finden Sie im Abschnitt „FIDO2-Sicherheitsschlüssel“ im Artikel [What is passwordless? (Was genau ist die kennwortlose Authentifizierung?)](concept-authentication-passwordless.md).

> [!NOTE]
> Wenn Sie auf NFC basierte Sicherheitsschlüssel erwerben und diese verwenden möchten, benötigen Sie einen unterstützten NFC-Leser.

## <a name="enable-passwordless-authentication-method"></a>Aktivieren von Methoden zur kennwortlosen Authentifizierung

### <a name="enable-the-combined-registration-experience"></a>Aktivieren der kombinierten Registrierung

Die Registrierungsfunktionen für Methoden zur kennwortlosen Authentifizierung sind von der Vorschauversion der kombinierten Registrierung abhängig. Führen Sie die im Artikel [Aktivieren der kombinierten Registrierung von Sicherheitsinformationen (Vorschauversion)](howto-registration-mfa-sspr-combined.md) aufgeführten Schritte aus, um die Vorschauversion der kombinierten Registrierung zu aktivieren.

### <a name="enable-new-passwordless-authentication-method"></a>Aktivieren neuer kennwortloser Authentifizierungsmethoden

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Authentifizierungsmethoden** > **Authentifizierungsmethodenrichtlinie (Vorschau)** .
1. Wählen Sie bei jeder **Methode** die folgenden Optionen aus:
   1. **Aktivieren**: „Ja“ oder „Nein“
   1. **Ziel**: „Alle Benutzer“ oder „Benutzer auswählen“
1. **Speichern** Sie jede Methode.

> [!WARNING]
> Die „Einschränkungsrichtlinien für FIDO2-Sicherheitsschlüssel“ funktionieren noch nicht. Diese Funktion wird vor der allgemeinen Verfügbarkeit zur Verfügung stehen. Ändern Sie diese Standardrichtlinien nicht.

> [!NOTE]
> Sie müssen nicht beide kennwortlosen Methoden abonnieren. (Wenn Sie nur die Vorschauversion einer kennwortlosen Methode verwenden möchten, können Sie auch nur diese Methode aktivieren.) Wir empfehlen Ihnen das Testen beider Methoden, da beide gewisse Vorteile mit sich bringen.

## <a name="user-registration-and-management-of-fido2-security-keys"></a>Benutzerregistrierung und Verwaltung von FIDO2-Sicherheitsschlüsseln

1. Rufen Sie [https://myprofile.microsoft.com](https://myprofile.microsoft.com) auf.
1. Melden Sie sich an, falls Sie dies noch nicht getan haben.
1. Klicken Sie auf **Sicherheitsinformation**.
   1. Wenn der Benutzer bereits mindestens eine mehrstufige Authentifizierungsmethode registriert hat, kann er sofort einen FIDO2-Sicherheitsschlüssel registrieren.
   1. Wenn der Benutzer keine mehrstufige Authentifizierungsmethode registriert hat, muss er eine hinzufügen.
1. Fügen Sie einen FIDO2-Sicherheitsschlüssel hinzu, indem Sie zuerst auf **Methode hinzufügen** und dann auf **Sicherheitsschlüssel** klicken.
1. Klicken Sie auf **USB-Gerät** oder **NFC-Gerät**.
1. Halten Sie Ihren Schlüssel bereit, und klicken Sie auf **Weiter**.
1. Sie werden aufgefordert, im angezeigten Feld eine PIN für Ihren Sicherheitsschlüssel zu erstellen/einzugeben. Führen Sie dann die erforderliche Geste für Ihren Schlüssel biometrisch oder per Touch aus.
1. Sie gelangen wieder zurück zur kombinierten Registrierung und werden aufgefordert, einen aussagekräftigen Namen für Ihr Token bereitzustellen, sodass Sie dieses von den anderen unterscheiden können. Klicken Sie auf **Weiter**.
1. Klicken Sie auf **Fertig**, um den Prozess abzuschließen.

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>Verwalten eines biometrischen Sicherheitsschlüssels bzw. einer PIN oder Zurücksetzen eines Sicherheitsschlüssels

* Windows 10, Version 1809
   * Hierfür ist die Software des Anbieters des Sicherheitsschlüssels erforderlich.
* Windows 10, Version 1903 oder höher
   * Benutzer können die **Windows-Einstellungen** auf ihrem Gerät öffnen und zu **Konten** > **Sicherheitsschlüssel** navigieren.
   * Benutzer können ihre PIN ändern, Biometrie aktualisieren oder ihren Sicherheitsschlüssel zurücksetzen.

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Benutzerregistrierung und Verwaltung der Microsoft Authenticator-App

Befolgen Sie die Anleitung im Artikel [Sign in to your accounts using the Microsoft Authenticator app (Anmelden bei Konten mithilfe der Microsoft Authenticator-App)](../user-help/user-help-auth-app-sign-in.md), um die Microsoft Authenticator-App für die Anmeldung per Smartphone zu konfigurieren.

## <a name="sign-in-with-passwordless-credential"></a>Anmelden mit kennwortlosen Anmeldeinformationen

### <a name="sign-in-at-the-lock-screen"></a>Anmelden auf dem Sperrbildschirm

Im nachfolgenden Beispiel hat der Benutzer Bala Sandhu seinen FIDO2-Sicherheitsschlüssel bereits bereitgestellt. Bala kann auf dem Windows 10-Sperrbildschirm den Anbieter für Sicherheitsschlüsselanmeldeinformationen auswählen und den Sicherheitsschlüssel für die Anmeldung bei Windows einfügen.

![Anmelden mit Sicherheitsschlüssel auf dem Windows 10-Sperrbildschirm](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="sign-in-on-the-web"></a>Anmelden im Web

Im nachfolgenden Beispiel hat der Benutzer bereits einen FIDO2-Sicherheitsschlüssel bereitgestellt. Der Benutzer kann sich im Web mit seinem FIDO2-Sicherheitsschlüssel im Microsoft Edge-Browser unter Windows 10 Version 1809 oder höher anmelden.

![Anmelden mit Sicherheitsschlüssel in Microsoft Edge](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-edge-sign-in.png)

## <a name="known-issues"></a>Bekannte Probleme

### <a name="security-key-provisioning"></a>Bereitstellung von Sicherheitsschlüsseln

Die Administratorbereitstellung und Bereitstellungsaufhebung von Sicherheitsschlüsseln ist in der öffentlichen Vorschauversion nicht verfügbar.

### <a name="hybrid-azure-ad-join"></a>Azure AD-Hybrideinbindung

Benutzer, die das einmalige Anmelden mit WIA und verwaltete Anmeldeinformationen wie FIDO2-Sicherheitsschlüssel oder das kennwortlose Anmelden mit der Microsoft Authenticator-App verwenden, benötigen unter Windows 10-Geräten die Hybrideinbindung, um die Vorteile des einmaligen Anmeldens nutzen zu können. Sicherheitsschlüssel funktionieren vorerst jedoch nur bei mit Azure Active Directory verknüpften Computern. Es wird empfohlen, nur FIDO2-Sicherheitsschlüssel für den Windows-Sperrbildschirm auf mit Azure Active Directory verknüpften Computern zu testen. Diese Einschränkung gilt nicht für das Web.

### <a name="upn-changes"></a>Änderungen des Benutzerprinzipalnamens

Wir arbeiten derzeit an einem Feature, das eine Änderung des Benutzerprinzipalnamens auf hybriden AADJ- und AADJ-Geräten ermöglicht. Wenn der Benutzerprinzipalname eines Benutzers geändert wird, können Sie FIDO2-Sicherheitsschlüssel für dieses Konto nicht mehr ändern. Dann kann lediglich das Gerät zurückgesetzt werden, und der Benutzer muss dieses nochmals registrieren.

## <a name="next-steps"></a>Nächste Schritte

[Informationen zur Geräteregistrierung](../devices/overview.md)

[Informationen zu Microsoft Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
