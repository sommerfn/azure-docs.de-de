---
title: Konfigurieren der Azure Active Directory-Hybrideinbindung für verwaltete Domänen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Azure Active Directory-Hybrideinbindung für verwaltete Domänen konfigurieren.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 787900918035dc8b14d3a173496ab1a23b0f93bb
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68813090"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Tutorial: Konfigurieren der Azure Active Directory-Hybrideinbindung für verwaltete Domänen

Ähnlich wie ein Benutzer in Ihrer Organisation ist auch ein Gerät eine zentrale Identität, die Sie schützen möchten. Über die Identität eines Geräts können Sie Ihre Ressourcen jederzeit und von überall aus schützen. Dafür können Sie Geräteidentitäten mit einer der folgenden Methoden in Azure Active Directory (Azure AD) bereitstellen und verwalten:

- Azure AD-Einbindung
- Azure AD-Hybrideinbindung
- Azure AD-Registrierung

Durch das Bereitstellen Ihrer Geräte in Azure AD wird die Benutzerproduktivität über einmaliges Anmelden (SSO) für Ihre gesamten Cloud- und lokalen Ressourcen maximiert. Gleichzeitig können Sie den Zugriff auf Ihre Cloud- und lokalen Ressourcen durch den [bedingten Zugriff](../active-directory-conditional-access-azure-portal.md) sichern.

In diesem Tutorial erfahren Sie, wie die Azure AD-Hybrideinbindung für in die Active Directory-Domäne eingebundene Computer in einer verwalteten Umgebung konfiguriert wird. 

Eine verwaltete Umgebung kann entweder durch [Kennworthashsynchronisierung](../hybrid/whatis-phs.md) (Password Hash Sync, PHS) oder [Pass-Through-Authentifizierung](../hybrid/how-to-connect-pta.md) (Pass Through Authentication, PTA) mit [nahtlosem einmaligem Anmelden](../hybrid/how-to-connect-sso.md) (Seamless Single Sign-On, Seamless SSO) bereitgestellt werden. In diesen Szenarien müssen Sie keinen Verbundserver für die Authentifizierung konfigurieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren der Hybrid-Azure AD-Einbindung
> * Aktivieren von kompatiblen Windows-Geräten
> * Überprüfen der eingebundenen Geräte
> * Problembehandlung

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie mit folgenden Artikeln vertraut sind:

- [Was ist eine Geräteidentität?](overview.md)
- [Planen der Implementierung einer Azure AD-Hybrideinbindung](hybrid-azuread-join-plan.md)
- [Kontrollierte Überprüfung der Azure AD-Hybrideinbindung](hybrid-azuread-join-control.md)

> [!NOTE]
> Azure AD unterstützt keine Smartcards oder Zertifikate in verwalteten Domänen.

Zum Konfigurieren des Szenarios in diesem Artikel muss die [aktuelle Version von Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 oder höher) installiert sein.

Vergewissern Sie sich, dass Azure AD Connect die Computerobjekte der Geräte für die Azure AD-Hybrideinbindung mit Azure AD synchronisiert. Wenn die Computerobjekte zu bestimmten Organisationseinheiten (OEs) gehören, müssen Sie diese Organisationseinheiten ebenfalls so konfigurieren, dass sie in Azure AD Connect synchronisiert werden. Weitere Informationen zum Synchronisieren von Computerobjekten mit Azure AD Connect finden Sie unter [Azure AD Connect-Synchronisierung: Konfigurieren der Filterung](../hybrid/how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering).

Ab Version 1.1.819.0 enthält Azure AD Connect einen Assistenten, den Sie für die Konfiguration der Azure AD-Hybrideinbindung verwenden können. Mit dem Assistenten wird der Konfigurationsprozess erheblich vereinfacht. Der Assistent konfiguriert die Dienstverbindungspunkte (SCP) für die Geräteregistrierung.

Die Konfigurationsschritte in diesem Artikel basieren auf der Verwendung des Assistenten in Azure AD Connect.

