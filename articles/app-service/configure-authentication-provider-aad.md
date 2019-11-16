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
ms.custom: fasttrack-edit
ms.openlocfilehash: 6812f99d8433ef318eca37eb2615d43f4749e944
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73886186"
---
# <a name="configure-your-app-service-app-to-use-azure-ad-login"></a>Konfigurieren Ihrer App Service-App zur Verwendung der Azure AD-Anmeldung

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Artikel wird gezeigt, wie Sie Azure App Service so konfigurieren, dass Azure Active Directory (Azure AD) als Authentifizierungsanbieter verwendet wird.

> [!NOTE]
> Derzeit werden Azure App Service und Azure Functions nur von Azure AD v1.0 unterstützt. Sie werden von der [Microsoft Identity Platform v2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-overview), die Microsoft Authentication Librarys (MSAL) enthält, nicht unterstützt.

Befolgen Sie diese bewährten Methoden, wenn Sie Ihre App und die Authentifizierung einrichten:

- Erteilen Sie jeder App Service-App eigene Berechtigungen und Einwilligung.
- Konfigurieren Sie jede App Service-App mit eigener Registrierung.
- Vermeiden Sie das Teilen von Berechtigungen zwischen Umgebungen, indem Sie separate App-Registrierungen für gesonderte Bereitstellungsslots verwenden. Wenn Sie neuen Code testen, kann diese Vorgehensweise dabei helfen, Probleme zu verhindern, die sich auf die Produktions-App auswirken können.

## <a name="express"> </a>Konfigurieren mit Expresseinstellungen

1. Wechseln Sie im [Azure-Portal] zu Ihrer App Service-App.
1. Wählen Sie im linken Bereich **Einstellungen** > **Authentifizierung/Autorisierung,** aus, und stellen Sie sicher, dass für die **App Service-Authentifizierung** die Option **Ein** festgelegt ist.
1. Wählen Sie **Azure Active Directory** und dann unter **Verwaltungsmodus** die Option **Express**.
1. Klicken Sie auf **OK**, um die App Service-App in Azure Active Directory zu registrieren. Eine neue App-Registrierung wird erstellt.

   Wenn Sie stattdessen eine vorhandene App-Registrierung auswählen möchten:

   1. Wählen Sie **Vorhandene App auswählen** aus, und suchen Sie nach dem Namen einer zuvor erstellten App-Registrierung in Ihrem Mandanten.
   1. Wählen Sie die App-Registrierung aus, und wählen Sie dann **OK** aus.
   1. Wählen Sie dann auf der Seite „Azure Active Directory-Einstellungen“ **OK** aus.

   Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren.
