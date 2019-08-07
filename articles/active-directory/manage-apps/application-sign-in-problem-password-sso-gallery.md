---
title: Probleme bei der Anmeldung bei einer für SSO konfigurierten Azure AD-Katalog-App | Microsoft-Dokumentation
description: Beheben von Problemen mit einer Azure AD-Kataloganwendung, die für einmaliges Anmelden per Kennwort konfiguriert ist.
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: e9fd17d9e066be6a1abff5165436a09b8921184e
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2019
ms.locfileid: "68381308"
---
# <a name="sign-in-problems-with-an-azure-ad-gallery-app-configured-for-sso"></a>Probleme beim Anmelden bei einer Azure AD-Katalog-App, die für SSO konfiguriert ist

Der Zugriffsbereich ist ein webbasiertes Portal. Er ermöglicht Benutzern, die über Geschäfts-, Schul- oder Unikonten für Azure Active Directory (Azure AD) verfügen, auf cloudbasierte Apps zuzugreifen, für die sie Berechtigungen besitzen. Benutzer, die über Azure AD-Editionen verfügt, kann über den Zugriffsbereich auch die Funktionen für die Self-Service-basierte Gruppen- und App-Verwaltung nutzen.

Der Zugriffsbereich ist vom Azure-Portal getrennt. Benutzer benötigen kein Azure-Abonnement, um den Zugriffsbereich zu verwenden.

Für die Verwendung des einmaligen Anmeldens (SSO) per Kennwort im Anwendungsbereich muss die Erweiterung für den Zugriffsbereich im Browser installiert sein. Die Erweiterung wird automatisch heruntergeladen, wenn Sie eine App auswählen, die für kennwortbasiertes SSO konfiguriert ist.

## <a name="browser-requirements-for-access-panel"></a>Browseranforderungen für den Zugriffsbereich

Der Zugriffsbereich erfordert einen Browser, der JavaScript unterstützt und in dem CSS aktiviert ist.

Die folgenden Browser unterstützt kennwortbasiertes SSO:

- Internet Explorer 8, 9, 10 und 11 unter Windows 7 oder höher

- Chrome unter Windows 7 oder höher oder unter Mac OS X oder höher

- Firefox 26.0 oder höher unter Windows XP SP2 oder höher und Mac OS X 10.6 oder höher

>[!NOTE]
>Die Erweiterung für das kennwortbasierte einmalige Anmelden steht für Microsoft Edge unter Windows 10 zur Verfügung, sobald Browsererweiterungen von Microsoft Edge unterstützt werden.

## <a name="install-the-access-panel-browser-extension"></a>Installieren der Browsererweiterung für den Zugriffsbereich

Folgen Sie diesen Schritten:

