---
title: Web-App für Benutzeranmeldungen (App-Registrierung) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Benutzer anmeldet (App-Registrierung)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 641f71f6111930b54d0a2bd548f16d3cb0c07189
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175265"
---
# <a name="web-app-that-signs-in-users---app-registration"></a>Web-App für Benutzeranmeldungen – App-Registrierung

Auf dieser Seite werden die Besonderheiten der App-Registrierung für eine Web-App erläutert, mit der Benutzer angemeldet werden.

Für das Registrieren Ihrer Anwendung können Sie Folgendes verwenden:

- Die [Web-App-Schnellstarts](#register-an-app-using-the-quickstarts) – Schnellstarts im Azure-Portal vermitteln nicht nur erste Erfahrungen beim Erstellen einer Anwendung, sondern umfassen auch die Schaltfläche **Make this change for me** (Diese Änderung für mich durchführen). Sie können diese Schaltfläche verwenden, um die benötigten Eigenschaften festzulegen. Dies gilt auch für eine bereits vorhandene App. Sie müssen die Werte dieser Eigenschaften an Ihren konkreten Fall anpassen. Insbesondere wird sich die Web-API-URL für Ihre App wahrscheinlich vom vorgeschlagenen Standardwert unterscheiden, was sich auch auf den Abmelde-URI auswirkt.
- Das Azure-Portal für die [manuelle Registrierung Ihrer Anwendung](#register-an-app-using-azure-portal)
- PowerShell und Befehlszeilentools

## <a name="register-an-app-using-the-quickstarts"></a>Registrieren einer App mithilfe der Schnellstarts

Wenn Sie zu diesem Link navigieren, können Sie einen Bootstrap zur Erstellung Ihrer Web-App durchführen:

- [ASP.NET Core](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
- [ASP.NET](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

## <a name="register-an-app-using-azure-portal"></a>Registrieren einer App mit dem Azure-Portal

> [!NOTE]
> Welches Portal Sie verwenden müssen, hängt davon ab, ob Ihre Anwendung in der öffentlichen Cloud von Microsoft Azure oder in einer nationalen oder Sovereign Cloud ausgeführt wird. Weitere Informationen finden Sie unter [Nationale Clouds](./authentication-national-cloud.md#app-registration-endpoints).


1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an. Melden Sie sich alternativ am Azure-Portal der nationalen Cloud Ihrer Wahl an.
1. Wenn Sie mit Ihrem Konto auf mehrere Mandanten zugreifen können, klicken Sie rechts oben auf Ihr Konto, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** und anschließend **App-Registrierungen** > **Neue Registrierung** aus.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

4. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** die Registrierungsinformationen für Ihre Anwendung ein:
   1. Wählen Sie die unterstützten Kontotypen für Ihre Anwendung aus (siehe [Supported Account types (Unterstützte Kontotypen)](./v2-supported-account-types.md)).
   1. Geben Sie im Abschnitt **Name** einen aussagekräftigen Anwendungsnamen ein, der den Benutzern der App angezeigt wird (beispielsweise `AspNetCore-WebApp`).
   1. Fügen Sie in **Umleitungs-URI** den Anwendungstyp und das URI-Ziel hinzu, das nach einer erfolgreichen Authentifizierung zurückgegebene Tokenantworten akzeptiert. Beispiel: `https://localhost:44321/`.  Wählen Sie **Registrieren**.
1. Wählen Sie das Menü **Authentifizierung** aus, und fügen Sie dann die folgenden Informationen hinzu:
   1. Fügen Sie in **Antwort-URL** die Zeichenfolge `https://localhost:44321/signin-oidc` vom Typ „Web“ hinzu.
   1. Legen Sie im Abschnitt **Erweiterte Einstellungen** die Option **Abmelde-URL** auf `https://localhost:44321/signout-oidc` fest.
   1. Aktivieren Sie unter **Implizite Genehmigung** die Option **ID-Token**.
   1. Wählen Sie **Speichern** aus.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

4. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** die Registrierungsinformationen Ihrer Anwendung ein:
   1. Wählen Sie die unterstützten Kontotypen für Ihre Anwendung aus (siehe [Supported Account types (Unterstützte Kontotypen)](./v2-supported-account-types.md)).
   - Geben Sie im Abschnitt **Name** einen aussagekräftigen Anwendungsnamen ein, der den Benutzern der App angezeigt wird (beispielsweise `MailApp-openidconnect-v2`).
   - Wählen Sie im Abschnitt „Umleitungs-URI (optional)“ im Kombinationsfeld die Option **Web** aus, und geben Sie den Umleitungs-URI `https://localhost:44326/` ein.
1. Wählen Sie **Registrieren** aus, um die Anwendung zu erstellen.
1. Wählen Sie das Menü **Authentifizierung** aus, und fügen Sie dann die folgenden Informationen hinzu:
   - Aktivieren Sie im Abschnitt **Erweiterte Einstellungen** | **Implizite Gewährung** die Option **ID-Token**, weil in diesem Beispiel der [Flow für implizite Genehmigungen](v2-oauth2-implicit-grant-flow.md) aktiviert werden muss, um den Benutzer anzumelden.
1. Wählen Sie **Speichern** aus.

# <a name="javatabjava"></a>[Java](#tab/java)

4. Wenn die Seite **Anwendung registrieren** angezeigt wird, geben Sie einen Anzeigenamen für die Anwendung (z. B. „java-webapp“) ein, wählen Sie „Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten (z. B. Skype, Xbox, Outlook.com)“ aus, und wählen Sie dann „Web-App/API“ als *Anwendungstyp* aus.
1. Klicken Sie auf **Registrieren**, um die Anwendung zu registrieren.
1. Klicken Sie im Menü auf der linken Seite auf **Authentifizierung**, und wählen Sie unter *Umleitungs-URIs* die Option „Web“ aus. Sie müssen zwei verschiedene Umleitungs-URIs eingeben: einen für die Anmeldeseite und einen für die Graph-Seite. Sie sollten für beide denselben Host und dieselbe Portnummer verwenden, gefolgt von „/msal4jsample/secure/aad“ für die Anmeldeseite und „msal4jsample/graph/me“ für die Benutzerinfoseite.
 Im Beispiel wird standardmäßig Folgendes verwendet:

    - `http://localhost:8080/msal4jsample/secure/aad`.
    - `http://localhost:8080/msal4jsample/graph/me`

    Legen Sie im Abschnitt **Erweiterte Einstellungen** die Option **Abmelde-URL** auf `http://localhost:8080/msal4jsample/sign_out` fest.

     Klicken Sie auf **Speichern**.

1. Wählen Sie im Menü **Certificates & secrets** (Zertifikate und Geheimnisse) aus, und klicken Sie im Abschnitt **Geheime Clientschlüssel** auf **Neuer geheimer Clientschlüssel**:

    - Geben Sie eine Beschreibung für den Schlüssel ein.
    - Wählen Sie als Schlüsseldauer die Option **In 1 Jahr** aus.
    - Der Schlüsselwert wird angezeigt, wenn Sie **Hinzufügen** auswählen.
    - Kopieren Sie den Wert des Schlüssels zur späteren Verwendung. Der Schlüsselwert wird nicht erneut angezeigt und kann auch nicht auf andere Weise abgerufen werden. Erfassen Sie ihn daher, sobald er im Azure-Portal angezeigt wird.

# <a name="pythontabpython"></a>[Python](#tab/python)

4. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** die Registrierungsinformationen Ihrer Anwendung ein:
   - Geben Sie im Abschnitt **Name** einen aussagekräftigen Anwendungsnamen ein, der den Benutzern der App angezeigt wird (beispielsweise `python-webapp`).
   - Ändern Sie **Unterstützte Kontotypen** in **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten (z. B. Skype, Xbox, Outlook.com)** .
   - Wählen Sie im Abschnitt „Umleitungs-URI (optional)“ im Kombinationsfeld die Option **Web** aus, und geben Sie den Umleitungs-URI `http://localhost:5000/getAToken` ein.
1. Wählen Sie **Registrieren** aus, um die Anwendung zu erstellen.
1. Suchen Sie auf der Seite **Übersicht** den Wert von **Anwendungsclient-ID** und notieren Sie ihn zur späteren Verwendung. Sie benötigen diesen Wert, um die Visual Studio-Konfigurationsdatei für dieses Projekt zu konfigurieren.
1. Wählen Sie auf der Seite „Übersicht“ der App den Abschnitt **Authentifizierung** aus.
   - Legen Sie im Abschnitt **Erweiterte Einstellungen** die Option **Abmelde-URL** auf `http://localhost:5000/logout` fest.

  Wählen Sie **Speichern** aus.
1. Wählen Sie im linken Menü die Option **Certificates & secrets** (Zertifikate und Geheimnisse) aus, und klicken Sie im Abschnitt **Geheime Clientschlüssel** auf **Neuer geheimer Clientschlüssel**:

      - Geben Sie eine Beschreibung für den Schlüssel ein.
      - Wählen Sie als Schlüsseldauer die Option **In 1 Jahr** aus.
      - Wenn Sie auf **Hinzufügen** klicken, wird der Schlüsselwert angezeigt.
      - Kopieren Sie den Wert des Schlüssels. Sie benötigen sie später.
---

## <a name="register-an-app-using-powershell"></a>Registrieren einer App mit PowerShell

> [!NOTE]
> Derzeit erstellt Azure AD PowerShell nur Anwendungen mit den folgenden unterstützten Kontotypen:
>
> - MyOrg (nur Konten in diesem Organisationsverzeichnis)
> - AnyOrg (Konten in einem beliebigen Organisationsverzeichnis)
>
> Wenn Sie eine Anwendung erstellen möchten, die Benutzer mit ihren persönlichen Microsoft-Konten (z. B. Skype, XBox, Outlook.com) anmeldet, können Sie zunächst eine Anwendung mit mehreren Mandanten erstellen (unterstützte Kontotypen = Konten in einem beliebigen Organisationsverzeichnis) und anschließend die `signInAudience`-Eigenschaft in das Anwendungsmanifest aus dem Azure-Portal ändern. Dies wird in Schritt [1.3](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-3-AnyOrgOrPersonal#step-1-register-the-sample-with-your-azure-ad-tenant) des ASP.NET Core-Tutorials ausführlich erläutert (und kann allgemein auf Web-Apps in einer beliebigen Sprache übertragen werden).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Codekonfiguration der App](scenario-web-app-sign-user-app-configuration.md)
