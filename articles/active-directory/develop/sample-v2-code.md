---
title: Codebeispiele für die Microsoft Identity Platform | Microsoft-Dokumentation
description: Dieser Artikel enthält einen Index der verfügbaren Microsoft Identity Platform-Codebeispiele (v2.0-Endpunkt), geordnet nach Szenario.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01ed7383515a725816679acb405b2ff3b1dc8e92
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72817229"
---
# <a name="microsoft-identity-platform-code-samples-v20-endpoint"></a>Microsoft Identity Platform – Codebesipiele (V2.0-Endpunkt)

Sie können mit der Microsoft Identity Platform folgende Aktionen ausführen:

- Hinzufügen von Authentifizierungs- und Autorisierungsfeatures zu Ihren Webanwendungen und Web-APIs
- Anfordern eines Zugriffstokens für den Zugriff auf eine geschützte Web-API

In diesem Artikel wird der Microsoft Identity Platform-Endpunkt kurz erläutert, und es werden Links zu Beispielen für den Endpunkt angegeben. Anhand dieser Beispiele wird die Vorgehensweise erläutert, und es werden Codeausschnitte bereitgestellt, die Sie in Ihren Anwendungen verwenden können. Auf der Seite mit den Codebeispielen finden Sie ausführliche Infothemen mit hilfreichen Angaben zu Anforderungen, Installation und Einrichtung. Außerdem enthält der Code Kommentare, die die wichtigsten Abschnitte besser verständlich machen.

> [!NOTE]
> Wenn Sie an Beispielen zu v1.0 interessiert sind, sehen Sie unter [Azure AD-Codebeispiele (v1.0-Endpunkt)](sample-v1-code.md) nach.

Informationen zum grundlegenden Szenario für die einzelnen Beispieltypen finden Sie unter [App-Typen für den Microsoft Identity Platform-Endpunkt](v2-app-types.md).

