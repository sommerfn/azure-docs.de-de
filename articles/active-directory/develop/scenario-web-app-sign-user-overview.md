---
title: Web-App, die Benutzer anmeldet (Übersicht) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Benutzer anmeldet (Übersicht).
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
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 68d47d4233aec62ec5f1955e52025b0d55221af8
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72596716"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Szenario: Web-App, die Benutzer anmeldet

Erfahren Sie, wie Sie eine Web-App erstellen, die Benutzer mit Microsoft Identity Plattform anmeldet.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Erste Schritte

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Wenn Sie Ihre ersten portablen ASP.NET Core-Web-Apps erstellen möchten, die Benutzer anmelden, führen Sie die Schritte im folgenden Schnellstart aus:

> [!div class="nextstepaction"]
> [Schnellstart: ASP.NET Core-Web-App, die Benutzer anmeldet](quickstart-v2-aspnet-core-webapp.md)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Wenn Sie wissen möchten, wie Sie die Anmeldung bei einer Legacy-ASP.NET-Webanwendung hinzufügen, probieren Sie das folgende Tutorial aus:

> [!div class="nextstepaction"]
> [Schnellstart: ASP.NET-Web-App für die Benutzeranmeldung](quickstart-v2-aspnet-webapp.md)

# <a name="javatabjava"></a>[Java](#tab/java)

Wenn Sie Java-Entwickler sind, probieren Sie den folgenden Schnellstart aus:

> [!div class="nextstepaction"]
> [Schnellstart: Hinzufügen von „Mit Microsoft anmelden“ zu einer Java-Web-App](quickstart-v2-java-webapp.md)

# <a name="pythontabpython"></a>[Python](#tab/python)

Wenn Sie mit Python entwickeln, probieren Sie Folgendes aus:

> [!div class="nextstepaction"]
> [Schnellstart: Hinzufügen von „Mit Microsoft anmelden“ zu einer Python-Web-App](quickstart-v2-python-webapp.md)

---

## <a name="overview"></a>Übersicht

Wenn Sie für Ihre Web-App eine Authentifizierung einsetzen, können Benutzer bei der App angemeldet werden. Dadurch kann Ihre App auf eingeschränkte Profilinformationen zugreifen und beispielsweise Funktionen für bestimmte Benutzer anpassen. Web-Apps authentifizieren Benutzer in einem Webbrowser. In diesem Szenario weist die Webanwendung den Browser des Benutzers an, diesen in Azure AD anzumelden. Azure AD gibt über den Browser des Benutzers eine Anmeldeantwort mit Benutzeransprüchen in einem Sicherheitstoken zurück. Bei der Anmeldung von Benutzern wird das Standardprotokoll [OpenID Connect](./v2-protocols-oidc.md) genutzt. Der Anmeldevorgang wird durch die Verwendung von [Middlewarebibliotheken](scenario-web-app-sign-user-app-configuration.md#libraries-used-to-protect-web-apps) noch vereinfacht.

![Web-App, die Benutzer anmeldet](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

In einem zweiten Schritt können Sie die Anwendung so konfigurieren, dass sie für den angemeldeten Benutzer Web-APIs aufruft. Bei diesem nächsten Schritt handelt es sich um ein anderes Szenario, das unter [Web-App ruft Web-APIs auf](scenario-web-app-call-api-overview.md) beschrieben wird.

> [!NOTE]
> Wenn Sie Ihre Web-App so konfigurieren, dass eine Anmeldung erforderlich ist, schützten Sie sie auf diese Weise. Außerdem wird ein Benutzertoken mithilfe von **Middlewarebilbliotheken** überprüft. Im Fall von .NET sind für dieses Szenario noch keine Microsoft Authentication Libraries (MSAL) erforderlich, mit denen ein Token zum Aufruf geschützter APIs abgerufen werden kann. Die Authentifizierungsbibliotheken werden erst im nächsten Szenario eingeführt, wenn die Web-App Web-APIs aufrufen muss.

## <a name="specifics"></a>Besonderheiten

- Bei der Anwendungsregistrierung müssen Sie eine oder – wenn die App an mehreren Standorten bereitgestellt wird – mehrere Antwort-URIs angeben. In einigen Fällen (etwa bei ASP.NET oder ASP.NET Core) müssen Sie das ID-Token aktivieren. Abschließend sollten Sie einen Abmelde-URI einrichten, damit Ihre Anwendung entsprechend reagiert, wenn sich ein Benutzer abmeldet.
- Im Code für Ihre Anwendung müssen Sie die Autorität angeben, an die Ihre Web-App die Anmeldung delegiert. Sie sollten außerdem die Tokenüberprüfung (vor allem in ISV-Szenarios) anpassen.
- Webanwendungen unterstützen alle Kontotypen. Weitere Informationen finden Sie unter [Supported account types (Unterstützte Kontotypen)](v2-supported-account-types.md).

## <a name="next-steps"></a>Nächste Schritte

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [App-Registrierung](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=aspnetcore?tabs=aspnetcore)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [App-Registrierung](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=aspnet)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [App-Registrierung](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=java)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [App-Registrierung](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-app-registration?tabs=python)

---
