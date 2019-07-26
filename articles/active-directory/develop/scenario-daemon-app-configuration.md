---
title: Daemon-App, die Web-APIs aufruft (App-Konfiguration) – Microsoft Identity Plattform
description: Erfahren Sie, wie Sie eine Daemon-App erstellen, die Web-APIs aufruft (App-Konfiguration).
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
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd2da6baecdce3ab85a45347f27f573bf814445d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055751"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Daemon-App, die Web-APIs aufruft – Codekonfiguration

Erfahren Sie, wie Sie den Code Ihrer Daemonanwendung konfigurieren können, die Web-APIs aufruft.

## <a name="msal-libraries-supporting-daemon-apps"></a>MSAL-Bibliotheken, die Daemon-Apps unterstützen

Folgende Microsoft-Bibliotheken unterstützen Daemon-Apps:

  MSAL-Bibliothek | BESCHREIBUNG
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Die folgenden Plattformen werden zum Erstellen von Daemonanwendungen unterstützt: .NET Framework und .NET Core (nicht unterstützt: UWP, Xamarin.iOS und Xamarin.Android, da mit diesen Plattformen öffentliche Clientanwendungen erstellt werden).
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | In Entwicklung – Public Preview
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | In Entwicklung – Public Preview

## <a name="configuration-of-the-authority"></a>Autoritätskonfiguration

Da Ihre Daemonanwendungen keine delegierten Berechtigungen sondern Anwendungsberechtigungen verwenden, darf ihr *unterstützter Kontotyp* nicht *Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten (z. B. Skype, Xbox, Outlook.com)* lauten. Es gibt keinen Mandantenadministrator, der der Daemonanwendung für persönliche Microsoft-Konten Einwilligung erteilen könnte. Sie müssen *accounts in my organization* (Konten in meiner Organisation) oder *accounts in any organization* (Konten in allen Organisationen) auswählen.

Deshalb sollte die in der Anwendungskonfiguration angegebene Autorität über einen Mandanten verfügen (eine Mandanten-ID oder ein Domänennamen, die zu Ihrer Organisation gehören).

Wenn Sie ein unabhängiger Softwarehersteller sind und ein Tool mit mehreren Mandanten bereitstellen möchten, können Sie `organizations` verwenden. Beachten Sie jedoch, dass Sie Ihren Kunden erklären müssen, wie diese die Administratoreinwilligung gewähren. Weitere Informationen finden Sie im Abschnitt zum [Anfordern der Einwilligung für einen gesamten Mandanten](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant). Außerdem gibt es derzeit in MSAL eine Einschränkung, dass `organizations` nur zulässig ist, wenn die Clientanmeldeinformationen ein Anwendungsgeheimnis (kein Zertifikat) sind. Weitere Informationen finden Sie unter [MSAL.NET Fehler #891](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/891)

## <a name="application-configuration-and-instantiation"></a>Konfiguration und Instanziierung von Anwendungen

Die Clientanmeldeinformationen in MSAL-Bibliotheken (Geheimnis oder Zertifikat) werden als Parameter der vertraulichen Clientanwendungskonstruktion übergeben.

> [!IMPORTANT]
> Auch wenn Ihre Anwendung eine Konsolenanwendung ist, die als Dienst ausgeführt wird, muss sie eine vertrauliche Clientanwendung sein, wenn sie eine Daemonanwendung ist.

### <a name="msalnet"></a>MSAL.NET

Fügen Sie Ihrer Anwendung das NuGet-Paket [Microsoft.IdentityClient](https://www.nuget.org/packages/Microsoft.Identity.Client) hinzu.

Verwenden Sie den Namespace MSAL.NET.

```CSharp
using Microsoft.Identity.Client;
```

Die Daemonanwendung wird von einem `IConfidentialClientApplication`-Objekt dargestellt.

```CSharp
IConfidentialClientApplication app;
```

Im Folgenden sehen Sie den Code zum Erstellen einer Anwendung mit einem Anwendungsgeheimnis:

```CSharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

Im Folgenden sehen Sie den Code zum Erstellen einer Anwendung mit einem Zertifikat:

```CSharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

### <a name="msalpython"></a>MSAL.Python

```Python
# Create a preferably long-lived app instance which maintains a token cache.

app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache

    )
```

### <a name="msaljava"></a>MSAL.Java

```Java
PrivateKey key = getPrivateKey();
X509Certificate publicCertificate = getPublicCertificate();

// create clientCredential with public and private key
IClientCredential credential = ClientCredentialFactory.create(key, publicCertificate);

ConfidentialClientApplication cca = ConfidentialClientApplication
  .builder(CLIENT_ID, credential)
  .authority(AUTHORITY_MICROSOFT)
  .build();
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Daemon app - acquiring tokens for the app (Daemon-App: Abrufen von Token für die App)](./scenario-daemon-acquire-token.md)