Für die Azure AD-Hybrideinbindung müssen die Geräte innerhalb des Netzwerks Ihrer Organisation Zugriff auf die folgenden Microsoft-Ressourcen haben:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com` (Wenn Sie nahtloses einmaliges Anmelden verwenden oder verwenden möchten.)

Wenn für Ihre Organisation Zugriff auf das Internet über einen ausgehenden Proxy erforderlich ist, empfiehlt Microsoft die [Implementierung von Web Proxy Auto-Discovery (WPAD)](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)), damit Windows 10-Computer Geräte bei Azure AD registrieren können. Wenn bei der Konfiguration und Verwaltung von WPAD Probleme auftreten, finden Sie entsprechende Informationen unter [Problembehandlung bei der automatischen Erkennung](https://docs.microsoft.com/previous-versions/tn-archive/cc302643(v=technet.10)). 

Wenn Sie WPAD nicht verwenden und Proxyeinstellungen auf Ihrem Computer konfigurieren möchten, ist dies ab Windows 10 1709 möglich. Weitere Informationen finden Sie unter [Configure WinHTTP settings by using a group policy object (GPO)](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/) (Konfigurieren von WinHTTP-Einstellungen über ein Gruppenrichtlinienobjekt (GPO)).

> [!NOTE]
> Wenn Sie Proxyeinstellungen auf Ihrem Computer mithilfe von WinHTTP-Einstellungen konfigurieren, können alle Computer, die keine Verbindung mit dem konfigurierten Proxy herstellen können, auch keine Internetverbindung herstellen.

Wenn Ihre Organisation Internetzugriff über einen authentifizierten ausgehenden Proxy erfordert, müssen Sie sicherstellen, dass Ihre Windows 10-Computer erfolgreich beim ausgehenden Proxy authentifiziert werden können. Da Windows 10-Computer die Geräteregistrierung mithilfe von Computerkontext ausführen, müssen Sie die Authentifizierung bei ausgehenden Proxys mit dem Computerkontext konfigurieren. Erkundigen Sie sich beim Anbieter Ihres ausgehenden Proxys nach den Konfigurationsanforderungen.

Sie können das Skript zum [Testen der Geräteregistrierungskonnektivität](https://gallery.technet.microsoft.com/Test-Device-Registration-3dc944c0) verwenden, um zu überprüfen, ob das Gerät unter dem Systemkonto auf die oben genannten Microsoft-Ressourcen zugreifen kann.

## <a name="configure-hybrid-azure-ad-join"></a>Konfigurieren der Hybrid-Azure AD-Einbindung

Zum Konfigurieren einer Azure AD-Hybrideinbindung mithilfe von Azure AD Connect benötigen Sie Folgendes:

- Die Anmeldeinformationen eines globalen Administrators für Ihren Azure AD-Mandanten
- Die Anmeldeinformationen eines Unternehmensadministrators für jede Gesamtstruktur

**So konfigurieren Sie eine Azure AD-Hybrideinbindung mithilfe von Azure AD Connect**

1. Starten Sie Azure AD Connect, und wählen Sie dann **Konfigurieren** aus.

   ![Willkommen](./media/hybrid-azuread-join-managed-domains/11.png)

1. Wählen Sie auf der Seite **Zusätzliche Aufgaben** die Option **Geräteoptionen konfigurieren** und dann **Weiter** aus.

   ![Zusätzliche Aufgaben](./media/hybrid-azuread-join-managed-domains/12.png)

1. Wählen Sie auf der Seite **Übersicht** die Option **Weiter** aus.

   ![Übersicht](./media/hybrid-azuread-join-managed-domains/13.png)

1. Geben Sie auf der Seite **Mit Azure AD verbinden** die Anmeldeinformationen eines globalen Administrators für Ihren Azure AD-Mandanten ein.  

   ![Stellen Sie eine Verbindung mit Azure AD her.](./media/hybrid-azuread-join-managed-domains/14.png)

1. Wählen Sie auf der Seite **Geräteoptionen** die Option **Hybrid-Azure AD-Einbindung konfigurieren** und dann **Weiter** aus.

   ![Geräteoptionen](./media/hybrid-azuread-join-managed-domains/15.png)

1. Führen Sie auf der Seite **SCP** für jede Gesamtstruktur, in der Azure AD Connect das SCP konfigurieren soll, die folgenden Schritte aus, und wählen Sie dann **Weiter** aus:

   ![SCP](./media/hybrid-azuread-join-managed-domains/16.png)

   1. Wählen Sie die Gesamtstruktur aus.
   1. Wählen Sie den Authentifizierungsdienst aus.
   1. Wählen Sie **Hinzufügen** aus, um die Anmeldeinformationen eines Unternehmensadministrators einzugeben.

1. Wählen Sie auf der Seite **Gerätebetriebssysteme** die Betriebssysteme der Geräte in Ihrer Active Directory-Umgebung und dann **Weiter** aus.

   ![Gerätebetriebssystem](./media/hybrid-azuread-join-managed-domains/17.png)

1. Wählen Sie auf der Seite **Bereit zur Konfiguration** die Option **Konfigurieren** aus.

   ![Bereit zur Konfiguration](./media/hybrid-azuread-join-managed-domains/19.png)

1. Wählen Sie auf der Seite **Konfiguration abgeschlossen** die Option **Beenden** aus.

   ![Konfiguration abgeschlossen](./media/hybrid-azuread-join-managed-domains/20.png)

## <a name="enable-windows-downlevel-devices"></a>Aktivieren von kompatiblen Windows-Geräten

Wenn es sich bei einigen Ihrer in die Domäne eingebundenen Geräte um kompatible Windows-Geräte handelt, gehen Sie wie folgt vor:

- Konfigurieren der lokalen Intraneteinstellungen für die Geräteregistrierung
- Konfigurieren des nahtlosen einmaligen Anmeldens
- Installieren von Microsoft Workplace Join für kompatible Windows-Computer

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Konfigurieren der lokalen Intraneteinstellungen für die Geräteregistrierung

Um die Einbindung in Hybrid-Azure AD für Ihre kompatiblen Windows-Geräte erfolgreich abzuschließen und Zertifikataufforderungen bei der Authentifizierung von Geräten bei Azure AD zu vermeiden, können Sie eine Richtlinie auf Ihre in die Domäne eingebundenen Geräte übertragen, mit der die folgende URL in Internet Explorer der Zone „Lokales Intranet“ hinzugefügt wird:

- `https://device.login.microsoftonline.com`
- `https://autologon.microsoftazuread-sso.com`