1. (Optional) Um den Zugriff auf Ihre App ausschließlich auf Benutzer zu beschränken, die von Azure Active Directory authentifiziert werden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Azure Active Directory anmelden** fest. Wenn Sie diese Funktion festlegen, erfordert Ihre App, dass alle Anforderungen authentifiziert werden. Sie leitet außerdem alle nicht authentifizierten Anforderungen zur Authentifizierung an Azure Active Directory um.

    > [!CAUTION]
    > Das Einschränken des Zugriffs auf diese Weise gilt für alle Aufrufe Ihrer App, was für Apps, die eine öffentlich verfügbare Startseite haben, eventuell nicht wünschenswert ist, wie bei vielen Single-Page-Anwendungen. Bei solchen Anwendungen ist möglicherweise die Einstellung **Anonyme Anforderungen zulassen (keine Aktion)** vorzuziehen, wobei die App selbst die Anmeldung manuell startet. Weitere Informationen finden Sie unter [Authentifizierungsflow](overview-authentication-authorization.md#authentication-flow).
1. Wählen Sie **Speichern** aus.

## <a name="advanced"> </a>Konfigurieren mit erweiterten Einstellungen

Sie können App-Einstellungen manuell konfigurieren, wenn Sie einen Azure AD-Mandanten verwenden möchten, der sich von dem unterscheidet, den Sie für die Anmeldung bei Azure verwenden. Um diese benutzerdefinierte Konfiguration abzuschließen, müssen Sie Folgendes tun:

1. Erstellen einer Registrierung in Azure AD.
1. Bereitstellen einige Registrierungsdetails für den App Service.

### <a name="register"> </a>Erstellen einer App-Registrierung in Azure AD für Ihre App Service-App

Sie benötigen die folgenden Informationen, wenn Sie Ihre App Service-App konfigurieren:

- Client-ID
- Mandanten-ID
- Geheimer Clientschlüssel (optional)
- Anwendungs-ID-URI

Führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim [Azure-Portal] an, und wechseln Sie zu Ihrer App Service-App. Notieren Sie sich die **URL** Ihrer App. Sie verwenden diese, um die Registrierung Ihrer Azure Active Directory-App zu konfigurieren.
1. Wählen Sie **Azure Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus.
1. Geben Sie auf der Seite **Anwendung registrieren** einen **Namen** für Ihre App-Registrierung ein.
1. Wählen Sie in **Umleitungs-URI** die Option **Web** aus, geben Sie die URL Ihrer App Service-App ein, und fügen Sie den Pfad `/.auth/login/aad/callback` an. Beispiel: `https://contoso.azurewebsites.net/.auth/login/aad/callback`. 
1. Klicken Sie auf **Erstellen**.
1. Nachdem die App-Registrierung erstellt wurde, kopieren Sie die **Anwendungs-ID (Client)** und die **Verzeichnis-ID (Mandant)** , damit Sie diese später verwenden können.
1. Wählen Sie **Branding** aus. Geben Sie in **URL der Startseite** die URL Ihrer App Service-App ein, und wählen Sie **Speichern** aus.
1. Wählen Sie **Eine API verfügbar machen** > **Festlegen** aus. Fügen Sie die URL Ihrer App Service-App ein, und wählen Sie **Speichern** aus.

   > [!NOTE]
   > Dieser Wert ist der **Anwendungs-ID-URI** der App-Registrierung. Wenn Ihre Web-App Zugriff auf eine API in der Cloud erfordert, benötigen Sie den **Anwendungs-ID-URI** der Web-App, wenn Sie die App Service-Cloudressource konfigurieren. Sie können dies beispielsweise verwenden, wenn der Clouddienst explizit Zugriff auf die Web-App gewähren soll.

1. Wählen Sie **Bereich hinzufügen**.
   1. Geben Sie in **Bereichsname** den Namen *user_impersonation* ein.
   1. Geben Sie in die Textfelder den Namen und die Beschreibung für den Einwilligungsbereich ein, die Benutzern auf der Einwilligungsseite angezeigt werden sollen. Geben Sie z. B. *Zugriff auf meine App* ein. 
   1. Wählen Sie **Bereich hinzufügen** aus.
1. (Optional) Um einen geheimen Clientschlüssel zu erstellen, wählen Sie **Zertifikate und Geheimnisse** > **Neuer geheimer Clientschlüssel** > **Hinzufügen** aus. Kopieren Sie den Wert des geheimen Clientschlüssels, der auf der Seite angezeigt wird. Er wird nicht noch einmal angezeigt.
1. (Optional) Wenn Sie mehrere **Antwort-URLs** hinzufügen möchten, wählen Sie **Authentifizierung** aus.

### <a name="secrets"> </a>Hinzufügen von Azure Active Directory-Informationen zu Ihrer App Service-App

1. Wechseln Sie im [Azure-Portal] zu Ihrer App Service-App. 
1. Wählen Sie im linken Bereich **Einstellungen > Authentifizierung/Autorisierung** aus, und stellen Sie sicher, dass für die **App Service-Authentifizierung** die Option **Ein** festgelegt ist.
1. (Optional) Standardmäßig lässt die App Service Authentifizierung nicht authentifizierten Zugriff auf Ihre App zu. Um Benutzerauthentifizierung zu erzwingen, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Azure Active Directory anmelden** fest.
1. Wählen Sie unter „Authentifizierungsanbieter“ die Option **Azure Active Directory** aus.
1. Wählen Sie in **Verwaltungsmodus** die Option **Erweitert** aus, und konfigurieren Sie die App Service-Authentifizierung gemäß der folgenden Tabelle:

    |Feld|BESCHREIBUNG|
    |-|-|
    |Client-ID| Verwenden Sie die **Anwendungs-ID (Client)** der App-Registrierung. |
    |Zertifikataussteller-ID| Verwenden Sie `https://login.microsoftonline.com/<tenant-id>`, und ersetzen Sie *\<tenant-id* durch die **Verzeichnis-ID (Mandant)** der App-Registrierung. |
    |Geheimer Clientschlüssel (optional)| Verwenden Sie den geheimen Clientschlüssel, den Sie in der App-Registrierung generiert haben.|
    |Zulässige Tokenzielgruppen| Wenn dies eine Cloud- oder Server-App ist und Sie Authentifizierungstoken von einer Web-App zulassen möchten, fügen Sie hier den **Anwendungs-ID-URI** der Web-App hinzu. |

    > [!NOTE]
    > Die konfigurierte **Client-ID** wird *immer* implizit als zulässige Zielgruppe betrachtet, und zwar unabhängig davon, wie Sie die Option **Zulässige Tokenzielgruppen** konfiguriert haben.
1. Wählen Sie **OK** und anschließend **Speichern** aus.

Sie können nun Azure Active Directory für die Authentifizierung in Ihrer App Service-App verwenden.

## <a name="configure-a-native-client-application"></a>Konfigurieren einer nativen Clientanwendung

Sie können native Clients registrieren, um die Authentifizierung mittel Clientbibliothek zuzulassen, etwa der **Active Directory-Authentifizierungsbibliothek**.

1. Wählen Sie im [Azure-Portal] nacheinander **Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus.
1. Geben Sie auf der Seite **Anwendung registrieren** einen **Namen** für Ihre App-Registrierung ein.
1. Wählen Sie in **Umleitungs-URI** die Option **Öffentlicher Client (Mobilgerät und Desktop)** aus, geben Sie die URL Ihrer App Service-App ein, und fügen Sie den Pfad `/.auth/login/aad/callback` an. Beispiel: `https://contoso.azurewebsites.net/.auth/login/aad/callback`.
1. Klicken Sie auf **Erstellen**.

    > [!NOTE]
    > Für eine Windows-Anwendung verwenden Sie stattdessen die [Paket-SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als URI.
1. Sobald die App-Registrierung erstellt wurde, kopieren Sie den Wert von **Anwendungs-ID (Client)** .
1. Wählen Sie über die Option **API-Berechtigungen** > **Berechtigung hinzufügen** > **Meine APIs** aus.
1. Wählen Sie die App-Registrierung aus, die Sie zuvor für Ihre App Service-App erstellt haben. Wenn die App-Registrierung nicht angezeigt wird, stellen Sie sicher, dass Sie den Bereich **user_impersonation** in [Erstellen einer App-Registrierung in Azure AD für Ihre App Service-App](#register) hinzugefügt haben.
1. Wählen Sie **user_impersonation** aus, und wählen Sie dann **Berechtigungen hinzufügen** aus.

Sie haben nun eine native Clientanwendung konfiguriert, die auf Ihre App Service-App zugreifen kann.

## <a name="related-content"> </a>Nächste Schritte

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
