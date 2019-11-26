---
title: Self-Service-Kennwortzurücksetzung in Azure AD für Windows – Azure Active Directory
description: Aktivieren der Self-Service-Kennwortzurücksetzung mithilfe von „Kennwort vergessen“ auf dem Windows-Anmeldebildschirm
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 10/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 519993be873e7864dab4de4f66919c56aebfc379
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73171871"
---
# <a name="how-to-enable-password-reset-from-the-windows-login-screen"></a>Gewusst wie: Aktivieren der Kennwortzurücksetzung über den Windows-Anmeldebildschirm

Für Computer, auf denen Windows 7, 8, 8.1 oder 10 ausgeführt wird, können Sie es Benutzern ermöglichen, ihr Kennwort auf dem Windows-Anmeldebildschirm zurückzusetzen. Die Benutzer müssen nicht mehr ein Gerät mit einem Webbrowser suchen, um auf das [SSPR-Portal](https://aka.ms/sspr) zuzugreifen.

![Beispiel für Windows 7- und Windows 10-Anmeldebildschirm mit angezeigtem SSPR-Link](./media/howto-sspr-windows/windows-reset-password.png)

## <a name="general-limitations"></a>Allgemeine Einschränkungen

- Die Kennwortzurücksetzung wird für Remotedesktop oder über erweiterte Hyper-V-Sitzungen derzeit nicht unterstützt.
- Dieses Feature funktioniert nicht für Netzwerke mit 802.1x-Netzwerkauthentifizierung und der Option „Unmittelbar vor der Benutzeranmeldung ausführen“. In Netzwerken mit 802.1x-Netzwerkauthentifizierung empfiehlt es sich, die Computerauthentifizierung zu verwenden, um dieses Feature zu aktivieren.
- In Hybrid Azure AD eingebundene Computer müssen über eine direkte Netzwerkverbindung mit einem Domänencontroller verfügen, um das neue Kennwort verwenden und zwischengespeicherte Anmeldeinformationen aktualisieren zu können.
- Wenn Sie ein Image verwenden, stellen Sie vor dem Ausführen von Sysprep sicher, dass der Webcache für den integrierten Administrator vor der Durchführung des CopyProfile-Schritts gelöscht ist. Weitere Informationen zu diesem Schritt finden Sie im Supportartikel [Schlechte Leistung bei Verwendung eines benutzerdefinierten Standardbenutzerprofils](https://support.microsoft.com/help/4056823/performance-issue-with-custom-default-user-profile).
- Bei den folgenden Einstellungen ist bekannt, dass sie die Möglichkeit zum Verwenden und Zurücksetzen von Kennwörtern auf Windows 10-Geräten beeinträchtigen:
    - Wenn in Windows 10-Versionen vor v1809 die Richtlinie STRG+ALT+ENTF vorschreibt, funktioniert **Kennwort zurücksetzen** nicht.
    - Wenn Benachrichtigungen bei gesperrtem Bildschirm deaktiviert sind, funktioniert **Kennwort zurücksetzen** nicht.
    - „HideFastUserSwitching“ ist auf „Aktiviert“ oder „1“ festgelegt.
    - „DontDisplayLastUserName“ ist auf „Aktiviert“ oder auf „1“ festgelegt.
    - „NoLockScreen“ ist auf „Aktiviert“ oder „1“ festgelegt.
    - „EnableLostMode“ ist auf dem Gerät festgelegt.
    - „Explorer.exe“ wird durch eine benutzerdefinierte Shell ersetzt.
- Die Kombination der folgenden drei Einstellungen kann dazu führen, dass dieses Feature fehlschlägt.
    - Interaktive Anmeldung: Kein STRG+ALT+ENTF erforderlich = Deaktiviert
    - DisableLockScreenAppNotifications = 1 oder „Aktiviert“
    - IsContentDeliveryPolicyEnforced = 1 oder „True“

## <a name="windows-10-password-reset"></a>Windows 10-Kennwortzurücksetzung

### <a name="windows-10-prerequisites"></a>Voraussetzungen für Windows 10

- Ein Administrator muss die Self-Service-Kennwortzurücksetzung in Azure AD über das Azure-Portal aktivieren.
- **Benutzer müssen sich für SSPR registrieren, bevor sie diese Funktion verwenden können.**
- Anforderungen an Netzwerkproxy
   - Windows 10-Geräte 
       - Port 443 für `passwordreset.microsoftonline.com` und `ajax.aspnetcdn.com`
       - Windows 10-Geräte unterstützen nur die Proxykonfiguration auf Computerebene.
- Es muss mindestens Windows 10 mit dem Update vom April 2018 (v1803) ausgeführt werden, und die Geräte müssen eine der folgenden Voraussetzungen erfüllen:
    - In Azure AD eingebunden
    - Hybrid in Azure AD eingebunden

### <a name="enable-for-windows-10-using-intune"></a>Aktivieren für Windows 10 mithilfe von Intune

Die Bereitstellung der Konfigurationsänderung zum Aktivieren der Kennwortzurücksetzung über den Anmeldebildschirm mithilfe von Intune ist die flexibelste Methode. Mit Intune können Sie die Konfigurationsänderung für eine bestimmte Gruppe von Computern bereitstellen, die Sie definieren. Diese Methode erfordert die Registrierung des Gerätes über Intune.

#### <a name="create-a-device-configuration-policy-in-intune"></a>Erstellen einer Richtlinie für die Gerätekonfiguration in Intune

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Intune**.
1. Erstellen Sie ein neues Profil für die Gerätekonfiguration, indem Sie zu **Gerätekonfiguration** > **Profile** > **Profil erstellen** navigieren.
   - Geben Sie einen aussagekräftigen Namen für das Profil an.
   - Geben Sie optional eine aussagekräftige Beschreibung des Profils an.
   - Plattform **Windows 10 und höher**
   - Profiltyp **Benutzerdefiniert**
1. Konfigurieren von **Einstellungen**
   - Fügen Sie mit **Hinzufügen** die folgende OMA-URI-Einstellung hinzu, um den Link „Kennwort zurücksetzen“ zu aktivieren.
      - Geben Sie einen aussagekräftigen Namen an, um den Zweck der Einstellung zu verdeutlichen.
      - Geben Sie optional eine aussagekräftige Beschreibung der Einstellung an.
      - Festlegung von **OMA-URI** auf `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset`
      - Festlegung von **Data type** auf **Integer**
      - Festlegung von **Value** auf **1**
      - Klicken Sie auf **OK**
   - Klicken Sie auf **OK**
1. Klicken Sie auf **Erstellen**
1. Diese Richtlinie kann bestimmten Benutzern, Geräten oder Gruppen zugewiesen werden. Weitere Informationen finden Sie im Artikel [Zuweisen von Benutzer- und Geräteprofilen in Microsoft Intune](https://docs.microsoft.com/intune/device-profile-assign).

### <a name="enable-for-windows-10-using-the-registry"></a>Aktivieren für Windows 10 mithilfe der Registrierung

1. Melden Sie sich mit Administratoranmeldeinformationen beim Windows-PC an.
1. Führen Sie den Befehl **regedit** als Administrator aus.
1. Legen Sie den folgenden Registrierungsschlüssel fest:
   - `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      - `"AllowPasswordReset"=dword:00000001`

#### <a name="troubleshooting-windows-10-password-reset"></a>Behandeln von Problemen bei der Windows 10-Kennwortzurücksetzung

Das Azure AD-Überwachungsprotokoll enthält Informationen zur IP-Adresse und zum Clienttyp, für die das Kennwort zurückgesetzt wurde.

![Beispiel: Kennwortzurücksetzung unter Windows 7 im Azure AD-Überwachungsprotokoll](media/howto-sspr-windows/windows-7-sspr-azure-ad-audit-log.png)

Wenn Benutzer ihr Kennwort über den Anmeldebildschirm eines Windows 10-Geräts zurücksetzen, wird ein temporäres Konto mit niedrigen Berechtigungen mit dem Namen `defaultuser1` erstellt. Mit diesem Konto wird der Vorgang zur Kennwortzurücksetzung geschützt. Für das Konto selbst ist ein zufällig generiertes Kennwort festgelegt. Das Konto wird außerdem nicht für die Geräteanmeldung angezeigt und wird nach dem Zurücksetzen des Kennworts durch den Benutzer automatisch entfernt. Unter Umständen sind mehrere Profile vom Typ `defaultuser` vorhanden, sie können jedoch ignoriert werden.

## <a name="windows-7-8-and-81-password-reset"></a>Kennwortzurücksetzung für Windows 7, 8 und 8.1

### <a name="windows-7-8-and-81-prerequisites"></a>Voraussetzungen für Windows 7, 8 und 8.1

- Ein Administrator muss die Self-Service-Kennwortzurücksetzung in Azure AD über das Azure-Portal aktivieren.
- **Benutzer müssen sich für SSPR registrieren, bevor sie diese Funktion verwenden können.**
- Anforderungen an Netzwerkproxy
   - Geräte mit Windows 7, 8 und 8.1
       - Port 443 für `passwordreset.microsoftonline.com`
- Gepatchtes Windows 7- oder Windows 8.1-Betriebssystem.
- TLS 1.2 wurde gemäß der Anleitung unter [Registrierungseinstellungen für Transport Layer Security (TLS)](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12) aktiviert.
- Wenn auf Ihrem Computer mehrere Drittanbieter von Anmeldeinformationen aktiviert sind, sehen Benutzer mehrere Benutzerprofile auf dem Anmeldebildschirm.

> [!WARNING]
> TLS 1.2 muss aktiviert sein. Die Einstellung „Automatische Aushandlung“ reicht nicht aus.

### <a name="install"></a>Installieren

1. Laden Sie das Installationsprogramm für die Windows-Version herunter, die Sie aktivieren möchten.
   - Software ist im Microsoft Download Center unter [https://aka.ms/sspraddin](https://aka.ms/sspraddin) verfügbar.
1. Melden Sie sich auf dem Computer an, auf dem Sie installieren möchten, und führen Sie das Installationsprogramm aus.
1. Ein Neustart nach der Installation wird dringend empfohlen.
1. Wählen Sie nach dem Neustart auf dem Anmeldebildschirm einen Benutzer aus, und klicken Sie auf „Kennwort vergessen?“, um den Workflow zur Kennwortzurücksetzung zu initiieren.
1. Schließen Sie den Workflow mit den folgenden Schritten auf dem Bildschirm ab, um Ihr Kennwort zurückzusetzen.

![Beispiel: Windows 7 mit angeklickter Option „Kennwort vergessen?“ SSPR-Flow](media/howto-sspr-windows/windows-7-sspr.png)

#### <a name="silent-installation"></a>Automatische Installation

- Verwenden Sie für die automatische Installation den Befehl „msiexec /i SsprWindowsLogon.PROD.msi /qn“.
- Verwenden Sie für die automatische Deinstallation den Befehl „msiexec /x SsprWindowsLogon.PROD.msi /qn“.

#### <a name="troubleshooting-windows-7-8-and-81-password-reset"></a>Behandeln von Problemen bei der Kennwortzurücksetzung für Windows 7, 8 und 8.1

Ereignisse werden sowohl auf dem Computer als auch in Azure AD protokolliert. Azure AD-Ereignisse enthalten Informationen zur IP-Adresse und zum Clienttyp, für die das Kennwort zurückgesetzt wurde.

![Beispiel: Kennwortzurücksetzung unter Windows 7 im Azure AD-Überwachungsprotokoll](media/howto-sspr-windows/windows-7-sspr-azure-ad-audit-log.png)

Wenn zusätzliche Protokollierung erforderlich ist, kann ein Registrierungsschlüssel auf dem Computer geändert werden, um die ausführliche Protokollierung zu aktivieren. Aktivieren Sie die ausführliche Protokollierung nur zur Problembehandlung.

`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{86D2F0AC-2171-46CF-9998-4E33B3D7FD4F}`

- Zum Aktivieren der ausführlichen Protokollierung erstellen Sie den Eintrag `REG_DWORD: “EnableLogging”`, und legen Sie ihn auf 1 fest.
- Um die ausführliche Protokollierung zu deaktivieren, ändern Sie `REG_DWORD: “EnableLogging”` in 0.

## <a name="what-do-users-see"></a>Anzeige für Benutzer

Was ändert sich für den Benutzer, nachdem Sie die Kennwortzurücksetzung für die Windows-Geräte konfiguriert haben? Wie erfahren sie, dass sie ihr Kennwort über den Anmeldebildschirm zurücksetzen können?

![Beispiel für Windows 7- und Windows 10-Anmeldebildschirm mit angezeigtem SSPR-Link](./media/howto-sspr-windows/windows-reset-password.png)

Wenn Benutzer versuchen, sich anzumelden, wird jetzt der Link **Kennwort zurücksetzen** oder **Kennwort vergessen** angezeigt. Mit diesen Links wird die Oberfläche für die Self-Service-Kennwortzurücksetzung auf dem Anmeldebildschirm geöffnet. Mit dieser Funktion können Benutzer ihr Kennwort zurücksetzen, ohne ein anderes Gerät für den Zugriff auf einen Webbrowser verwenden zu müssen.

Eine Anleitung für Benutzer zur Verwendung dieses Features befindet sich unter [Ich habe mein Azure AD-Kennwort vergessen. Was nun?](../user-help/active-directory-passwords-update-your-own-password.md).

## <a name="next-steps"></a>Nächste Schritte

[Planen zulässiger Authentifizierungsmethoden](concept-authentication-methods.md)

[Konfigurieren von Windows 10](https://docs.microsoft.com/windows/configuration/)