Außerdem müssen Sie in der lokalen Intranetzone des Benutzers die Option **Aktualisierungen der Statusleiste per Skript zulassen** aktivieren.

### <a name="configure-seamless-sso"></a>Konfigurieren des nahtlosen einmaligen Anmeldens

Sie müssen außerdem [nahtloses SSO konfigurieren](../hybrid/how-to-connect-sso-quick-start.md#step-2-enable-the-feature), um die Azure AD-Hybrideinbindung Ihrer kompatiblen Windows-Geräte in einer verwalteten Domäne erfolgreich durchzuführen, die [PHS](../hybrid/whatis-phs.md) oder [PTA](../hybrid/how-to-connect-pta.md) als Authentifizierungsmethode für die Azure AD-Cloud verwendet.

### <a name="install-microsoft-workplace-join-for-windows-downlevel-computers"></a>Installieren von Microsoft Workplace Join für kompatible Windows-Computer

Zur Registrierung von kompatiblen Windows-Geräten müssen Organisationen [Microsoft Workplace Join für Computer installieren, auf denen nicht Windows 10 ausgeführt wird](https://www.microsoft.com/download/details.aspx?id=53554). Microsoft Workplace Join für Computer, auf denen nicht Windows 10 ausgeführt wird, steht im Microsoft Download Center zur Verfügung.

Sie können das Paket mithilfe eines Softwareverteilungssystems wie  [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager) bereitstellen. Das Paket unterstützt die Standardoptionen für die Installation im Hintergrund unter Verwendung des `quiet`-Parameters. Configuration Manager Current Branch bietet zusätzliche Vorteile gegenüber früheren Versionen, z.B. die Möglichkeit zur Nachverfolgung abgeschlossener Registrierungen.

Das Installationsprogramm erstellt einen geplanten Task für das System, der im Kontext des Benutzers ausgeführt wird. Der Task wird ausgelöst, wenn sich der Benutzer bei Windows anmeldet. Nach der Authentifizierung durch Azure AD bindet der Task das Gerät unter Verwendung der Anmeldeinformationen des Benutzers im Hintergrund in Azure AD ein.

## <a name="verify-the-registration"></a>Überprüfen der Registrierung

Zum Überprüfen des Geräteregistrierungsstatus in Ihrem Azure-Mandanten können Sie das Cmdlet **[Get-MsolDevice](/powershell/msonline/v1/get-msoldevice)** im [Azure Active Directory PowerShell-Modul](/powershell/azure/install-msonlinev1?view=azureadps-2.0) verwenden.

Bei Verwendung des Cmdlets **Get-MSolDevice** zur Überprüfung der Dienstdetails:

- Ein Objekt mit der **Geräte-ID**, die der ID auf dem Windows-Client entspricht, muss vorhanden sein.
- Der Wert für **DeviceTrustType** muss **Domänenbeitritt** sein. Diese Einstellung entspricht dem Status **Hybrid in Azure AD eingebunden** auf der Seite **Geräte** im Azure AD-Portal.
- Für Geräte, die für den bedingten Zugriff verwendet werden, muss **Aktiviert** den Wert **True** und **DeviceTrustLevel** den Wert **Verwaltet** haben.

**So überprüfen Sie die Dienstdetails**

1. Öffnen Sie Windows PowerShell als Administrator.
1. Geben Sie `Connect-MsolService` ein, um die Verbindung mit Ihrem Azure-Mandanten herzustellen.  
1. Geben Sie `get-msoldevice -deviceId <deviceId>` ein.
1. Vergewissern Sie sich, dass **Aktiviert** auf **True** festgelegt ist.

## <a name="troubleshoot-your-implementation"></a>Problembehandlung bei der Implementierung

Sollten bei der Azure AD-Hybrideinbindung für in Domänen eingebundene Windows-Geräte Probleme auftreten, finden Sie weitere Informationen unter:

- [Problembehandlung für in Azure AD eingebundene aktuelle Windows-Hybridgeräte](troubleshoot-hybrid-join-windows-current.md)
- [Problembehandlung für in Azure AD eingebundene kompatible Windows-Hybridgeräte](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Geräteidentitäten im Azure-Portal verwalten](device-management-azure-portal.md).
