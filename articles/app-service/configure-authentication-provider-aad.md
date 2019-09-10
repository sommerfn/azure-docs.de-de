---
title: 'Konfigurieren der Azure Active Directory-Authentifizierung: Azure App Service'
description: Erfahren Sie, wie Sie die Azure Active Directory-Authentifizierung für Ihre App Services-App konfigurieren.
author: cephalin
services: app-service
documentationcenter: ''
manager: gwallace
editor: ''
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service
ms.workload: web,mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/03/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 8b4b6549f9553773cc44c311f49befbb3eec9dc9
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70233100"
---
# <a name="configure-your-app-service-app-to-use-azure-active-directory-sign-in"></a>Konfigurieren Ihrer App Service-App zur Verwendung der Azure Active Directory-Anmeldung

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

> [!NOTE]
> Derzeit wird AAD V2 (einschließlich MSAL) für Azure App Service und Azure Functions nicht unterstützt.
>

In diesem Artikel wird gezeigt, wie Sie Azure App Service so konfigurieren, dass Azure Active Directory als Authentifizierungsanbieter verwendet wird.

Es empfiehlt sich, dass Sie jede App Service-App mit ihrer eigenen Registrierung konfigurieren, sodass die App ihre eigenen Berechtigungen und ihre eigene Einwilligung hat. Sie sollten auch die Verwendung separater App-Registrierungen für separate Bereitstellungsslots in Betracht ziehen. Dadurch wird gemeinsames Verwenden von Berechtigungen zwischen Umgebungen verhindert, sodass ein Problem in neuem Code, den Sie testen, die Produktion nicht beeinträchtigt.

## <a name="express"> </a>Konfigurieren mit Expresseinstellungen