1. Öffnen Sie den [Zugriffsbereich](https://myapps.microsoft.com) in einem unterstützten Browser, und melden Sie sich als Benutzer in Azure AD an.

2. Wählen Sie im Zugriffsbereich eine Kennwort-SSO-fähige App aus.

3. Klicken Sie auf **Jetzt installieren**, wenn Sie dazu aufgefordert werden.

4. Sie werden basierend auf Ihrem Browser zum Downloadlink weitergeleitet. Wählen Sie **Hinzufügen** aus, um die Browsererweiterung zu installieren.

5. Wählen Sie **Aktivieren** oder **Zulassen** aus, wenn eine entsprechende Aufforderung angezeigt wird.

6. Starten Sie nach der Installation den Browser neu.

7.  Melden Sie sich beim Zugriffsbereich an, und vergewissern Sie sich, ob Sie Ihre Kennwort-SSO-fähigen Apps starten können.

Sie können die Erweiterungen für Chrome und Firefox auch direkt über diese Links herunterladen:

-   [Zugriffsbereichserweiterung für Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Zugriffsbereichserweiterung für Firefox](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Einrichten einer Gruppenrichtlinie für Internet Explorer

Sie können eine Gruppenrichtlinie einrichten, um die Zugriffsbereichserweiterung für Internet Explorer per Remotezugriff auf den Computern Ihrer Benutzer zu installieren.

Dies sind die Voraussetzungen:

-   [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx) muss eingerichtet sein, und die Computer Ihrer Benutzer müssen Ihrer Domäne hinzugefügt werden.

-   Sie verfügen über die Berechtigung „Einstellungen bearbeiten“ zum Bearbeiten des Gruppenrichtlinienobjekts. Standardmäßig verfügen Mitglieder der folgenden Sicherheitsgruppen über diese Berechtigung: Domänenadministratoren, Organisationsadministratoren und Ersteller/Besitzer von Gruppenrichtlinien. [Weitere Informationen](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

Informationen zum Konfigurieren der Gruppenrichtlinie und Bereitstellen der Gruppenrichtlinie für Benutzer finden Sie unter [Bereitstellen der Zugriffsbereichserweiterung für Internet Explorer mit der Gruppenrichtlinie](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy).

## <a name="troubleshoot-access-panel-in-internet-explorer"></a>Problembehandlung für den Zugriffsbereich in Internet Explorer

Informationen über den Zugriff auf ein Diagnosetool und Anweisungen zum Konfigurieren der Erweiterung finden Sie unter [Problembehandlung in der Zugriffsbereichserweiterung für Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting).

## <a name="configure-password-sso-for-an-azure-ad-gallery-app"></a>Konfigurieren des einmaligen Anmeldens per Kennwort für eine Azure AD-Katalog-App

Um eine App aus dem Azure AD-Katalog zu konfigurieren, müssen folgende Schritte ausgeführt werden:

-   Hinzufügen der App aus dem Azure AD-Anwendungskatalog
-   [Konfigurieren der App für das einmalige Anmelden per Kennwort](#configure-the-app-for-password-sso)
-   [Zuweisen von Benutzern zur App](#assign-users-to-the-app)

### <a name="add-the-app-from-the-azure-ad-gallery"></a>Hinzufügen der App aus dem Azure AD-Anwendungskatalog

Folgen Sie diesen Schritten:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und melden Sie sich als globaler Administrator oder Co-Administrator an.

2. Klicken Sie oben im Navigationsbereich auf der linken Seite auf **Alle Dienste** um die Azure AD-Erweiterung zu öffnen.

3. Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und klicken Sie auf **Azure Active Directory**.

4. Klicken Sie im Azure AD-Navigationsbereich auf **Unternehmensanwendungen**.

5. Klicken Sie in der oberen rechten Ecke des Bereichs **Unternehmensanwendungen** auf **Hinzufügen**.

6. Geben Sie im Abschnitt **Aus Katalog hinzufügen** im Feld **Namen eingeben** den Namen der App ein.

7. Wählen Sie die App aus, die Sie für SSO konfigurieren möchten.

8. *Optional*: Bevor Sie die App hinzufügen, können Sie ihren Namen im Feld **Name** ändern.

9. Klicken Sie auf **Hinzufügen**, um die App hinzuzufügen.

   Nach einer kurzen Verzögerung wird der Konfigurationsbereich der App angezeigt.

### <a name="configure-the-app-for-password-sso"></a>Konfigurieren der App für das Kennwort-SSO

Folgen Sie diesen Schritten:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und melden Sie sich als globaler Administrator oder Co-Administrator an.

2. Klicken Sie oben im Navigationsbereich auf der linken Seite auf **Alle Dienste** um die Azure AD-Erweiterung zu öffnen.

3. Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und klicken Sie auf **Azure Active Directory**.

4. Klicken Sie im Azure AD-Navigationsbereich auf **Unternehmensanwendungen**.

5. Klicken Sie auf **Alle Anwendungen**, um eine Liste mit Ihren Apps anzuzeigen.

   > [!NOTE]
   > Wenn eine App nicht wie erwartet angezeigt wird, verwenden Sie das **Filter**-Steuerelement über der Liste **Alle Anwendungen**. Legen Sie für die Option **Anzeigen** „Alle Anwendungen“ fest.

6. Wählen Sie die App aus, die Sie für SSO konfigurieren möchten.

7. Warten Sie, bis die App geladen ist, und klicken Sie anschließend im Bereich auf der linken Seite der App auf **Einmaliges Anmelden**.

8. Wählen Sie den Modus **Kennwortbasiertes Anmelden** aus.

9. Weisen Sie der App Benutzer zu.

10. Sie können auch Anmeldeinformationen für Benutzer bereitstellen. (Andernfalls werden die Benutzer beim Starten der App aufgefordert, ihre Anmeldeinformationen einzugeben.) Wählen Sie zu diesem Zweck die Zeilen der Benutzer aus. Wählen Sie dann **Anmeldeinformationen aktualisieren** aus, und geben Sie die Benutzernamen und Kennwörter der Benutzer ein.

### <a name="assign-users-to-the-app"></a>Zuweisen von Benutzern zur App

Um einer App Benutzer direkt zuzuweisen, gehen Sie folgendermaßen vor:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und melden Sie sich als globaler Administrator an.

2. Wählen Sie im Navigationsbereich auf der linken Seite **Alle Dienste** aus, um die Azure AD-Erweiterung zu öffnen.

3. Geben Sie im Filtersuchfeld **Azure Active Directory** ein, und klicken Sie auf **Azure Active Directory**.

4. Klicken Sie im Azure AD-Navigationsbereich auf **Unternehmensanwendungen**.

5. Klicken Sie auf **Alle Anwendungen**, um eine Liste mit Ihren Anwendungen anzuzeigen.

   > [!NOTE]
   > Wenn eine App nicht wie erwartet angezeigt wird, verwenden Sie das **Filter**-Steuerelement über der Liste **Alle Anwendungen**. Legen Sie für die Option **Anzeigen** „Alle Anwendungen“ fest.

6. Wählen Sie in der Liste die App aus, der Sie einen Benutzer zuweisen möchten.

7. Nachdem die Anwendung geladen wurde, wählen Sie im linken Navigationsbereich der App **Benutzer und Gruppen** aus.

8. Wählen Sie oberhalb der Liste **Benutzer und Gruppen** die Option **Hinzufügen** aus, um den Bereich **Zuweisung hinzufügen** zu öffnen.

9. Wählen Sie dann im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

10. Geben Sie im Feld **Nach Name oder E-Mail-Adresse suchen** den vollständigen Namen oder die E-Mail-Adresse des Benutzers ein, der zugewiesen werden soll.

11. Bewegen Sie den Cursor über die Benutzer in der Liste. Aktivieren Sie das Kontrollkästchen neben dem Bild oder Logo des Benutzerprofils, um diesen Benutzer der Liste **Ausgewählt** hinzuzufügen.

12. *Optional*: Zum Hinzuzufügen eines weiteren Benutzers geben Sie dessen Namen oder E-Mail-Adresse in das Feld **Nach Name oder E-Mail-Adresse suchen** ein, und aktivieren Sie dann das Kontrollkästchen, um diesen Benutzer zur Liste **Ausgewählt** hinzuzufügen.

13. Wenn Sie alle gewünschten Benutzer ausgewählt haben, klicken Sie auf **Auswählen**, um sie der Liste der Benutzer und Gruppen hinzuzufügen, die der App zugewiesen sind.

14. *Optional*: Klicken Sie im Bereich **Zuweisung hinzufügen** auf das Feld auf **Rolle auswählen**, um eine Rolle auszuwählen, die den ausgewählten Benutzern zugewiesen werden soll.

15. Wählen Sie **Zuweisen** aus, um die App den ausgewählten Benutzern zuzuweisen.

    Nach einer kurzen Verzögerung können die Benutzer über den Zugriffsbereich auf diese Apps zugreifen.

## <a name="request-support"></a>Anfordern von Support 
Wenn Sie beim Einrichten von SSO und Zuweisen von Benutzern eine Fehlermeldung erhalten, öffnen Sie ein Supportticket. Fügen Sie so viele der folgenden Informationen wie möglich hinzu:

-   Fehlerkorrelations-ID
-   UPN (E-Mail-Adresse des Benutzers)
-   Mandanten-ID
-   Browsertyp
-   Zeitzone und Zeitpunkt/Zeitraum des Fehlers
-   Fiddler-Ablaufverfolgungen

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von einmaligem Anmelden bei Ihren Apps mit dem Anwendungsproxy](application-proxy-configure-single-sign-on-with-kcd.md)
