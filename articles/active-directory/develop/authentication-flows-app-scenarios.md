---
title: Authentifizierungsszenarien für Microsoft Identity Platform | Azure
description: Erfahren Sie mehr über die Authentifizierungsflows und Anwendungsszenarien in Microsoft Identity Platform. Lernen Sie die verschiedenen Anwendungstypen kennen, die Identitäten authentifizieren, Token abrufen und geschützte APIs aufrufen können.
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
ms.date: 07/15/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d50019e8de1daf3d69342dcaf9eeecfba493a83
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302429"
---
# <a name="authentication-flows-and-application-scenarios"></a>Authentifizierungsflows und Anwendungsszenarien

Der Microsoft Identity Platform (v2.0)-Endpunkt unterstützt die Authentifizierung für eine Vielzahl moderner App-Architekturen, die alle auf den branchenüblichen Standardprotokollen [OAuth 2.0 oder OpenID Connect](active-directory-v2-protocols.md) basieren.  Anwendungen authentifizieren mithilfe von [Authentifizierungsbibliotheken](reference-v2-libraries.md) Identitäten und rufen Token für den Zugriff auf geschützte APIs ab. In diesem Artikel werden die verschiedenen Authentifizierungsflows und Anwendungsszenarien beschrieben, in denen sie verwendet werden.  Dieser Artikel enthält außerdem eine Liste der [Anwendungsszenarien und unterstützten Authentifizierungsflows](#scenarios-and-supported-authentication-flows) sowie eine Liste mit [Anwendungsszenarien und unterstützten Plattformen und Sprachen](#scenarios-and-supported-platforms-and-languages).

## <a name="application-categories"></a>Anwendungskategorien

Token können von einer Vielzahl von Anwendungstypen abgerufen werden: Webanwendungen, mobile Anwendungen oder Desktopanwendungen, Web-APIs und Anwendungen, die auf Geräten ohne Browser (oder IoT-Geräten) ausgeführt werden. Anwendungen lassen sich wie folgt in Kategorien unterteilen:

- [Geschützte Ressourcen oder Clientanwendungen](#protected-resources-vs-client-applications). Bei einigen Szenarien geht es um den Schutz von Ressourcen (Web-Apps oder Web-APIs), bei anderen handelt es sich um das Abrufen eines Sicherheitstokens zum Aufrufen einer geschützten Web-API.
- [Mit Benutzern oder ohne Benutzer](#with-users-or-without-users). Bei einigen Szenarien gibt es einen angemeldeten Benutzer, bei anderen ist kein Benutzer beteiligt (Daemon-Szenarien).
- [Single-Page-Anwendungen, öffentliche Client Anwendungen und vertrauliche Client-Anwendungen](#single-page-applications-public-client-applications-and-confidential-client-applications). Diese Anwendungen bilden drei große Kategorien von Anwendungstypen. Zum Bearbeiten dieser Anwendungstypen werden unterschiedliche Bibliotheken und Objekte verwendet.
- [Zielgruppe für die Anmeldung](v2-supported-account-types.md#certain-authentication-flows-dont-support-all-the-account-types). Einige Authentifizierungsflows sind für bestimmte Anmeldungszielgruppen nicht verfügbar. Einige Flows sind nur für Geschäfts-, Schul- oder Unikonten verfügbar, andere stehen für Geschäfts-, Schul- oder Unikonten und für persönliche Microsoft-Konten zur Verfügung. Die zulässige Zielgruppe hängt von den Authentifizierungsflows ab.
- [Unterstützte OAuth 2.0-Authentifizierungsflows](#scenarios-and-supported-authentication-flows).  Authentifizierungsflows werden zum Implementieren der Anwendungsszenarien verwendet, die Token anfordern.  Anwendungsszenarien und Authentifizierungsflows lassen sich nicht eins zu eins zuordnen.
- [Unterstützte Plattformen](#scenarios-and-supported-platforms-and-languages). Nicht alle Anwendungsszenarien sind für jede Plattform verfügbar.

### <a name="protected-resources-vs-client-applications"></a>Geschützte Ressourcen oder Client Anwendungen

Die Authentifizierungsszenarien beinhalten zwei Aktivitäten:

- **Abrufen von Sicherheitstoken** für eine geschützte Web-API. Microsoft empfiehlt zum Abrufen von Token die Verwendung von [Authentifizierungsbibliotheken](reference-v2-libraries.md#microsoft-supported-client-libraries), insbesondere die MSAL-Familie (MicroSoft Authentification Libraries).
- **Schützen einer Web-API** (oder Web-App). Eine der Herausforderungen beim Schützen einer Ressource (Web-App oder Web-API) besteht darin, das Sicherheitstoken zu überprüfen. Auf einigen Plattformen bietet Microsoft [Bibliotheken der Middleware](reference-v2-libraries.md#microsoft-supported-server-middleware-libraries) an.

### <a name="with-users-or-without-users"></a>Mit Benutzern oder ohne Benutzer

Bei den meisten Authentifizierungsszenarien werden Token im Namen eines (angemeldeten) **Benutzers** erworben.

![Szenarien mit Benutzern](media/scenarios/scenarios-with-users.svg)

Es gibt jedoch auch Szenarien (Daemon-Apps), bei denen die Anwendungen Token für sich selbst (ohne Benutzer) abrufen.

![Daemon-Apps](media/scenarios/daemon-app.svg)

### <a name="single-page-applications-public-client-applications-and-confidential-client-applications"></a>Single-Page-Anwendungen, öffentliche Client Anwendungen und vertrauliche Client-Anwendungen

Die Sicherheitstoken können von einer Vielzahl von Anwendungstypen erworben werden. Die Anwendungen lassen sich tendenziell in drei Kategorien unterteilen:

- **Single-Page-Anwendungen** (SPAs) sind eine Form von Webanwendungen, bei denen Token von der im Browser ausgeführten App (geschrieben in JavaScript oder Typescript) erworben werden. Viele moderne Apps besitzen ein Single-Page-App-Front-End, das in erster Linie in JavaScript geschrieben ist. Häufig wird zum Schreiben der App ein Framework wie Angular, React oder Vue verwendet. MSAL.js ist die einzige Microsoft-Authentifizierungsbibliothek, die Single-Page-Anwendungen unterstützt.

- **Öffentliche Clientanwendungen** melden immer Benutzer an. Zu diesen Anwendungen zählen:
  - Desktopanwendungen, die Web-APIs im Namen des angemeldeten Benutzers aufrufen.
  - Mobile Anwendungen.
  - Anwendungen einer dritten Kategorie, die auf Geräten ohne Browser ausgeführt werden (browserlose Apps, die z.B. im IoT ausgeführt werden).

  Sie werden von der MSAL-Klasse namens [PublicClientApplication](msal-client-applications.md) dargestellt.

- **Vertrauliche Clientanwendungen**
  - Web-Anwendungen, die eine Web-API aufrufen
  - Web-APIs, die eine Web-API aufrufen
  - Daemonanwendungen (auch bei Implementierung als Konsolendienst wie Daemon unter Linux oder als Windows-Dienst)
 
  Für diese App-Typen wird [ConfidentialClientApplication](msal-client-applications.md) verwendet.

## <a name="application-scenarios"></a>Anwendungsszenarien

Der Microsoft Identity Platform-Endpunkt unterstützt die Authentifizierung für eine Vielzahl von App-Architekturen: Single-Page-Apps, Web-Apps, Web-APIs, mobile und native Apps sowie Daemons und serverseitige Apps.  Anwendungen verwenden die verschiedenen Authentifizierungsflows zum Anmelden von Benutzern und zum Abrufen von Token, um geschützte APIs aufzurufen.

### <a name="web-application-signing-in-a-user"></a>Web-App, die einen Benutzer anmeldet

![Web-App meldet Benutzer an](media/scenarios/scenario-webapp-signs-in-users.svg)

Zum **Schützen einer Web-App** (die den Benutzer anmeldet) verwenden Sie Folgendes:

- In der .NET-Umgebung verwenden Sie ASP.NET oder ASP.NET Core mit der Open ID Connect-Middleware für ASP.NET. Im Hintergrund umfasst der Schutz einer Ressource das Überprüfen des Sicherheitstokens, das durch die [IdentityModel-Erweiterungen für die .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki)-Bibliothek (und nicht durch MSAL-Bibliotheken) erfolgt.

- Bei der Entwicklung in Node.js verwenden Sie Passport.js.

Einzelheiten finden Sie unter [Web-App, die Benutzer anmeldet](scenario-web-app-sign-user-overview.md).

### <a name="web-application-signing-in-a-user-and-calling-a-web-api-on-behalf-of-the-user"></a>Web-App, die einen Benutzer anmeldet und im Namen des Benutzers eine Web-API aufruft

![Web-App ruft Web-APIs auf](media/scenarios/web-app.svg)

Um von der Web-App die **Web-API im Namen des Benutzers aufzurufen**, verwenden Sie die MSAL-Klasse `ConfidentialClientApplication`. Mithilfe des Autorisierungscodeflows speichern Sie das abgerufene Token im Tokencache. Anschließend ruft der Controller nach Bedarf automatisch Token aus dem Cache ab. Durch MSAL wird das Token bei Bedarf aktualisiert.

Einzelheiten finden Sie unter [Web-App, die Web-APIs aufruft](scenario-web-app-call-api-overview.md).

### <a name="desktop-application-calling-a-web-api-on-behalf-of-the-signed-in-user"></a>Desktopanwendung, die im Namen des angemeldeten Benutzers eine Web-API aufruft

Um von einer Desktopanwendung, die Benutzer anmeldet, eine Web-API aufzurufen, verwenden Sie die interaktiven Tokenabrufmethoden der MSAL-Klasse „PublicClientApplication“. Mit diesen interaktiven Methoden können Sie die Benutzeroberfläche für die Anmeldeumgebung steuern. MSAL nutzt einen Webbrowser, um diese Interaktion zu aktivieren.

![Desktop](media/scenarios/desktop-app.svg)

Für Anwendungen, die unter Windows gehostet und auf Computern ausgeführt werden, die zu einer Windows-Domäne gehören oder in AAD eingebunden sind, gibt es eine weitere Möglichkeit. Sie können ein Token automatisch mithilfe der [integrierten Windows-Authentifizierung](https://aka.ms/msal-net-iwa) abrufen.

Anwendungen, die auf einem Gerät ohne Browser ausgeführt werden, können weiterhin eine API im Namen eines Benutzers aufrufen. Zum Authentifizieren muss sich der Benutzer auf einem anderen Gerät mit einem Webbrowser anmelden. Zum Aktivieren dieses Szenario müssen Sie den [Gerätecodeflow](https://aka.ms/msal-net-device-code-flow) verwenden.

![Gerätecodeflow](media/scenarios/device-code-flow-app.svg)

Schließlich können Sie, obwohl dies nicht empfohlen wird, in öffentlichen Clientanwendungen [Benutzername und Kennwort](https://aka.ms/msal-net-up) verwenden. Bei einigen Szenarien (z.B. DevOps) ist zu beachten, dass dieser Flow zwar weiterhin erforderlich ist, seine Verwendung jedoch Einschränkungen für Ihre Anwendung bedeutet. Apps beispielsweise, die diesen Flow verwenden, können einen Benutzer, der eine mehrstufige Authentifizierung (bedingter Zugriff) durchführen muss, nicht anmelden. Auch kann Ihre Anwendung das einmalige Anmelden nicht nutzen. Diese Möglichkeit widerspricht außerdem den Prinzipien moderner Authentifizierung und steht nur aus Legacygründen zur Auswahl.

Wenn Sie in Desktopanwendungen den Tokencache dauerhaft beibehalten möchten, sollten Sie die [Serialisierung des Tokencaches](https://aka.ms/msal-net-token-cache-serialization) anpassen. Mit früheren Generationen von Authentifizierungsbibliotheken (Adal.NET 3.x und 4.x) können Sie sogar abwärts- und aufwärtskompatible Tokencaches aktivieren, indem Sie eine [duale Tokencacheserialisierung](https://aka.ms/msal-net-dual-cache-serialization) implementieren.

Einzelheiten finden Sie unter [Desktop-App, die Web-APIs aufruft](scenario-desktop-overview.md).

### <a name="mobile-application-calling-a-web-api-on-behalf-of-the-user-whos-signed-in-interactively"></a>Mobile Anwendung, die eine Web-API im Namen des Benutzers aufruft, der sich interaktiv angemeldet hat

![Mobile](media/scenarios/mobile-app.svg)

Wie Desktopanwendungen verwenden auch mobile Anwendungen die interaktiven Tokenabrufmethoden der MSAL-Klasse „PublicClientApplication“, um ein Token zum Aufrufen einer Web-API abzurufen. Unter iOS und Android verwendet MSAL standardmäßig den Webbrowser des Systems. Sie können jedoch die Verwendung der eingebetteten Webansicht angeben. Abhängig von der mobilen Plattform sind Besonderheiten zu beachten (UWP, iOS, Android).
Bei einigen Szenarien, die einen bedingten Zugriff im Zusammenhang mit der Geräte-ID oder ein Gerät enthalten, das registriert wird, muss ein [Broker](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/leveraging-brokers-on-Android-and-iOS) auf dem Gerät installiert sein. Beispiele für Broker sind das Microsoft-Unternehmensportal (Android) und Microsoft Authenticator (Android und iOS). MSAL kann jetzt mit Brokern interagieren.

> [!NOTE]
> Ihrer mobilen App (mithilfe von MSAL.iOS, MSAL.Android oder MSAL.NET/Xamarin) können App-Schutzrichtlinien zugewiesen werden (um z.B. zu verhindern, dass der Benutzer geschützten Text kopiert). Dies wird [von Intune verwaltet](https://docs.microsoft.com/intune/app-sdk), und die App wird von Intune als verwaltete App erkannt. Das [Intune-SDK](https://docs.microsoft.com/intune/app-sdk-get-started) ist von den MSAL-Bibliotheken getrennt und kommuniziert eigenständig mit AAD.

Einzelheiten finden Sie unter [Mobile App, die Web-APIs aufruft](scenario-mobile-overview.md).

### <a name="protected-web-api"></a>Geschützte Web-API

Mit dem Microsoft Identity Plattform-Endpunkt können Sie Webdienste schützen, z.B. die RESTful-Web-API Ihrer App. Eine geschützte Web-API wird zum Schutz der Daten und zum Authentifizieren eingehender Anfragen mit einem Zugriffstoken aufgerufen. Der Aufrufer einer Web-API fügt an den Autorisierungsheader einer HTTP-Anforderung ein Zugriffstoken an. Wenn Sie Ihre ASP.NET- oder ASP.NET Core-Web-API schützen möchten, müssen Sie das Zugriffstoken überprüfen. Hierfür verwenden Sie die JWT-Middleware für ASP.NET. Die Validierung erfolgt im Hintergrund durch die [IdentityModel-Erweiterungen für die .NET-Bibliothek](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki), nicht durch MSAL.NET.

Einzelheiten finden Sie unter [Geschützte Web-API](scenario-protected-web-api-overview.md).

### <a name="web-api-calling-another-downstream-web-api-on-behalf-of-the-user-for-whom-it-was-called"></a>Web-API, die im Namen des Benutzers, für den sie aufgerufen wurde, eine andere Downstream-Web-API aufruft

Wenn Ihre geschützte ASP.NET- oder ASP.NET Core-Web-API außerdem im Auftrag des Benutzers eine andere Web-API aufrufen soll, muss die App mit der Methode der MSAL-Klasse „ConfidentialClientApplication“ zum Abrufen eines Tokens [im Auftrag eines Benutzers](https://aka.ms/msal-net-on-behalf-of) ein Token für die Downstream-Web-API abrufen. Diese Aufrufe werden auch als Dienst-zu-Dienst-Aufrufe bezeichnet.
Web-APIs, die eine andere Web-API aufrufen, müssen auch eine benutzerdefinierte Cacheserialisierung bereitstellen.

  ![Web-API](media/scenarios/web-api.svg)

Einzelheiten finden Sie unter [Web-API, die Web-APIs aufruft](scenario-web-api-call-api-overview.md).

### <a name="desktopservice-or-web-daemon-application-calling-web-api-without-a-user-in-its-own-name"></a>Desktop-/Dienst- oder Web-Daemon-App, die eine Web-API ohne Benutzer (im eigenen Namen) aufruft

Apps, die Prozesse mit langer Ausführungszeit enthalten oder ohne Benutzereingriff ausgeführt werden, benötigen ebenfalls eine Möglichkeit, auf sichere Web-APIs zuzugreifen. Diese Apps können sich mithilfe der App-Identität (anstelle der delegierten Benutzeridentität) authentifizieren und Token abrufen. Sie weisen ihre Identität mit einem Clientgeheimnis oder einem Zertifikat nach.
Sie können solche Apps (Daemon-App) schreiben, um zusätzlich zu den Abrufmethoden der MSAL-Klasse „ConfidentialClientApplication“ mit [Clientanmeldeinformationen](https://aka.ms/msal-net-client-credentials) ein Token für die App abzurufen. Dabei wird angenommen, dass die App zuvor ein Geheimnis (Anwendungskennwort, Zertifikat oder Clientassertion) bei Azure AD registriert hat, das dann mit diesem Aufruf geteilt wird.

![Daemon-App](media/scenarios/daemon-app.svg)

## <a name="scenarios-and-supported-authentication-flows"></a>Szenarien und unterstützte Authentifizierungsflows

Szenarien, die das Abrufen von Token beinhalten, lassen sich auch OAuth 2.0-Authentifizierungsflows zuordnen, die im Detail unter [Microsoft Identity Platform-Protokolle](active-directory-v2-protocols.md) beschrieben sind.

|Szenario | Detaillierte Vorgehensweise für das Szenario | OAuth 2.0-Flow/Gewährung | Zielgruppe |
|--|--|--|--|
| [![Single-Page-App](media/scenarios/spa-app.svg)](scenario-spa-overview.md) | [Single-Page-App](scenario-spa-overview.md) | [Implizit](v2-oauth2-implicit-grant-flow.md) | Geschäfts-, Schul- oder Unikonten und persönliche Konten, B2C
| [![Web-App, die Benutzer anmeldet](media/scenarios/scenario-webapp-signs-in-users.svg)](scenario-web-app-sign-user-overview.md) | [Web-App, die Benutzer anmeldet](scenario-web-app-sign-user-overview.md) | [Autorisierungscode](v2-oauth2-auth-code-flow.md) | Geschäfts-, Schul- oder Unikonten und persönliche Konten, B2C |
| [![Web-App, die Web-APIs aufruft](media/scenarios/web-app.svg)](scenario-web-app-call-api-overview.md) | [Web-App, die Web-APIs aufruft](scenario-web-app-call-api-overview.md) | [Autorisierungscode](v2-oauth2-auth-code-flow.md) | Geschäfts-, Schul- oder Unikonten und persönliche Konten, B2C |
| [![Desktop-App, die Web-APIs aufruft](media/scenarios/desktop-app.svg)](scenario-desktop-overview.md) | [Desktop-App, die Web-APIs aufruft](scenario-desktop-overview.md)| Interaktiv ([Autorisierungscode](v2-oauth2-auth-code-flow.md) mit PKCE) | Geschäfts-, Schul- oder Unikonten und persönliche Konten, B2C |
| | | In Windows integriert | Geschäfts-, Schul- oder Unikonten |
| | | [Kennwort des Ressourcenbesitzers](v2-oauth-ropc.md)  | Geschäfts-, Schul- oder Unikonten, B2C |
| ![Gerätecodefluss](media/scenarios/device-code-flow-app.svg)| [Desktop-App, die Web-APIs aufruft](scenario-desktop-overview.md) | [Gerätecode](v2-oauth2-device-code.md)  | Geschäfts-, Schul- oder Unikonten* |
| [![Mobile App, die Web-APIs aufruft](media/scenarios/mobile-app.svg)](scenario-mobile-overview.md) | [Mobile App, die Web-APIs aufruft](scenario-mobile-overview.md) | Interaktiv ([Autorisierungscode](v2-oauth2-auth-code-flow.md) mit PKCE)  |   Geschäfts-, Schul- oder Unikonten und persönliche Konten, B2C
| | | Kennwort des Ressourcenbesitzers  | Geschäfts-, Schul- oder Unikonten, B2C |
| [![Daemon-App](media/scenarios/daemon-app.svg)](scenario-daemon-overview.md) | [Daemon-App](scenario-daemon-overview.md) | [Clientanmeldeinformationen](v2-oauth2-client-creds-grant-flow.md)  |   Nur App-Berechtigungen (kein Benutzer), nur in AAD-Organisationen
| [![Web-API, die Web-APIs aufruft](media/scenarios/web-api.svg)](scenario-web-api-call-api-overview.md) | [Web-API, die Web-APIs aufruft](scenario-web-api-call-api-overview.md)| [Im Namen von](v2-oauth2-on-behalf-of-flow.md) | Geschäfts-, Schul- oder Unikonten und persönliche Konten |

## <a name="scenarios-and-supported-platforms-and-languages"></a>Szenarien und unterstützte Plattformen und Sprachen

Nicht jeder Anwendungstyp ist auf jeder Plattform verfügbar. Sie können zum Erstellen Ihrer Anwendungen auch verschiedene Sprachen verwenden. Microsoft-Authentifizierungsbibliotheken unterstützen eine Reihe von **Plattformen** (JavaScript, .NET Framework, .NET Core, Windows 10/UWP, Xamarin.iOS, Xamarin.Android, iOS nativ, Android nativ, Java, Python).

|Szenario  | Windows | Linux | Mac | iOS | Android
|--|--|--|--|--|--|--|
| [Single-Page-App](scenario-spa-overview.md) <br/>[![Single-Page-App](media/scenarios/spa-app.svg)](scenario-spa-overview.md) | ![MSAL.js](media/sample-v2-code/logo_js.png) MSAL.js | ![MSAL.js](media/sample-v2-code/logo_js.png) MSAL.js | ![MSAL.js](media/sample-v2-code/logo_js.png) MSAL.js | ![MSAL.js](media/sample-v2-code/logo_js.png) MSAL.js | ![MSAL.js](media/sample-v2-code/logo_js.png) MSAL.js
| [Web-App, die Benutzer anmeldet](scenario-web-app-sign-user-overview.md) <br/>[![Web-App, die Benutzer anmeldet](media/scenarios/scenario-webapp-signs-in-users.svg)](scenario-web-app-sign-user-overview.md) | ![ASP.NET](media/sample-v2-code/logo_NET.png)</br> ASP.NET ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core | ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core | ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core
| [Web-App, die Web-APIs aufruft](scenario-web-app-call-api-overview.md) <br/> [![Web-App, die Web-APIs aufruft](media/scenarios/web-app.svg)](scenario-web-app-call-api-overview.md) | ![ASP.NET](media/sample-v2-code/logo_NET.png) </br> ASP.NET + MSAL.NET </br> ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) Flask + MSAL Python| ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) Flask + MSAL Python| ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) Flask + MSAL Python
| [Desktop-App, die Web-APIs aufruft](scenario-desktop-overview.md) <br/> [![Desktop-App, die Web-APIs aufruft](media/scenarios/desktop-app.svg)](scenario-desktop-overview.md) ![Gerätecodeflow](media/scenarios/device-code-flow-app.svg) | ![MSAL.NET](media/sample-v2-code/logo_NET.png)  MSAL.NET ![.NET Core](media/sample-v2-code/logo_NETcore.png) MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python| ![.NET Core](media/sample-v2-code/logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python| ![.NET Core](media/sample-v2-code/logo_NETcore.png) MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python
| [Mobile App, die Web-APIs aufruft](scenario-mobile-overview.md) <br/> [![Mobile App, die Web-APIs aufruft](media/scenarios/mobile-app.svg)](scenario-mobile-overview.md) | ![UWP](media/sample-v2-code/logo_windows.png) MSAL.NET ![Xamarin](media/sample-v2-code/logo_xamarin.png) MSAL.NET | | | ![iOS/Objective-C/Swift](media/sample-v2-code/logo_iOS.png) MSAL.iOS | ![Android](media/sample-v2-code/logo_Android.png) MSAL.Android
| [Daemon-App](scenario-daemon-overview.md) <br/> [![Daemon-App](media/scenarios/daemon-app.svg)](scenario-daemon-overview.md) | ![.NET](media/sample-v2-code/logo_NET.png) MSAL.NET ![.NET Core](media/sample-v2-code/logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python| ![.NET Core](media/sample-v2-code/logo_NETcore.png) MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python| ![.NET Core](media/sample-v2-code/logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python
| [Web-API, die Web-APIs aufruft](scenario-web-api-call-api-overview.md) <br/> [![Web-API, die Web-APIs aufruft](media/scenarios/web-api.svg)](scenario-web-api-call-api-overview.md) | ![.NET](media/sample-v2-code/logo_NET.png) <br/> ASP.NET + MSAL.NET ![.NET Core](media/sample-v2-code/logo_NETcore.png) <br/> ASP.NET Core + MSAL.NET| ![.NET Core](media/sample-v2-code/logo_NETcore.png) <br/> ASP.NET Core + MSAL.NET| ![.NET Core](media/sample-v2-code/logo_NETcore.png)<br/> ASP.NET Core + MSAL.NET

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [Authentifizierungsgrundlagen](authentication-scenarios.md) und [Zugriffstoken](access-tokens.md).