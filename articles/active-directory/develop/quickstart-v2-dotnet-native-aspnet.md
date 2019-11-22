---
title: Aufrufen einer von Azure AD geschützten ASP.NET-Web-API – Microsoft Identity
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie aus einer Windows-Desktopanwendung (Windows Presentation Foundation, WPF) eine ASP.NET-Web-API aufrufen, die von Azure Active Directory geschützt wird. Der WPF-Client authentifiziert einen Benutzer, fordert ein Zugriffstoken an und ruft die Web-API auf.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e0fdeb2c1955eab18b440c3ef3bcac725ad92b6
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73200259"
---
# <a name="quickstart-call-an-aspnet-web-api-protected-by-azure-ad"></a>Schnellstart: Aufrufen einer durch Azure AD geschützten ASP.NET-Web-API

In dieser Schnellstartanleitung machen Sie eine Web-API verfügbar und schützen sie, sodass nur authentifizierte Benutzer darauf zugreifen können. Dieses Beispiel zeigt, wie Sie eine ASP.NET-Web-API verfügbar machen, sodass diese Token akzeptieren kann, die sowohl von persönlichen Konten (z. B. outlook.com, live.com u. a.) als auch von Geschäfts-, Schul- und Unikonten von Unternehmen oder Organisationen ausgegeben wurden, die in Azure Active Directory integriert wurden.

Das Beispiel enthält auch einen Windows-Desktopanwendungsclient (WPF), der veranschaulicht, wie Sie ein Zugriffstoken anfordern, um auf eine Web-API zuzugreifen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieses Beispiels benötigen Sie Folgendes:

