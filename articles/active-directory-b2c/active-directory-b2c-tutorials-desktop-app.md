---
title: 'Tutorial: Aktivieren der Authentifizierung in einer nativen Clientanwendung – Azure Active Directory B2C | Microsoft-Dokumentation'
description: Tutorial zur Verwendung von Azure Active Directory B2C zum Bereitstellen einer Benutzeranmeldung für eine .NET-Desktopanwendung.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 3740a032db6ca9fd0fb88ce348610684d9f895bc
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71326325"
---
# <a name="tutorial-enable-authentication-in-a-native-client-application-using-azure-active-directory-b2c"></a>Tutorial: Aktivieren der Authentifizierung in einer nativen Clientanwendung mit Azure Active Directory B2C

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

Notieren Sie sich die **ANWENDUNGS-ID** zur Verwendung in einem späteren Schritt.

## <a name="configure-the-sample"></a>Das Beispiel konfigurieren

In diesem Tutorial konfigurieren Sie ein Beispiel, das Sie von GitHub herunterladen können. Das Beispiel für die WPF-Desktopanwendung veranschaulicht die Registrierung und Anmeldung und ruft eine geschützte Web-API in Azure AD B2C auf. [Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip), [durchsuchen Sie das Repository](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop), oder klonen Sie das Beispiel aus GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

Ersetzen Sie zum Ändern der App-Einstellungen `<your-tenant-name>` durch Ihren Mandantennamen und `<application-ID`> durch die notierte Anwendungs-ID.

1. Öffnen Sie die Projektmappe `active-directory-b2c-wpf` in Visual Studio.
2. Öffnen Sie im Projekt `active-directory-b2c-wpf` die Datei **App.xaml.cs**, und nehmen Sie die folgenden Änderungen vor:

    ```csharp
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "<application-ID>";
    ```

3. Aktualisieren Sie die Variable **PolicySignUpSignIn** durch den Namen des von Ihnen erstellten Benutzerflows.

    ```csharp
    public static string PolicySignUpSignIn = "B2C_1_signupsignin1";
    ```

## <a name="run-the-sample"></a>Ausführen des Beispiels

Drücken Sie **F5**, um das Beispiel zu erstellen und auszuführen.

### <a name="sign-up-using-an-email-address"></a>Registrieren mit einer E-Mail-Adresse

1. Klicken Sie auf **Anmelden**, um sich als Benutzer zu registrieren. Dabei wird der Benutzerflow **B2C_1_signupsignin1** verwendet.
2. Azure AD B2C zeigt eine Anmeldeseite mit einem Registrierungslink an. Da Sie noch nicht über ein Konto verfügen, klicken Sie auf den Link **Jetzt registrieren**.
3. Der Registrierungsworkflow zeigt eine Seite an, über die die Identität des Benutzers mithilfe einer E-Mail-Adresse erfasst und überprüft wird. Darüber hinaus werden im Rahmen des Registrierungsworkflows auch das Kennwort des Benutzers sowie die angeforderten Attribute erfasst, die im Benutzerflow definiert wurden.

    Verwenden Sie eine gültige E-Mail-Adresse, und bestätigen Sie sie mithilfe des Prüfcodes. Legen Sie ein Kennwort fest. Geben Sie Werte für die angeforderten Attribute ein.

    ![Die Registrierungsseite wird im Rahmen des Anmelde-/Registrierungsworkflows angezeigt.](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.PNG)

4. Klicken Sie auf **Erstellen**, um im Azure AD B2C-Mandanten ein lokales Konto zu erstellen.

Nun kann sich der Benutzer mithilfe seiner E-Mail-Adresse anmelden und die Desktop-App verwenden.

> [!NOTE]
> Wenn Sie auf die Schaltfläche **API aufrufen** klicken, wird der Fehler „Nicht autorisiert“ angezeigt. Der Fehler wird angezeigt, da Sie versuchen, über den Demomandanten auf eine Ressource zuzugreifen. Da Ihr Zugriffstoken nur für Ihren Azure AD-Mandanten gilt, ist der API-Aufruf nicht autorisiert. Fahren Sie mit dem nächsten Tutorial fort, um eine geschützte Web-API für Ihren Mandanten zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Hinzufügen der nativen Clientanwendung
> * Konfigurieren des Beispiels für die Verwendung der Anwendung
> * Anmelden über den Benutzerflow

> [!div class="nextstepaction"]
> [Tutorial: Gewähren des Zugriffs auf eine Node.js-Web-API über eine Desktop-App unter Verwendung von Azure Active Directory B2C](active-directory-b2c-tutorials-spa-webapi.md)
