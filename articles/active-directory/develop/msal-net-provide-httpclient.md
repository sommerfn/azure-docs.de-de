---
title: Bereitstellen von HttpClient und Proxy (MSAL.NET)
titleSuffix: Microsoft identity platform
description: Weitere Informationen zur Bereitstellung eines eigenen HttpClient und Proxys für die Verbindung mit Azure AD über die Microsoft Authentication Library für .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 259c796cb3653ebdd330f5e65db76cc29c181467
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72802762"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>Bereitstellen eines eigenen HttpClient und Proxys über MSAL.NET
Bei der [Initialisierung einer öffentlichen Clientanwendung](msal-net-initializing-client-applications.md) können Sie mit `.WithHttpClientFactory method` einen eigenen HttpClient bereitstellen.  Die Bereitstellung eines eigenen HttpClient ermöglicht fortgeschrittene Szenarien wie die differenzierte Steuerung eines HTTP-Proxys, das Anpassen von Benutzer-Agent-Headern oder das Erzwingen von MSAL, einen bestimmten HttpClient zu verwenden (z. B. in ASP.NET Core-Web-Apps/-APIs).

## <a name="initialize-with-httpclientfactory"></a>Initialisieren mit HttpClientFactory
Das folgende Beispiel zeigt, wie Sie ein `HttpClientFactory` erstellen und dann damit eine öffentliche Clientanwendung initialisieren können:

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>HttpClient und Xamarin iOS
Bei der Verwendung von Xamarin iOS wird empfohlen, ein `HttpClient` zu erstellen, das explizit den `NSURLSession`-basierten Handler für iOS 7 und höher verwendet. MSAL.NET erstellt automatisch ein `HttpClient`, das `NSURLSessionHandler` für iOS 7 und höher verwendet. Weitere Informationen finden Sie in der [Xamarin iOS-Dokumentation für HttpClient](/xamarin/cross-platform/macios/http-stack).