* Visual Studio 2017 oder 2019.  [Laden Sie Visual Studio kostenlos herunter](https://www.visualstudio.com/downloads/).

* Entweder ein [Microsoft-Konto](https://www.outlook.com) oder ein [Office 365-Entwicklerprogramm](/office/developer-program/office-365-developer-program).

## <a name="download-or-clone-this-sample"></a>Herunterladen oder Klonen dieses Beispiels

Sie können dieses Beispiel über Ihre Shell oder Befehlszeile klonen:

  ```console
  git clone https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git
  ```

Alternativ dazu können Sie das [Beispiel als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip).

## <a name="register-your-web-api-in-the-application-registration-portal"></a>Registrieren Ihrer Web-API im Anwendungsregistrierungsportal

### <a name="choose-the-azure-ad-tenant-where-you-want-to-create-your-applications"></a>Auswählen des Azure AD-Mandanten, in dem Sie Ihre Anwendungen erstellen möchten

Wenn Sie Ihre Apps manuell registrieren möchten, ist Folgendes der erste Schritt:

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wenn Ihr Konto in mehr als einem Azure AD-Mandanten vorhanden ist, wählen Sie in der rechten oberen Ecke im Menü Ihr Profil aus, und klicken Sie dann auf **Verzeichnis wechseln**.
   Ändern Sie die Portalsitzung zum gewünschten Azure AD-Mandanten.

### <a name="register-the-service-app-todolistservice"></a>Registrieren der Dienst-App (TodoListService)

1. Navigieren Sie zur Seite [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) von Microsoft Identity Platform für Entwickler.
1. Wählen Sie **Neue Registrierung** aus.
1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** die Registrierungsinformationen Ihrer Anwendung ein:
   - Geben Sie im Abschnitt **Name** einen aussagekräftigen Anwendungsnamen ein, der den Benutzern der App angezeigt wird (beispielsweise `AppModelv2-NativeClient-DotNet-TodoListService`).
   - Wählen Sie unter **Unterstützte Kontotypen** die Option **Konten in einem beliebigen Organisationsverzeichnis** aus.
   - Wählen Sie **Registrieren** aus, um die Anwendung zu erstellen.

1. Suchen Sie auf der Seite **Übersicht** den Wert von **Anwendungsclient-ID** und notieren Sie ihn zur späteren Verwendung. Sie benötigen diesen Wert, um die Visual Studio-Konfigurationsdatei für dieses Projekt zu konfigurieren (`ClientId` in `TodoListService\Web.config`).
1. Wählen Sie den Abschnitt **Eine API verfügbar machen** aus, und gehen Sie dann folgendermaßen vor:
   - Wählen Sie **Bereich hinzufügen** aus.
   - Akzeptieren Sie den vorgeschlagenen Anwendungs-ID-URI (api ://{clientId}) durch Auswahl von **Speichern und fortfahren**.
   - Legen Sie die folgenden Parameter fest:
     - Verwenden Sie `access_as_user` als **Bereichsnamen**.
     - Stellen Sie sicher, dass die Option **Administratoren und Benutzer** unter **Zum Einwilligen berechtigte Personen** ausgewählt ist.
     - Geben Sie unter **Anzeigename der Administratoreinwilligung** `Access TodoListService as a user` ein.
     - Geben Sie unter **Beschreibung der Administratoreinwilligung** `Accesses the TodoListService Web API as a user` ein.
     - Geben Sie unter **Anzeigename der Benutzereinwilligung** `Access TodoListService as a user` ein.
     - Geben Sie unter **Beschreibung der Benutzereinwilligung** `Accesses the TodoListService Web API as a user` ein.
     - Behalten Sie für den **Zustand** die Einstellung **Aktiviert** bei.
     - Wählen Sie **Bereich hinzufügen** aus.

### <a name="configure-the-service-and-client-projects-to-match-the-registered-web-api"></a>Konfigurieren der Dienst- und Clientprojekte für die registrierte Web-API 

1. Öffnen Sie die Projektmappe in Visual Studio, und öffnen Sie dann im Stammverzeichnis des Projekts **TodoListService** die Datei **Web.config**.
1. Ersetzen Sie den Wert des `ida:ClientId`-Parameters durch die **Client-ID (Anwendungs-ID)** der Anwendung, die Sie gerade im Anwendungsregistrierungsportal registriert haben.

### <a name="add-the-new-scope-to-the-todolistclients-appconfig"></a>Hinzufügen des neuen Bereichs zur app.config-Datei von *TodoListClient*

1. Öffnen Sie die **app.config**-Datei, die sich im Stammordner des **TodoListClient**-Projekts befindet, und fügen Sie die **Anwendungs-ID** der Anwendung, die Sie soeben für Ihren *TodoListService* registriert haben, im `TodoListServiceScope`-Parameter ein. Ersetzen Sie dabei die Zeichenfolge `{Enter the Application ID of your TodoListService from the app registration portal}`.

   > Hinweis: Stellen Sie sicher, dass folgendes Format verwendet wird:
   >
   > `api://{TodoListService-Application-ID}/access_as_user` 
   >
   >(Hierbei ist {TodoListService-Application-ID} die GUID, die für die Anwendungs-ID der TodoListService-Anwendung steht).

## <a name="register-the-client-app-todolistclient"></a>Registrieren der Client-App (TodoListClient)

In diesem Schritt konfigurieren Sie Ihr *TodoListClient*-Projekt, indem Sie eine neue Anwendung im Anwendungsregistrierungsportal registrieren. In Fällen, in denen Client und Server als *ein und dieselbe Anwendung* betrachtet werden, können Sie auch dieselbe Anwendung wiederverwenden, die Sie in Schritt 2 registriert haben. Sie müssen dieselbe Anwendung verwenden, wenn Ihre Benutzer sich mit ihren persönlichen Microsoft-Konten anmelden sollen.

### <a name="register-the-todolistclient-application-in-the-application-registration-portal"></a>Registrieren der *TodoListClient*-Anwendung im *Anwendungsregistrierungsportal*

1. Navigieren Sie zur Seite [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) von Microsoft Identity Platform für Entwickler.
1. Wählen Sie **Neue Registrierung** aus.
1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** die Registrierungsinformationen Ihrer Anwendung ein:
   - Geben Sie im Abschnitt **Name** einen aussagekräftigen Anwendungsnamen ein, der den Benutzern der App angezeigt wird (beispielsweise `NativeClient-DotNet-TodoListClient`).
   - Wählen Sie unter **Unterstützte Kontotypen** die Option **Konten in einem beliebigen Organisationsverzeichnis** aus.
   - Wählen Sie **Registrieren** aus, um die Anwendung zu erstellen.
1. Wählen Sie auf der Seite „Übersicht“ der App den Abschnitt **Authentifizierung** aus.
   - Aktivieren Sie im Abschnitt **Umleitungs-URLs** | **Vorgeschlagene Umleitungs-URLs für öffentliche Clients (Mobilgerät, Desktop)** die Option **urn:ietf:wg:oauth:2.0:oob**.
   - Wählen Sie **Speichern** aus.
1. Wählen Sie den Abschnitt **API-Berechtigungen** aus.
   - Klicken Sie auf die Schaltfläche **Berechtigung hinzufügen**.
   - Wählen Sie die Registerkarte **Meine APIs** aus.
   - Wählen Sie in der Liste der APIs `AppModelv2-NativeClient-DotNet-TodoListService API` oder den Namen aus, den Sie für die Web-API eingegeben haben.
   - Aktivieren Sie die Berechtigung **access_as_user**, wenn diese nicht bereits aktiviert ist. Verwenden Sie bei Bedarf das Suchfeld.
   - Wählen Sie die Schaltfläche **Berechtigungen hinzufügen** aus.

### <a name="configure-your-todolistclient-project"></a>Konfigurieren des *TodoListClient*-Projekts

1. Kopieren Sie im *Anwendungsregistrierungsportal* auf der Seite **Übersicht** den Wert der **Anwendungs-ID (Client)** .
1. Öffnen Sie die **app.config**-Datei, die sich im Stammordner des **TodoListClient**-Projekts befindet, und fügen Sie den Wert in den `ida:ClientId`-Parameterwert ein.

## <a name="run-your-project"></a>Ausführen des Projekts

1. Drücken Sie die Taste `<F5>`, um das Projekt auszuführen. *TodoListClient* wird geöffnet.
1. Wählen Sie oben rechts die Option **Anmelden** aus, und melden Sie sich mit dem gleichen Benutzer, den Sie zum Registrieren der Anwendung verwendet haben, oder einem Benutzer aus dem gleichen Verzeichnis an.
1. Wenn Sie sich zum ersten Mal anmelden, werden Sie möglicherweise aufgefordert, der *TodoListService*-Web-API zuzustimmen.
1. Die Anmeldung fordert auch das Zugriffstoken für den Bereich *access_as_user* an, mit dem Sie auf die *TodoListService*-Web-API zugreifen und die *To-Do*-Liste bearbeiten können.

## <a name="pre-authorize-your-client-application"></a>Vorautorisieren der Clientanwendung

Um Benutzern aus anderen Verzeichnissen den Zugriff auf Ihre Web-API zu ermöglichen, können Sie die Clientanwendungen für den Zugriff auf Ihre Web-API *vorab autorisieren*. Dazu fügen Sie die Anwendungs-IDs der Clientanwendungen der Liste der *vorab autorisierten* Anwendungen für Ihre Web-API hinzu. Indem Sie einen vorautorisierten Client hinzufügen, müssen Sie die Benutzer nicht auffordern, der Verwendung Ihrer Web-API zuzustimmen. Führen Sie die folgenden Schritte aus, um Ihre Webanwendung vorab zu autorisieren:

1. Wechseln Sie wieder zum *Anwendungsregistrierungsportal*, und öffnen Sie die Eigenschaften Ihres **TodoListService**.
1. Klicken Sie im Abschnitt **API verfügbar machen** im Bereich *Autorisierte Clientanwendungen* auf **Clientanwendung hinzufügen**.
1. Fügen Sie die Anwendungs-ID der `TodoListClient`-Anwendung in das Feld *Client-ID* ein.
1. Wählen Sie im Abschnitt *Autorisierte Bereiche* den Bereich für diese Web-API aus: `api://<Application ID>/access_as_user`.
1. Klicken Sie unten auf der Seite auf die Schaltfläche **Anwendung hinzufügen**.

## <a name="run-your-project"></a>Ausführen des Projekts

1. Drücken Sie die Taste `<F5>`, um das Projekt auszuführen. *TodoListClient* wird geöffnet.
1. Wählen Sie oben rechts die Option **Anmelden** (oder „Cache löschen/Anmelden“) aus, und melden Sie sich mit einem persönlichen Microsoft-Konto (live.com oder hotmail.com) oder einem Geschäfts-, Schul- oder Unikonto an.

## <a name="optional-restrict-sign-in-access-to-your-application"></a>Optional: Beschränken des Anmeldezugriffs auf Ihre Anwendung

Wenn Sie dieses Codebeispiel herunterladen und die Anwendung mit den oben genannten Schritten für die Verwendung des Azure Active Directory v2-Endpunkts konfigurieren, können standardmäßig sowohl persönliche Konten (outlook.com, live.com und weitere) als auch Geschäfts-, Schul- oder Unikonten aus jeder in Azure AD integrierten Organisation Token anfordern und auf Ihre Web-API zugreifen. 

Um die Anmeldung auf Ihre Anwendung einzuschränken, verwenden Sie eine der folgenden Optionen:

### <a name="option-1-restrict-access-to-a-single-organization-single-tenant"></a>Option 1: Beschränken des Zugriffs auf eine einzelne Organisation (Einzelmandant)

Sie können den Anmeldezugriff für Ihre Anwendung auf bestimmte Benutzerkonten beschränken, die in einem einzelnen Azure AD-Mandanten vorhanden sind, einschließlich *Gastkonten* dieses Mandanten. Dieses Szenario wird häufig für *Branchenanwendungen* eingesetzt:

1. Öffnen Sie die Datei **App_Start\Startup.Auth**, und ändern Sie den Wert des Metadatenendpunkts, der an den `OpenIdConnectSecurityTokenProvider` übergeben wird, in `"https://login.microsoftonline.com/{Tenant ID}/v2.0/.well-known/openid-configuration"` (Sie können auch den Mandantennamen verwenden, beispielsweise `contoso.onmicrosoft.com`).
2. Legen Sie in der gleichen Datei die `ValidIssuer`-Eigenschaft für `TokenValidationParameters` auf `"https://sts.windows.net/{Tenant ID}/"` und das `ValidateIssuer`-Argument auf `true` fest.

### <a name="option-2-use-a-custom-method-to-validate-issuers"></a>Option 2: Überprüfen von Ausstellern mithilfe einer benutzerdefinierten Methode

Sie können eine benutzerdefinierte Methode implementieren, um Aussteller mit dem **IssuerValidator**-Parameter zu überprüfen. Weitere Informationen zur Verwendung dieses Parameters finden Sie unter [TokenValidationParameters-Klasse](/dotnet/api/microsoft.identitymodel.tokens.tokenvalidationparameters?view=azure-dotnet).

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu dem Szenario mit geschützten Web-APIs, das von der Microsoft Identity Platform unterstützt wird, finden Sie hier:
> [!div class="nextstepaction"]
> [Szenario: Geschützte Web-API](scenario-protected-web-api-overview.md)
