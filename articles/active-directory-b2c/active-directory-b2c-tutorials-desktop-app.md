---
title: 'Tutorial: Authentifizieren von Benutzern in einer nativen Clientanwendung – Azure Active Directory B2C'
description: Tutorial zur Verwendung von Azure Active Directory B2C zum Bereitstellen einer Benutzeranmeldung für eine .NET-Desktopanwendung.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 10/12/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: cd0fc90988048f98be46370d2c7836d9506cc44a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73475267"
---
# <a name="tutorial-authenticate-users-in-a-native-desktop-client-using-azure-active-directory-b2c"></a>Tutorial: Authentifizieren von Benutzern in einem nativen Desktopclient mithilfe von Azure Active Directory B2C

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory B2C (Azure AD B2C) für die Anmeldung und Registrierung von Benutzern in einer WPF-Desktop-Anwendung (Windows Presentation Foundation) verwenden. Mit Azure AD B2C können sich Ihre Anwendungen über offene Standardprotokolle bei Konten für soziale Netzwerke sowie bei Unternehmenskonten und Azure Active Directory-Konten authentifizieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen der nativen Clientanwendung
> * Konfigurieren des Beispiels für die Verwendung der Anwendung
> * Anmelden über den Benutzerflow

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

- [Erstellen Sie Benutzerflows](tutorial-create-user-flows.md), um Benutzererfahrungen in Ihrer Anwendung zu aktivieren.
- Installieren Sie [Visual Studio 2019](https://www.visualstudio.com/downloads/) mit den Workloads **.NET-Desktopentwicklung** und **ASP.NET und Webentwicklung**.

## <a name="add-the-native-client-application"></a>Hinzufügen der nativen Clientanwendung

[!INCLUDE [active-directory-b2c-appreg-native](../../includes/active-directory-b2c-appreg-native.md)]

Notieren Sie sich die **Anwendungs-ID (Client)** zur Verwendung in einem späteren Schritt.

## <a name="configure-the-sample"></a>Das Beispiel konfigurieren

In diesem Tutorial konfigurieren Sie ein Beispiel, das Sie von GitHub herunterladen können. Das Beispiel für die WPF-Desktopanwendung veranschaulicht die Registrierung und Anmeldung und kann eine geschützte Web-API in Azure AD B2C aufrufen. [Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip), [durchsuchen Sie das Repository](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop), oder klonen Sie das Beispiel aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

So aktualisieren Sie die Anwendung, sodass sie mit Ihrem Azure AD B2C-Mandanten arbeitet und seine Benutzerflows anstelle der Flows im standardmäßigen Demomandanten aufruft:

1. Öffnen Sie die Projektmappe **active-directory-b2c-wpf** (`active-directory-b2c-wpf.sln`) in Visual Studio.
2. Öffnen Sie im Projekt **active-directory-b2c-wpf** die Datei *App.xaml.cs*, und suchen Sie die folgenden Variablendefinitionen. Ersetzen Sie `{your-tenant-name}` durch den Namen Ihres Azure AD B2C-Mandanten und `{application-ID}` durch die zuvor notierte Anwendungs-ID.

    ```csharp
    private static readonly string Tenant = "{your-tenant-name}.onmicrosoft.com";
    private static readonly string AzureAdB2CHostname = "{your-tenant-name}.b2clogin.com";
    private static readonly string ClientId = "{application-ID}";
    ```

3. Aktualisieren Sie die Variablen für die Richtliniennamen mit den Namen der Benutzerflows, die Sie zur Vorbereitung erstellt haben. Beispiel:

    ```csharp
    public static string PolicySignUpSignIn = "B2C_1_signupsignin1";
    public static string PolicyEditProfile = "B2C_1_profileediting1";
    public static string PolicyResetPassword = "B2C_1_passwordreset1";
    ```

## <a name="run-the-sample"></a>Ausführen des Beispiels

Drücken Sie **F5**, um das Beispiel zu erstellen und auszuführen.

### <a name="sign-up-using-an-email-address"></a>Registrieren mit einer E-Mail-Adresse

1. Wählen Sie **Anmelden** aus, um sich als Benutzer zu registrieren. Dabei wird der Benutzerflow **B2C_1_signupsignin1** verwendet.
2. In Azure AD B2C wird eine Anmeldeseite mit dem Link **Jetzt registrieren** angezeigt. Da Sie noch nicht über ein Konto verfügen, klicken Sie auf den Link **Jetzt registrieren**.
3. Der Registrierungsworkflow zeigt eine Seite an, über die die Identität des Benutzers mithilfe einer E-Mail-Adresse erfasst und überprüft wird. Darüber hinaus werden im Rahmen des Registrierungsworkflows auch das Kennwort des Benutzers sowie die angeforderten Attribute erfasst, die im Benutzerflow definiert wurden.

    Verwenden Sie eine gültige E-Mail-Adresse, und bestätigen Sie sie mithilfe des Prüfcodes. Legen Sie ein Kennwort fest. Geben Sie Werte für die angeforderten Attribute ein.

    ![Die Registrierungsseite wird im Rahmen des Anmelde-/Registrierungsworkflows angezeigt.](media/active-directory-b2c-tutorials-desktop-app/azure-ad-b2c-sign-up-workflow.png)

4. Wählen Sie **Erstellen**, um im Azure AD B2C-Mandanten ein lokales Konto zu erstellen.

Der Benutzer kann sich nun mithilfe seiner E-Mail-Adresse anmelden und die Desktopanwendung verwenden. Nach einer erfolgreichen Registrierung oder Anmeldung werden die Tokendetails im unteren Bereich der WPF-App angezeigt.

![Im unteren Bereich der WPF-Desktopanwendung angezeigte Tokendetails](media/active-directory-b2c-tutorials-desktop-app/desktop-app-01-post-signin.png)

Wenn Sie die Schaltfläche **API aufrufen** auswählen, wird eine **Fehlermeldung** angezeigt. Der Fehler tritt auf, weil die Anwendung in ihrem aktuellen Zustand versucht, auf eine vom Demomandanten (`fabrikamb2c.onmicrosoft.com`) geschützte API zuzugreifen. Da Ihr Zugriffstoken nur für Ihren Azure AD B2C-Mandanten gilt, ist der API-Aufruf nicht autorisiert.

Fahren Sie mit dem nächsten Tutorial fort, um eine geschützte Web-API in Ihrem eigenen Mandanten zu registrieren und die Funktion **API aufrufen** zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Hinzufügen der nativen Clientanwendung
> * Konfigurieren des Beispiels für die Verwendung der Anwendung
> * Anmelden über den Benutzerflow

Wenn Sie die Schaltflächenfunktion **API aufrufen** aktivieren möchten, gewähren Sie der WPF-Desktopanwendung Zugriff auf eine Web-API, die in Ihrem eigenen Azure AD B2C-Mandanten registriert ist:

> [!div class="nextstepaction"]
> [Tutorial: Gewähren des Zugriffs auf eine Node.js-Web-API über eine Desktop-App unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-desktop-app-webapi.md).
