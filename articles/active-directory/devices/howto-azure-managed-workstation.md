---
title: Bereitstellen von Azure-verwalteten Arbeitsstationen – Azure Active Directory
description: Erfahren Sie, wie Sie sichere, von Azure verwaltete Arbeitsstationen bereitstellen, um das Risiko von Verletzungen aufgrund von Fehlkonfigurationen oder Kompromissen zu reduzieren.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51d8bbc8b8be9679fbf024d7c51de53c430dc493
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67550488"
---
# <a name="deploy-a-secure-azure-managed-workstation"></a>Stellen Sie eine sichere, von Azure verwaltete Arbeitsstation bereit

Jetzt, da Sie [sichere Arbeitsstationen verstehen](concept-azure-managed-workstation.md), ist es an der Zeit, den Prozess der Bereitstellung zu beginnen. Mit dieser Anleitung verwenden Sie definierte Profile, um eine Arbeitsstation zu erstellen, die von Anfang an sicherer ist.

![Bereitstellen einer sicheren Arbeitsstation](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

Sie müssen ein Profil auswählen, bevor Sie die Lösung bereitstellen können. Sie können mehrere Profile gleichzeitig in einer Bereitstellung verwenden und sie mit Tags oder Gruppen zuzuweisen.
> [!NOTE]
> Wenden Sie eines der Profile nach Ihren Anforderungen an. Sie können zu einem anderen Profil wechseln, indem Sie es in Intune zuweisen.

| Profil | Niedrig | Verbessert | Hoch | Spezialisiert | Geschützt | Isolated |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Benutzer in Azure AD | Ja | Ja | Ja | Ja | Ja | Ja |
| Mit Intune verwaltete | Ja | Ja | Ja | Ja | Ja | Ja |
| Gerät bei Azure AD registriert | Ja |  |  |  |  | |   |
| Gerät in Azure AD eingebunden |   | Ja | Ja | Ja | Ja | Ja |
| Intune-Sicherheitsbaseline angewandt |   | Ja <br> (Erweitert) | Ja <br> (HighSecurity) | Ja <br> (NCSC) | Ja <br> (Geschützt) |  Nicht verfügbar |
| Hardware erfüllt sichere Windows 10-Standards |   | Ja | Ja | Ja | Ja | Ja |
| Microsoft Defender ATP aktiviert |   | Ja  | Ja | Ja | Ja | Ja |
| Entfernen von Administratorrechten |   |   | Ja  | Ja | Ja | Ja |
| Bereitstellung mithilfe von Microsoft Autopilot |   |   | Ja  | Ja | Ja | Ja |
| Apps nur von Intune installiert |   |   |   | Ja | Ja |Ja |
| URLs auf genehmigte Liste beschränkt |   |   |   | Ja | Ja |Ja |
| Internet (eingehend/ausgehend blockiert) |   |   |   |  |  |Ja |

## <a name="license-requirements"></a>Lizenzanforderungen

Die in diesem Handbuch erörterten Konzepte setzen Microsoft 365 Enterprise E5 oder eine entsprechende SKU voraus. Einige der Empfehlungen in diesem Handbuch können mit niedrigeren SKUs implementiert werden. Weitere Informationen finden Sie unter [Microsoft 365 Enterprise-Lizenzierung](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise).

Um die Lizenzbereitstellung zu automatisieren, sollten Sie eine[ gruppenbasierte Lizenzierung](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) für Ihre Benutzer in Betracht ziehen.

## <a name="azure-active-directory-configuration"></a>Azure Active Directory-Konfiguration

Azure Active Directory (Azure AD) verwaltet Benutzer, Gruppen und Geräte für Ihre Administrator-Arbeitsstationen. Sie müssen die Identitätsdienste und Features mit einem[Administratorkonto](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) aktivieren.

Wenn Sie das gesicherte Arbeitsstation-Administratorkonto erstellen, stellen Sie das Konto für Ihren aktuellen Arbeitsplatz zur Verfügung. Stellen Sie sicher, dass Sie für diese Erstkonfiguration und die gesamte globale Konfiguration ein bekanntes sicheres Gerät verwenden. Um die Angriffe bei der ersten Nutzung zu reduzieren, sollten Sie die [Hinweise zur Vermeidung von Malware-Infektionen beachten](https://docs.microsoft.com/windows/security/threat-protection/intelligence/prevent-malware-infection).

Sie sollten auch eine mehrstufige Authentifizierung anfordern, zumindest für Ihre Administratoren. Einen Implementierungsleitfaden finden Sie unter [Planen einer cloudbasierten Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

### <a name="azure-ad-users-and-groups"></a>Azure AD-Benutzer und -Gruppen

1. Navigieren Sie im Azure-Portal zu **Azure Active Directory** > **Benutzer** > **Neuer Benutzer**.
1. Erstellen Sie Ihren Geräteadministrator, indem Sie den Schritten im [Tutorial Benutzer erstellen](https://docs.microsoft.com/Intune/quickstart-create-user) folgen.
1. Geben Sie Folgendes ein:
   * **Name**: Administrator der sicheren Arbeitsstation
   * **Benutzername** - `secure-ws-admin@identityitpro.com`
   * **Verzeichnisrolle** - **Eingeschränkter Administrator** und wählen Sie die Rolle **Intune Administrator** aus.
1. Klicken Sie auf **Erstellen**.

Als nächstes erstellen Sie zwei Gruppen: Arbeitsstationbenutzer und Arbeitsstationgeräte.

Navigieren Sie vom Azure-Portal aus zu **Azure Active Directory** > **Gruppen** > **Neue Gruppe**.

1. Für die Arbeitsstation-Benutzergruppe können Sie eine [gruppenbasierte Lizenzierung](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) konfigurieren, um die Bereitstellung von Lizenzen für Benutzer zu automatisieren.
1. Geben Sie für die Gruppe der Arbeitsstation-Benutzer Folgendes ein:
   * **Gruppentyp**: Sicherheit
   * **Gruppenname**: Benutzer der sicheren Arbeitsstation
   * **Mitgliedschaftstyp**: Zugewiesen
1. Fügen Sie Ihren sicheren Administrator-Benutzer für die Arbeitsstation hinzu: `secure-ws-admin@identityitpro.com`
1. Sie können alle anderen Benutzer hinzufügen, die sichere Arbeitsstationen verwalten.
1. Klicken Sie auf **Erstellen**.

1. Geben Sie für die Gruppe der Arbeitsstation-Benutzer Folgendes ein:
   * **Gruppentyp**: Sicherheit
   * **Gruppenname**: Sichere Arbeitsstationen
   * **Mitgliedschaftstyp**: Zugewiesen
1. Klicken Sie auf **Erstellen**.

### <a name="azure-ad-device-configuration"></a>Azure AD-Gerätekonfiguration

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Angeben, wer Geräte in Azure AD einbinden darf

Konfigurieren Sie Ihre Geräteeinstellungen in Active Directory, damit Ihre administrative Sicherheitsgruppe die Geräte mit Ihrer Domäne verbinden kann. So konfigurieren Sie diese Einstellung über das Azure-Portal:

1. Gehen Sie zu **Azure Active Directory** > **Geräte** > **Geräteeinstellungen**.
1. Wählen Sie **Ausgewählte** unter **Benutzer können Geräte mit Azure AD verbinden**, und wählen Sie dann die Gruppe „Secure Workstation Users“ aus.

#### <a name="removal-of-local-admin-rights"></a>Entfernen von lokalen Administratorrechten

Diese Methode erfordert, dass Benutzer der Arbeitsstationen VIP, DevOps und Secure-Level keine Administratorrechte auf ihren Computern haben. So konfigurieren Sie diese Einstellung über das Azure-Portal:

1. Gehen Sie zu **Azure Active Directory** > **Geräte** > **Geräteeinstellungen**.
1. Wählen Sie unter **Weitere lokale Administratoren für in Azure AD eingebundene Geräte** die Option **Keine** aus.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Voraussetzen der Multi-Factor Authentication für das Einbinden von Geräten

Um den Prozess der Verbindung von Geräten mit Azure AD weiter zu stärken:

1. Gehen Sie zu **Azure Active Directory** > **Geräte** > **Geräteeinstellungen**.
1. Wählen Sie **Ja** unter **Multi-Faktor-Authentifizierung erforderlich, um Geräte zu verbinden**.
1. Wählen Sie **Speichern** aus.

#### <a name="configure-mdm"></a>Konfigurieren von MDM

Gehen Sie im Azure-Portal so vor:

1. Navigieren Sie zu **Azure Active Directory** > **Mobility (MDM und MAM)**  > **Microsoft Intune**.
1. Ändern Sie die Einstellung des **MDM-Benutzerbereichs** auf **Alle**.
1. Wählen Sie **Speichern** aus.

Mit diesen Schritten können Sie jedes Gerät mit Intune verwalten. Weitere Informationen finden Sie unter [Intune Schnellstart: Einrichten der automatischen Registrierung für Windows 10-Geräte](https://docs.microsoft.com/Intune/quickstart-setup-auto-enrollment). Sie erstellen in einem späteren Schritt Intune-Konfigurations- und Compliance-Richtlinien.

#### <a name="azure-ad-conditional-access"></a>Azure AD Conditional Access

Azure AD Conditional Access kann helfen, privilegierte administrative Aufgaben auf konforme Geräte zu beschränken. Vordefinierte Mitglieder der Gruppe **Secure Workstation Users** müssen bei der Anmeldung an Cloud-Anwendungen eine mehrstufige Authentifizierung durchführen. Eine bewährte Vorgehensweise ist es, Notfallkonten von der Richtlinie auszunehmen. Weitere Informationen finden Sie unter [Verwalten von Administratorkonten für den Notfallzugriff in Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access).

So konfigurieren Sie Conditional Access über das Azure-Portal:

1. Gehen Sie zu **Azure Active Directory** > **Conditional Access** > **Neue Richtlinie**.
1. Geben Sie Folgendes ein:
   * **Namen**: Erforderliche Richtlinie für sicheres Gerät
   * Arbeitsaufträge
     * **Benutzer und Gruppen**
       * Einschließen – **Benutzer und Gruppen** – Wählen Sie die zuvor erstellte Gruppe **Secure Workstation Users** aus.
       * Ausschließen – **Benutzer und Gruppen** – Wählen Sie die Notfallkonten Ihres Unternehmens aus.
     * **Cloud-Apps** – **Alle Cloud-Apps** einbeziehen.
    * Zugriffssteuerung
      * **Erteilen** – Wählen Sie **Zugriff erteilen** Radioschaltfläche.
        * **Mehrstufige Authentifizierung erforderlich**.
        * **Markieren des Geräts als kompatibel erforderlich**.
        * Für mehrere Steuerelemente: **Alle ausgewählten Steuerungen anfordern**.
    * Richtlinie aktivieren: **Ein**.

Sie haben die Möglichkeit, Richtlinien zu erstellen, die Länder blockieren, in denen Benutzer nicht auf Unternehmensressourcen zugreifen würden. Weitere Informationen zu IP-Standort basierten Richtlinien für bedingten Zugriff finden Sie in [der Standortbedingung in Azure Active Directory Conditional Access](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition).

## <a name="intune-configuration"></a>Intune-Konfiguration

### <a name="configure-enrollment-status"></a>Konfigurieren des Registrierungsstatus

Es ist wichtig, sicherzustellen, dass Ihre sichere Arbeitsstation ein vertrauenswürdiges, sauberes Gerät ist. Beim Kauf neuer Geräte können Sie darauf bestehen, dass diese werkseitig auf [Windows 10 Pro im S-Modus](https://docs.microsoft.com/Windows/deployment/Windows-10-pro-in-s-mode) eingestellt sind, was die Gefährdung durch Schwachstellen beim Supply Chain Management begrenzt. Nachdem Sie ein Gerät von Ihrem Lieferanten erhalten haben, können Sie es mit Autopilot aus dem S-Modus umstellen. Die folgende Anleitung enthält Informationen zum Anwenden des Transformationsprozesses.

Um sicherzustellen, dass die Geräte vor der Verwendung vollständig konfiguriert sind, bietet Intune eine Möglichkeit, die **Geräteverwendung zu blockieren, bis alle Anwendungen und Profile installiert sind**.

Über das **Azure-Portal**:
1. Gehen Sie zu **Microsoft Intune** > **Device Enrollment** > **Windows Enrollment** > **Enrollment Status Page** > **Default**  > **Settings**.
1. Setzen Sie **Installationsfortschritt für Apps und Profile anzeigen** auf **Ja**.
1. Setzen Sie **Geräteverwendung blockieren, bis alle Apps und Profile installiert sind** auf **Ja**.

### <a name="create-an-autopilot-deployment-profile"></a>Erstellen eines Autopilot-Bereitstellungsprofils

Nachdem Sie eine Gerätegruppe erstellt haben, müssen Sie ein Bereitstellungsprofil erstellen, um die Autopilot-Geräte zu konfigurieren.

In Intune im Azure-Portal:

1. Wählen Sie in Intune im Azure-Portal **Geräteregistrierung** > **Windows-Registrierung** > **Bereitstellungsprofile** > **Profil erstellen** aus.
1. Geben Sie Folgendes ein:
   * Name – **Sicheres Bereitstellungsprofil für Arbeitsstation**.
   * Beschreibung – **Bereitstellung von sicheren Arbeitsstationen**.
   * **Alle Zielgeräte in Autopilot umwandeln** auf **Ja** setzen. Diese Einstellung stellt sicher, dass alle Geräte in der Liste mit dem Autopilot-Bereitstellungsdienst registriert werden. Lassen Sie 48 Stunden zur Verarbeitung der Registrierung verstreichen.
1. Klicken Sie auf **Weiter**.
   * Wählen Sie im **Bereitstellungsmodus** **Self-Deploying (Preview)** . Geräte mit diesem Profil sind dem Benutzer zugeordnet, der das Gerät registriert. Anmeldeinformationen sind erforderlich, um das Gerät zu registrieren.
   * Das Feld **Join to Azure AD as** sollte anzeigen, dass **Azure AD joined** ist und ausgegraut ist.
   * Wählen Sie Ihre Sprache (Region), Benutzerkonto-Typ **Standard**. 
1. Klicken Sie auf **Weiter**.
   * Wählen Sie eine Bereichsmarkierung, wenn Sie eine Vorkonfigurierte haben.
1. Klicken Sie auf **Weiter**.
1. Wählen Sie **Zuordnungen** >  **Zuordnung zu** >  **ausgewählten Gruppen**. Wählen Sie unter **Zu integrierende Gruppen** die Option **Sichere Arbeitsstation-Benutzer**.
1. Klicken Sie auf **Weiter**.
1. Wählen Sie **Erstellen**, um das Profil zu erstellen. Das Autopilot-Bereitstellungsprofil steht nun für die Zuweisung von Geräten zur Verfügung.

### <a name="configure-windows-update"></a>Konfigurieren des Windows-Updates

Windows 10 auf dem neuesten Stand zu halten, ist eines der wichtigsten Dinge, die Sie tun können. Um Windows in einem sicheren Zustand zu halten, setzen Sie einen Aktualisierungsring ein, um die Geschwindigkeit zu steuern, mit der Updates auf die Arbeitsstationen angewendet werden. 

Diese Anleitung empfiehlt, einen neuen Updatering zu erstellen und die folgenden Standardeinstellungen zu ändern:

Führen Sie im Azure-Portal die folgenden Schritte aus:

1. Wechseln Sie zu **Microsoft Intune** > **Softwareupdates** > **Windows 10-Updateringe**.
1. Geben Sie Folgendes ein:
   * Name: **Updates für verwaltete Azure-Arbeitsstationen**
   * Wartungskanal: **Windows-Insider: schnell**
   * Rückstellungszeitraum für Qualitätsupdates (Tage): **3**
   * Rückstellungszeitraum für Funktionsupdates (Tage): **3**
   * Automatisches Updateverhalten: **Ohne Endbenutzersteuerung automatisch installieren und neu starten**
   * Anhalten von Windows-Updates durch Benutzer blockieren: **Blockieren**
   * Genehmigung des Benutzers für Neustart außerhalb der Arbeitszeiten erforderlich: **Erforderlich**
   * Benutzer (erzwungenen) Neustart ermöglichen: **Erforderlich**
   * Übergang von Benutzern nach einem automatischen Neustart zum erzwungenen Neustart (Tage) : **3**
   * Erinnerung zu engagiertem Neustart zurückstellen (Tage): **3**
   * Stichtag für ausstehende Neustarts festlegen (Tage): **3**

1. Klicken Sie auf **Erstellen**.
1. Fügen Sie auf der Registerkarte **Zuweisungen** die Gruppe **Sichere Arbeitsstationen** hinzu.

Weitere Informationen zu Windows-Update-Richtlinien finden Sie unter [Policy CSP – Update](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-update).

### <a name="windows-defender-atp-intune-integration"></a>Integration von Windows Defender ATP in Intune

Windows Defender ATP und Microsoft Intune verhindern gemeinsam Sicherheitsverletzungen. Sie können auch die Auswirkungen von Sicherheitsverletzungen einschränken. ATP-Funktionen bieten Echtzeit-Bedrohungserkennung und ermöglichen eine umfassende Überprüfung und Protokollierung der Endgeräte.

Um die Integration von Windows Defender ATP und Intune zu konfigurieren, gehen Sie zum Azure-Portal.

1. Navigieren Sie zu **Microsoft Intune** >  **Device Compliance** >  **Windows Defender ATP**.
1. Klicken Sie in Schritt 1 unter **Konfigurieren von Windows Defender ATP** auf **Verbindungsherstellung zwischen Windows Defender ATP und Microsoft Intune über Windows Defender Security Center**.
1. In Windows Defender Security Center:
   1. Wählen Sie **Einstellungen** > **Erweiterte Funktionen**.
   1. Wählen Sie für **Microsoft Intune-Verbindung** die Option **Ein** aus.
   1. Wählen Sie **Einstellungen speichern** aus.
1. Nachdem eine Verbindung hergestellt wurde, kehren Sie zu Intune zurück und wählen Sie oben **Aktualisieren** aus.
1. Setzen Sie **Verbindung von Windows-Geräten mit Version 10.0.15063 und höher mit Windows Defender ATP herstellen** auf **Ein**.
1. Wählen Sie **Speichern** aus.

Weitere Informationen finden Sie unter [Windows Defender Advanced Threat Protection](https://docs.microsoft.com/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection).

### <a name="finish-workstation-profile-hardening"></a>Härten von Arbeitsstationen-Profil fertigstellen

Um die Härtung der Lösung erfolgreich abzuschließen, laden Sie das entsprechende Skript herunter und führen Sie es aus. Hier finden Sie die Download-Links für Ihre gewünschte **Profilebene**:

| Profil | Downloadspeicherort | Dateiname |
| --- | --- | --- |
| Niedrige Sicherheit | – |  – |
| Erweiterte Sicherheit | https://aka.ms/securedworkstationgit | Enhanced-Workstation-Windows10-(1809).ps1 |
| Hohe Sicherheit  | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809).ps1 |
| Spezialisiert | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC – Windows10 (1803) SecurityBaseline.ps1 |
| Spezielle Compliance * | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10(1803).ps1 |
| Geschützt | https://aka.ms/securedworkstationgit | Secure-Workstation-Windows10-(1809)-SecurityBaseline.ps1 |

\* Specialized Compliance ist ein Skript, das die in der NCSC Windows10 SecurityBaseline bereitgestellte Specialized-Konfiguration erzwingt.

Nachdem das Skript erfolgreich ausgeführt wurde, können Sie Updates von Profilen und Richtlinien in Intune vornehmen. Die Skripte für erweiterte und sichere Profile erstellen Richtlinien und Profile für Sie, aber Sie müssen die Richtlinie Ihrer Gruppe **Secure Workstations** zuweisen.

* Hier finden Sie die von den Skripten erstellten Intune-Gerätekonfigurationsprofile: **Azure-Portal** > **Microsoft Intune** > **Gerätekonfiguration** > **Profile**.
* Hier finden Sie die von den Skripten erstellten Intune Device Compliance-Richtlinien: **Azure-Portal** > **Microsoft Intune** > **Geräte-Compliance** > **Profile**.

Um die von den Skripten vorgenommenen Änderungen zu überprüfen, können Sie die Profile exportieren. Auf diese Weise können Sie zusätzliche Härtungen bestimmen, die gemäß der SECCON-Dokumentation erforderlich sein können.

Führen Sie das Intune-Datenexportskript `DeviceConfiguration_Export.ps1` aus dem [DeviceConfiguration GiuHub-Repository](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) aus, um alle aktuellen Intune-Profile zu exportieren.

## <a name="additional-configurations-and-hardening-to-consider"></a>Zu berücksichtigende zusätzliche Konfigurationen und Härtung

Wenn Sie den Anweisungen hier folgen, haben Sie eine sichere Arbeitsstation eingerichtet. Allerdings sollten Sie auch zusätzliche Steuerelemente in Betracht ziehen. Beispiel:

* Beschränken des Zugriffs auf andere Browser
* Ausgehende HTTP zulassen
* Ausgewählte Websites blockieren
* Festlegen von Berechtigungen für die Ausführung benutzerdefinierter PowerShell-Skripte

### <a name="set-rules-in-the-firewall-configuration-service-provider-csp"></a>Legen Sie die Regeln in der Firewall-Konfigurationsdienstanbieter fest (CSP)

Sie können bei Bedarf zusätzliche Änderungen an der Verwaltung der Eingangs- und Ausgangsregeln für Ihre zulässigen und gesperrten Endpunkte vornehmen. Während Sie die sichere Arbeitsstation weiter härten, können Sie die Einschränkung aufheben, die den gesamten ein- und ausgehenden Datenverkehr unterbindet. Sie können zulässige ausgehende Websites hinzufügen, um gemeinsame und vertrauenswürdige Websites einzubeziehen. Weitere Informationen finden Sie unter [Firewall-Konfigurationsdienst](https://docs.microsoft.com/Windows/client-management/mdm/firewall-csp).

Standardempfehlungen zu Einschränkungen sind:

* Allen Eingang verweigern
* Allen Ausgang verweigern

Das Spamhaus-Projekt unterhält [die Domain Block List (DBL)](https://www.spamhaus.org/dbl/): eine gute Ressource, die als Ausgangspunkt für das Blockieren von Websites dienen kann.

### <a name="manage-local-applications"></a>Verwalten von lokalen Anwendungen

Die sichere Arbeitsstation wechselt in einen wirklich gehärteten Zustand, wenn lokale Anwendungen entfernt werden, einschließlich Produktivitätsanwendungen. Hier fügen Sie Chrome als Standardbrowser hinzu und verwenden Intune, um die Möglichkeit eines Benutzers einzuschränken, den Browser und seine Plugins zu ändern.

#### <a name="deploy-applications-using-intune"></a>Bereitstellen von Anwendungen mit Intune

In einigen Fällen sind Anwendungen wie der Google Chrome-Browser auf der geschützten Arbeitsstation erforderlich. Das folgende Beispiel enthält Anweisungen zur Installation von Chrome auf Geräten der Sicherheitsgruppe **Secure Workstations**.

1. Laden Sie das Offlineinstallationsprogramm [Chrome-Paket für Windows-64-Bit](https://cloud.google.com/chrome-enterprise/browser/download/) herunter.
1. Entpacken Sie die Dateien und notieren Sie sich den Speicherort der Datei `GoogleChromeStandaloneEnterprise64.msi`.
1. Navigieren Sie im **Azure-Portal** zu **Microsoft Intune** > **Clientanwendungen** > **Apps** > **Hinzufügen**.
1. Wählen Sie unter **App-Typ** die Option **Branchenspezifisch** aus.
1. Wählen Sie unter **App-Paketdatei** die Datei `GoogleChromeStandaloneEnterprise64.msi` am extrahierten Speicherort aus, und klicken Sie auf **OK**.
1. Geben Sie unter **App-Informationen** eine Beschreibung und einen Publisher an. Klicken Sie auf **OK**.
1. Wählen Sie **Hinzufügen**.
1. Wählen Sie auf der Registerkarte **Zuordnungen** unter **Zuordnungsart** die Option **Verfügbar für registrierte Geräte**.
1. Fügen Sie unter **Included Groups** die Gruppe **Secure Workstation** hinzu.
1. Wählen Sie **OK** und anschließend **Speichern** aus.

Weitere Informationen zur Konfiguration von Chrome-Einstellungen finden Sie unter [Verwalten des Chrome-Browsers mit Microsoft Intune](https://support.google.com/chrome/a/answer/9102677).

#### <a name="configuring-the-company-portal-for-custom-apps"></a>Konfigurieren des Unternehmensportals für benutzerdefinierte Apps

In einem gesicherten Modus ist die Anwendungsinstallation auf das Intune Firmenportal beschränkt. Das Installieren des Portals erfordert jedoch Zugriff auf den Microsoft Store. In Ihrer gesicherten Lösung können Sie das Unternehmensportal über einen Offline-Modus für alle Geräte verfügbar machen.

Eine Intune-verwaltete Kopie des [Firmenportals](https://docs.microsoft.com/Intune/store-apps-company-portal-app) bietet Ihnen bei Bedarf Zugriff auf zusätzliche Tools, die Sie an die Benutzer der gesicherten Arbeitsstationen weiterleiten können.

Möglicherweise müssen Sie Windows 32-Bit-Anwendungen oder andere Anwendungen installieren, deren Bereitstellung spezielle Vorbereitungen erfordert. In solchen Fällen kann das [Microsoft win32 Content Prep Tool](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) eine gebrauchsfertige`.intunewin` Formatdatei für die Installation bereitstellen.

### <a name="use-powershell-to-create-custom-settings"></a>Verwenden von PowerShell zum Erstellen von benutzerdefinierten Einstellungen

Sie können auch PowerShell verwenden, um Host Verwaltungsfunktionen zu erweitern. Das Beispielskript richtet einen Standardhintergrund auf dem Host ein. Es handelt sich um eine Funktion, die auch über das Azure-Portal und die Profile verfügbar ist. Das Beispielskript stellt lediglich die PowerShell-Funktionen zur Veranschaulichung bereit.

Möglicherweise müssen Sie einige benutzerdefinierte Kontrollen und Einstellungen auf Ihren sicheren Arbeitsstationen einrichten. Dieses Beispiel ändert den Hintergrund der Arbeitsstation, indem es die Fähigkeit von PowerShell nutzt, das Gerät einfach als gebrauchsfertige, sichere Arbeitsstation zu identifizieren.

Das Skript [SetDesktopBackground.ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) aus dem Microsoft Scripting Center ermöglicht es Windows, dieses [kostenlose, generische Hintergrundbild](https://i.imgur.com/OAJ28zO.png) beim Start zu laden.

1. Laden Sie das Skript auf ein lokales Gerät herunter.
1. Aktualisieren Sie den KundenXXXXXX und den Download-Ort des Hintergrundbildes. In unserem Beispiel ersetzen wir „customerXXXX“ mit „Hintergründe“.  
1. Navigieren Sie zum **Azure-Portal** > **Microsoft Intune** > **Gerätekonfiguration** > **PowerShell-Skripte** > **Hinzufügen**.
1. Geben Sie einen **Namen** für das Skript sowie den **Skript-Speicherort** an.
1. Wählen Sie **Konfigurieren**aus.
   1. Setzen Sie **Ausführen dieses Skripts mit den Anmeldedaten** auf **Ja**.
   1. Klicken Sie auf **OK**.
1. Klicken Sie auf **Erstellen**.
1. Wählen Sie **Zuordnungen** > **Gruppen auswählen** aus.
   1. Fügen Sie die Sicherheitsgruppe **sichere Arbeitsstationen** hinzu.
   1. Wählen Sie **Speichern** aus.

## <a name="enroll-and-validate-your-first-device"></a>Registrieren und Überprüfen Ihres ersten Geräts

1. Zum Registrieren Ihres Geräts benötigen Sie die folgenden Informationen:
   * **Seriennummer** – befindet sich auf dem Gerätegehäuse.
   * **Windows-Produkt-ID**: unter **System** > **Info** im Menü „Windows-Einstellungen“.
   * Sie können [Get-WindowsAutoPilotInfo](https://aka.ms/Autopilotshell) ausführen, um eine CSV-Hash-Datei mit allen erforderlichen Informationen für die Geräteregistrierung zu erhalten.
   
     Führen Sie `Get-WindowsAutoPilotInfo – outputfile device1.csv` aus, um die Informationen als CSV-Datei auszugeben, die in Intune importiert werden können.

     > [!NOTE]
     > Das Skript erfordert erhöhte Rechte. Ausführung als remote signiert. Der `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned` Befehl ermöglicht die korrekte Ausführung des Skripts.

   * Sie können diese Informationen sammeln, indem Sie sich bei einem Gerät der Version 1809 oder höher von Windows 10 anmelden. Diese Informationen können auch von Ihrem Hardware-Händler zur Verfügung gestellt werden.
1. Gehen Sie im **Azure-Portal** zu **Microsoft Intune** >  **Device Enrollment** >  **Windows Enrollment** >  **Geräte – Windows Autopilot Geräte verwalten**.
1. Wählen Sie **Importieren** und wählen Sie Ihre CSV-Datei aus.
1. Fügen Sie das Gerät der Sicherheitsgruppe **Secure Workstations** hinzu.
1. Gehen Sie auf dem Windows 10-Gerät, das Sie konfigurieren möchten, zu **Windows Einstellungen** > **Update & Sicherheit** >  **Wiederherstellung**.
   1. Wählen Sie **Anfangen** unter **Diesen PC zurücksetzen**.
   1. Folgen Sie den Anweisungen, um das Gerät mit den konfigurierten Profil- und Compliance-Richtlinien zurückzusetzen und neu zu konfigurieren.

Nachdem Sie das Gerät konfiguriert haben, schließen Sie eine Überprüfung ab und überprüfen Sie die Konfiguration. Vergewissern Sie sich, dass das erste Gerät korrekt konfiguriert ist, bevor Sie mit der Bereitstellung fortfahren.

## <a name="assign-and-monitor"></a>Zuweisen und Überwachen

Um Geräte und Benutzer zuzuordnen, müssen Sie die [ausgewählten Profile](https://docs.microsoft.com/intune/device-profile-assign) Ihrer Sicherheitsgruppe zuordnen. Alle neuen Benutzer, die Berechtigungen für den Dienst benötigen, müssen ebenfalls der Sicherheitsgruppe hinzugefügt werden.

Sie können Profile mit der [Intune Profilüberwachung](https://docs.microsoft.com/intune/device-profile-monitor) überwachen.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Microsoft Intune](https://docs.microsoft.com/intune/index).
* Verstehen Sie [Azure AD](https://docs.microsoft.com/azure/active-directory/index).