Sie können auch Beiträge zu den Beispielen auf GitHub schreiben. Wie das geht, erfahren Sie unter [Microsoft Azure Active Directory – Beispiele und Dokumentation](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications"></a>Single-Page-Webanwendungen

Diese Beispiele zeigen, wie Sie eine Single-Page-Webanwendung schreiben, die mit Microsoft Identity Platform geschützt ist. Diese Beispiele verwenden eine der Arten von MSAL.js.

| Plattform | BESCHREIBUNG | Link |
| -------- | --------------------- | -------- |
| ![Diese Abbildung zeigt das JavaScript-Logo.](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Aufrufen von Microsoft Graph |[javascript-graphapi-web-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| ![Diese Abbildung zeigt das JavaScript-Logo.](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Aufrufen von B2C |[b2c-javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) |
| ![Diese Abbildung zeigt das JavaScript-Logo.](media/sample-v2-code/logo_js.png) [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Aufrufen der eigenen Web-API |[javascript-singlepageapp-dotnet-webapi-v2](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |
| ![Diese Abbildung zeigt das AngularJS-Logo.](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL AngularJS)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs)| Aufrufen von Microsoft Graph  | [MsalAngularjsDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/samples/MsalAngularjsDemoApp)
| ![Diese Abbildung zeigt das Angular-Logo.](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL Angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)| Aufrufen von Microsoft Graph  | [MSALAngularDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/samples/MSALAngularDemoApp) |

## <a name="web-applications"></a>Webanwendungen

Die folgenden Beispiele zeigen Webanwendungen, die Benutzer anmelden. Einige Beispiele zeigen außerdem, wie die Anwendung Microsoft Graph oder Ihre eigene Web-API mit der Identität des Benutzers aufruft.

| Plattform | Nur Benutzeranmeldung | Benutzeranmeldung und Aufruf von Microsoft Graph |
| -------- | ------------------- | --------------------------------- |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.2 | [Benutzertutorial für die ASP.NET Core-Web-App-Anmeldung](https://aka.ms/aspnetcore-webapp-sign-in) | Das gleiche Beispiel im Schritt [ASP.NET Core-Web-App-Aufrufe an Microsoft Graph](https://aka.ms/aspnetcore-webapp-call-msgraph) |
| ![Diese Abbildung zeigt das ASP.NET-Logo.](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET | [Schnellstart für ASP.NET](https://github.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) </p> [dotnet-webapp-openidconnect-v2](https://github.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |  [dotnet-admin-restricted-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) </p> |[msgraph-training-aspnetmvcapp](https://github.com/microsoftgraph/msgraph-training-aspnetmvcapp)
| ![Diese Abbildung zeigt das Java-Logo.](media/sample-v2-code/logo_java.png)  |                   | [ms-identity-java-webapp](https://github.com/Azure-Samples/ms-identity-java-webapp): eine MSAL4J-Web-App, die Microsoft Graph aufruft |
| ![Diese Abbildung zeigt das Node.js-Logo.](media/sample-v2-code/logo_nodejs.png)  |                   | [Schnellstart für Node.js](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs) |
| ![Diese Abbildung zeigt das Ruby-Logo.](media/sample-v2-code/logo_ruby.png) |                   | [msgraph-training-rubyrailsapp](https://github.com/microsoftgraph/msgraph-training-rubyrailsapp) |

## <a name="desktop-and-mobile-public-client-apps"></a>Öffentliche Desktop- und mobile Client-Apps

In den folgenden Beispielen werden öffentliche Clientanwendungen (Desktop-/mobile Anwendungen) veranschaulicht, die im Namen eines Benutzers auf die Microsoft Graph-API oder Ihre eigene Web-API zugreifen. Alle diese Clientanwendungen verwenden die Microsoft Authentication Library (MSAL).

| Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph | Aufrufen einer ASP.NET Core 2.0-Web-API |
| ------------------ | -------- |  ----------| ---------- | ------------------------- |
| Desktop (WPF)      | ![Diese Abbildung zeigt das .NET/C#-Logo.](media/sample-v2-code/logo_NET.png) | [Interaktiv](msal-authentication-flows.md#interactive)| [dotnet-desktop-msgraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi) |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das .NET/C#-Logo (Desktop).](media/sample-v2-code/logo_NET.png) | [Integrierte Windows-Authentifizierung](msal-authentication-flows.md#integrated-windows-authentication) | [dotnet-iwa-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2) |  |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das .NET/C#-Logo (Desktop).](media/sample-v2-code/logo_NETcore.png) | [Benutzername/Kennwort](msal-authentication-flows.md#usernamepassword) |[dotnetcore-up-v2](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2) |  |
| Mobil (Android, iOS, UWP)   | ![Diese Abbildung zeigt das .NET/C#-Logo (Xamarin).](media/sample-v2-code/logo_xamarin.png) | [Interaktiv](msal-authentication-flows.md#interactive) |[xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) |  |
| Mobil (iOS)       | ![Diese Abbildung zeigt das iOS-/Objective-C- oder Swift-Logo.](media/sample-v2-code/logo_iOS.png) | [Interaktiv](msal-authentication-flows.md#interactive) |[ios-swift-objc-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) </p> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |  |
| Desktop (macOS)       | macOS | [Interaktiv](msal-authentication-flows.md#interactive) |[macOS-swift-objc-native-v2](https://github.com/Azure-Samples/ms-identity-macOS-swift-objc) |  |
| Mobil (Android-Java)   | ![Diese Abbildung zeigt das Android-Logo.](media/sample-v2-code/logo_Android.png) | [Interaktiv](msal-authentication-flows.md#interactive) |  [android-Java](https://github.com/Azure-Samples/ms-identity-android-java) |  |
| Mobil (Android-Kotlin)   | ![Diese Abbildung zeigt das Android-Logo.](media/sample-v2-code/logo_Android.png) | [Interaktiv](msal-authentication-flows.md#interactive) |  [android-Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin) |  |

## <a name="daemon-applications"></a>Daemonanwendungen

Die folgenden Beispiele zeigen eine Anwendung, die auf die Microsoft Graph-API mit ihrer eigenen Identität (ohne Benutzer) zugreift.

| Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph |
| ------------------ | -------- | ---------- | -------------------- |
| Konsole | ![Diese Abbildung zeigt das .NET Core-Logo.](media/sample-v2-code/logo_NETcore.png)</p> ASP.NET  | [Clientanmeldeinformationen](msal-authentication-flows.md#client-credentials) | [dotnetcore-daemon-v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2) |
| Web-App | ![Diese Abbildung zeigt das ASP.NET-Logo.](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET  | [Clientanmeldeinformationen](msal-authentication-flows.md#client-credentials) | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) |

## <a name="headless-applications"></a>Monitorlose Anwendungen

In den folgenden Beispielen wird eine öffentliche Clientanwendung veranschaulicht, die auf einem Gerät ohne Webbrowser ausgeführt wird. Diese App kann ein Befehlszeilentool sein oder unter Linux oder Mac ausgeführt werden, oder es kann sich um eine IoT-Anwendung handeln. In den Beispielen wird eine App verwendet, die im Namen eines Benutzers, der sich interaktiv an einem anderen Gerät anmeldet (z. B. einem Mobiltelefon), auf die Microsoft Graph-API zugreift. Diese Clientanwendung verwendet die Microsoft Authentication Library (MSAL).

| Clientanwendung | Plattform | Ablauf/Erteilung | Aufrufen von Microsoft Graph |
| ------------------ | -------- |  ----------| ---------- |
| Desktop (Konsole)   | ![Diese Abbildung zeigt das .NET/C#-Logo (Desktop).](media/sample-v2-code/logo_NETcore.png) | [Gerätecodeflow](msal-authentication-flows.md#device-code) |[dotnetcore-devicecodeflow-v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2) |

## <a name="web-apis"></a>Web-APIs

Die folgenden Beispiele veranschaulichen, wie Sie eine Web-API mit dem Microsoft Identity-Plattform-Endpunkt schützen und wie Sie eine nachgeschaltete API von der Web-API aus aufrufen.

| Plattform | Beispiel |
| -------- | ------------------- |
| ![Diese Abbildung zeigt das ASP.NET Core-Logo.](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.2 | ASP.NET Core-Web-API (Dienst) von [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi-calls-msgraph)  |
| ![Diese Abbildung zeigt das ASP.NET-Logo.](media/sample-v2-code/logo_NET.png)</p>ASP.NET MVC | Web-API (Dienst) von [ms-identity-aspnet-webapi-onbehalfof](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof) |

## <a name="other-microsoft-graph-samples"></a>Weitere Microsoft Graph-Beispiele

[Beispiele](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) und Tutorials, die verschiedene Verwendungsmuster für die Microsoft Graph-API (einschließlich Authentifizierung mit Azure AD) veranschaulichen, finden Sie unter [Beispiele und Tutorials der Microsoft Graph-Community](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Weitere Informationen

- [Entwicklerhandbuch zu Azure Active Directory (v1.0)](v1-overview.md)
- [Azure AD-Graph-API – Konzepte und Referenz](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Bibliothek für das Azure AD Graph-API-Hilfsprogramm](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