1. Navigieren Sie im [Azure-Portal] zu Ihrer App Service-App. Wählen Sie im linken Navigationsbereich **Authentifizierung/Autorisierung**.
2. Wenn **Authentifizierung/Autorisierung** nicht aktiviert ist, wählen Sie **Ein**.
3. Wählen Sie **Azure Active Directory** und dann unter **Verwaltungsmodus** die Option **Express**.
4. Klicken Sie auf **OK**, um die App Service-App in Azure Active Directory zu registrieren. Dadurch wird eine neue App-Registrierung erstellt. Wenn Sie stattdessen eine vorhandene App-Registrierung auswählen möchten, klicken Sie auf **Vorhandene App auswählen**, und suchen Sie nach dem Namen einer zuvor erstellten Registrierung des Mandanten. Klicken Sie auf die App-Registrierung, um sie auszuwählen, und klicken Sie auf **OK**. Klicken Sie dann auf der Seite „Azure Active Directory-Einstellungen“ auf **OK**.
Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren.
5. (Optional) Um den Zugriff auf Ihre App auf Benutzer zu beschränken, die von Azure Active Directory authentifiziert werden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Azure Active Directory anmelden** fest. Dies erfordert, dass alle Anforderungen authentifiziert werden müssen. Alle nicht authentifizierten Anforderungen werden zur Authentifizierung an Azure Active Directory umgeleitet.

    > [!NOTE]
    > Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite wünschen, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen. Bei solchen Anwendungen ist möglicherweise die Einstellung **Anonyme Anforderungen zulassen (keine Aktion)** vorzuziehen, wobei die App selbst die Anmeldung manuell startet, wie [hier](overview-authentication-authorization.md#authentication-flow) beschrieben.
6. Klicken Sie auf **Speichern**.

## <a name="advanced"> </a>Konfigurieren mit erweiterten Einstellungen

Sie können Konfigurationseinstellungen auch manuell bereitstellen, wenn sich der Azure Active Directory-Mandant, den Sie verwenden möchten, von dem Mandanten unterscheidet, mit dem Sie sich bei Azure anmelden. Um die Konfiguration abzuschließen, müssen Sie zunächst eine Registrierung in Azure Active Directory erstellen, und App Service dann einige Registrierungsdetails liefern.

### <a name="register"> </a>Erstellen einer App-Registrierung in Azure AD für Ihre App Service-App

Wenn Sie eine App-Registrierung manuell erstellen, sind drei Informationen erforderlich, die Sie später beim Konfigurieren Ihrer App Service-App benötigen: die Client-ID, die Mandanten-ID und optional der geheime Clientschlüssel und der Anwendungs-ID-URI.

1. Navigieren Sie im [Azure-Portal] zu Ihrer App Service-App, und notieren Sie die **URL** Ihrer App. Sie verwenden die URL, um die Registrierung Ihrer Azure Active Directory-App zu konfigurieren.
1. Wählen Sie im [Azure-Portal] über das linke Menü die Option **Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus. 
1. Geben Sie auf der Seite **Anwendung registrieren** einen **Namen** für Ihre App-Registrierung ein.
1. Wählen Sie in **Umleitungs-URI** die Option **Web** aus, geben Sie die URL Ihrer App Service-App ein, und fügen Sie den Pfad `/.auth/login/aad/callback` an. Beispiel: `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Klicken Sie anschließend auf **Erstellen**.
1. Nachdem die App-Registrierung erstellt wurde, kopieren Sie die **Anwendungs-ID (Client)** und die **Verzeichnis-ID (Mandant)** , damit Sie diese später verwenden können.
1. Wählen Sie **Branding** aus. Geben Sie in **URL der Startseite** die URL Ihrer App Service-App ein, und wählen Sie **Speichern** aus.
1. Wählen Sie **Eine API verfügbar machen** > **Festlegen** aus. Fügen Sie die URL Ihrer App Service-App ein, und wählen Sie **Speichern** aus.

    > [!NOTE]
    > Dieser Wert ist der **Anwendungs-ID-URI** der App-Registrierung. Wenn Sie beispielsweise möchten, dass eine Front-End-Web-App auf eine Back-End-API zugreift und die Back-End-API der Front-End-App explizit Zugriff gewährt, benötigen Sie den **Anwendungs-ID-URI** der *Front-End-App*, wenn Sie die App Service-App-Ressource für die *Back-End-API* konfigurieren.
1. Wählen Sie **Bereich hinzufügen**. Geben Sie in **Bereichsname** den Namen *user_impersonation* ein. Geben Sie in die Textfelder den Namen und die Beschreibung für den Einwilligungsbereich ein, die Benutzern auf der Einwilligungsseite angezeigt werden sollen, etwa *Zugriff auf meine App*. Klicken Sie danach auf **Bereich hinzufügen**.
1. (Optional) Um einen geheimen Clientschlüssel zu erstellen, wählen Sie **Zertifikate und Geheimnisse** > **Neuer geheimer Clientschlüssel** > **Hinzufügen** aus. Kopieren Sie den Wert des geheimen Clientschlüssels, der auf der Seite angezeigt wird. Sobald Sie zu einer anderen Seite navigiert haben, wird er nicht mehr angezeigt.
1. (Optional) Wenn Sie mehrere **Antwort-URLs** hinzufügen möchten, wählen Sie **Authentifizierung** im Menü aus.

### <a name="secrets"> </a>Hinzufügen von Azure Active Directory-Informationen zu Ihrer App Service-App

1. Navigieren Sie im [Azure-Portal] zu Ihrer App Service-App. Wählen Sie im linken Menü die Option **Authentifizierung/Autorisierung** aus. Falls die Funktion „Authentifizierung/Autorisierung“ nicht aktiviert ist, wählen Sie **Ein** aus. 
1. (Optional) Standardmäßig lässt die App Service Authentifizierung nicht authentifizierten Zugriff auf Ihre App zu. Um Benutzerauthentifizierung zu erzwingen, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Azure Active Directory anmelden** fest.
1. Wählen Sie unter „Authentifizierungsanbieter“ die Option **Azure Active Directory** aus.
1. Wählen Sie in **Verwaltungsmodus** die Option **Erweitert** aus, und konfigurieren Sie die App Service-Authentifizierung gemäß der folgenden Tabelle:

    |Feld|BESCHREIBUNG|
    |-|-|
    |Client-ID| Verwenden Sie die **Anwendungs-ID (Client)** der App-Registrierung. |
    |Zertifikataussteller-ID| Verwenden Sie `https://login.microsoftonline.com/<tenant-id>`, und ersetzen Sie *\<tenant-id* durch die **Verzeichnis-ID (Mandant)** der App-Registrierung. |
    |Geheimer Clientschlüssel (optional)| Verwenden Sie den geheimen Clientschlüssel, den Sie in der App-Registrierung generiert haben.|
    |Zulässige Tokenzielgruppen| Wenn dies eine *Back-End*-App ist und Sie Authentifizierungstoken von einer Front-End-App zulassen möchten, fügen Sie hier den **Anwendungs-ID-URI** der *Front-End*-App hinzu. |
1. Wählen Sie **OK** aus, und wählen Sie anschließend **Speichern** aus.

Sie können nun Azure Active Directory für die Authentifizierung in Ihrer App Service-App verwenden.

## <a name="configure-a-native-client-application"></a>Konfigurieren einer nativen Clientanwendung
Sie können native Clients registrieren, wenn Sie Anmeldungen mit einer Clientbibliothek, etwa der **Active Directory-Authentifizierungsbibliothek**, vornehmen möchten.

1. Wählen Sie im [Azure-Portal] über das linke Menü die Option **Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus. 
1. Geben Sie auf der Seite **Anwendung registrieren** einen **Namen** für Ihre App-Registrierung ein.
1. Wählen Sie in **Umleitungs-URI** die Option **Öffentlicher Client (Mobilgerät und Desktop)** aus, geben Sie die URL Ihrer App Service-App ein, und fügen Sie den Pfad `/.auth/login/aad/callback` an. Beispiel: `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Klicken Sie anschließend auf **Erstellen**.

    > [!NOTE]
    > Für eine Windows-Anwendung verwenden Sie stattdessen die [Paket-SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als URI.
1. Sobald die App-Registrierung erstellt wurde, kopieren Sie den Wert von **Anwendungs-ID (Client)** .
1. Wählen Sie über das linke Menü die Option **API-Berechtigungen** > **Berechtigung hinzufügen** > **Meine APIs** aus.
1. Wählen Sie die App-Registrierung aus, die Sie zuvor für Ihre App Service-App erstellt haben. Wenn die App-Registrierung nicht angezeigt wird, überprüfen Sie, ob Sie den **user_impersonation**-Bereich in [Erstellen einer App-Registrierung in Azure AD für Ihre App Service-App](#register) hinzugefügt haben.
1. Wählen Sie **user_impersonation** aus, und klicken Sie auf **Berechtigungen hinzufügen**.

Sie haben nun eine native Clientanwendung konfiguriert, die auf Ihre App Service-App zugreifen kann.

## <a name="related-content"></a>Verwandte Inhalte

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/configure-authentication-provider-aad/app-service-webapp-auth-config.png



<!-- URLs. -->

[Azure-Portal]: https://portal.azure.com/
[alternative method]:#advanced
