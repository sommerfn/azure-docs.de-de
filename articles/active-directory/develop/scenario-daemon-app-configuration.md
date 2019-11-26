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
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 32fbd4af78e02dad2a8a74ee21f9cb8c6ef0a976
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175494"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Daemon-App, die Web-APIs aufruft – Codekonfiguration

Erfahren Sie, wie Sie den Code Ihrer Daemonanwendung konfigurieren können, die Web-APIs aufruft.

## <a name="msal-libraries-supporting-daemon-apps"></a>MSAL-Bibliotheken, die Daemon-Apps unterstützen

Folgende Microsoft-Bibliotheken unterstützen Daemon-Apps:

  MSAL-Bibliothek | BESCHREIBUNG
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Die folgenden Plattformen werden zum Erstellen von Daemonanwendungen unterstützt: .NET Framework und .NET Core (nicht unterstützt: UWP, Xamarin.iOS und Xamarin.Android, da mit diesen Plattformen öffentliche Clientanwendungen erstellt werden).
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL Python | In Entwicklung – Public Preview
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL Java | In Entwicklung – Public Preview

## <a name="configuration-of-the-authority"></a>Autoritätskonfiguration

Da Ihre Daemonanwendungen keine delegierten Berechtigungen, sondern Anwendungsberechtigungen verwenden, darf ihr *unterstützter Kontotyp* nicht *Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten (z. B. Skype, Xbox, Outlook.com)* lauten. Es gibt keinen Mandantenadministrator, der der Daemonanwendung für persönliche Microsoft-Konten Einwilligung erteilen könnte. Sie müssen *accounts in my organization* (Konten in meiner Organisation) oder *accounts in any organization* (Konten in allen Organisationen) auswählen.

Deshalb sollte die in der Anwendungskonfiguration angegebene Autorität über einen Mandanten verfügen (eine Mandanten-ID oder ein Domänennamen, die zu Ihrer Organisation gehören).

Wenn Sie ein unabhängiger Softwarehersteller sind und ein mehrinstanzenfähiges Tool bereitstellen möchten, können Sie `organizations` verwenden. Beachten Sie jedoch, dass Sie Ihren Kunden auch erklären müssen, wie die Administratoreinwilligung erteilt wird. Weitere Informationen finden Sie im Abschnitt zum [Anfordern der Einwilligung für einen gesamten Mandanten](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant). Außerdem gibt es derzeit in MSAL eine Einschränkung: `organizations` ist nur zulässig, wenn die Clientanmeldeinformationen ein Anwendungsgeheimnis (kein Zertifikat) sind.

## <a name="application-configuration-and-instantiation"></a>Konfiguration und Instanziierung von Anwendungen

Die Clientanmeldeinformationen in MSAL-Bibliotheken (Geheimnis oder Zertifikat) werden als Parameter der vertraulichen Clientanwendungskonstruktion übergeben.

> [!IMPORTANT]
> Auch wenn Ihre Anwendung eine Konsolenanwendung ist, die als Dienst ausgeführt wird, muss sie eine vertrauliche Clientanwendung sein, wenn sie eine Daemonanwendung ist.

### <a name="configuration-file"></a>Konfigurationsdatei

Die Konfigurationsdatei definiert Folgendes:

- Die Autorität oder Cloudinstanz und Mandanten-ID
- Die Client-ID, die Sie von der Anwendungsregistrierung erhalten haben
- Einen geheimen Clientschlüssel oder ein Zertifikat

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

[appsettings.json](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/master/1-Call-MSGraph/daemon-console/appsettings.json) aus dem Beispiel [.NET Core console daemon](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2).

```JSon
{
  "Instance": "https://login.microsoftonline.com/{0}",
  "Tenant": "[Enter here the tenantID or domain name for your Azure AD tenant]",
  "ClientId": "[Enter here the ClientId for your application]",
  "ClientSecret": "[Enter here a client secret for your application]",
  "CertificateName": "[Or instead of client secret: Enter here the name of a certificate (from the user cert store) as registered with your application]"
}
```

Geben Sie entweder einen geheimen Clientschlüssel oder einen Zertifikatnamen an. Beide Einstellungen sind exklusiv.

# <a name="pythontabpython"></a>[Python](#tab/python)

Beim Erstellen eines vertraulichen Clients mit geheimen Clientschlüsseln sieht die Konfigurationsdatei [parameters.json](https://github.com/Azure-Samples/ms-identity-python-daemon/blob/master/1-Call-MsGraph-WithSecret/parameters.json) im Beispiel [Python Daemon](https://github.com/Azure-Samples/ms-identity-python-daemon) wie folgt aus.

```Json
{
  "authority": "https://login.microsoftonline.com/Enter_the_Tenant_Name_Here",
  "client_id": "your_client_id",
  "scope": [ "https://graph.microsoft.com/.default" ],
  "secret": "The secret generated by AAD during your confidential app registration",
  "endpoint": "https://graph.microsoft.com/v1.0/users"
}
```

Beim Erstellen eines vertraulichen Clients mit Zertifikaten sieht die Konfigurationsdatei [parameters.json](https://github.com/Azure-Samples/ms-identity-python-daemon/blob/master/2-Call-MsGraph-WithCertificate/parameters.json) im Beispiel [Python Daemon](https://github.com/Azure-Samples/ms-identity-python-daemon) wie folgt aus.

```Json
{
  "authority": "https://login.microsoftonline.com/Enter_the_Tenant_Name_Here",
  "client_id": "your_client_id",
  "scope": [ "https://graph.microsoft.com/.default" ],
  "thumbprint": "790E... The thumbprint generated by AAD when you upload your public cert",
  "private_key_file": "server.pem",
  "endpoint": "https://graph.microsoft.com/v1.0/users"
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

Dies ist die Klasse, die in den MSAL Java-Entwicklungsbeispielen zum Konfigurieren der Beispiele verwendet wird: [TestData](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/public-client/TestData.java).

```Java
public class TestData {

    final static String TENANT_SPECIFIC_AUTHORITY = "https://login.microsoftonline.com/<TenantId>/";
    final static String GRAPH_DEFAULT_SCOPE = "https://graph.microsoft.com/.default";
    final static String CONFIDENTIAL_CLIENT_ID = "";
    final static String CONFIDENTIAL_CLIENT_SECRET = "";
}
```

---

### <a name="instantiation-of-the-msal-application"></a>Instanziierung der MSAL-Anwendung

Zum Instanziieren der MSAL-Anwendung müssen Sie folgende Schritte ausführen:

- Fügen Sie das MSAL-Paket (sprachabhängig) hinzu, importieren Sie es, oder verweisen Sie darauf.
- Die Konstruktion unterscheidet sich je nach Verwendung von geheimen Clientschlüsseln oder Zertifikaten (oder als erweitertes Szenario mit signierten Assertionen).

#### <a name="reference-the-package"></a>Verweisen auf das Paket

Verweisen Sie im Anwendungscode auf das MSAL-Paket.

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

Fügen Sie Ihrer Anwendung das NuGet-Paket [Microsoft.IdentityClient](https://www.nuget.org/packages/Microsoft.Identity.Client) hinzu.
In MSAL.NET wird die vertrauliche Clientanwendung durch die `IConfidentialClientApplication`-Schnittstelle dargestellt.
Verwenden Sie den MSAL.NET-Namespace im Quellcode.

```CSharp
using Microsoft.Identity.Client;
IConfidentialClientApplication app;
```

# <a name="pythontabpython"></a>[Python](#tab/python)

```python
import msal
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
import com.microsoft.aad.msal4j.ClientCredentialFactory;
import com.microsoft.aad.msal4j.ClientCredentialParameters;
import com.microsoft.aad.msal4j.ConfidentialClientApplication;
import com.microsoft.aad.msal4j.IAuthenticationResult;
```

---

#### <a name="instantiate-the-confidential-client-application-with-client-secrets"></a>Instanziieren der vertraulichen Clientanwendung mit geheimen Clientschlüsseln

Im Folgenden finden Sie den Code zum Instanziieren der vertraulichen Clientanwendung mit einem geheimen Clientschlüssel:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

```CSharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

# <a name="pythontabpython"></a>[Python](#tab/python)

```Python
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance which maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

# <a name="javatabjava"></a>[Java](#tab/java)

```Java
ConfidentialClientApplication app = ConfidentialClientApplication.builder(
        TestData.CONFIDENTIAL_CLIENT_ID,
        ClientCredentialFactory.create(TestData.CONFIDENTIAL_CLIENT_SECRET))
        .authority(TestData.TENANT_SPECIFIC_AUTHORITY)
        .build();
```

---

#### <a name="instantiate-the-confidential-client-application-with-client-certificate"></a>Instanziieren der vertraulichen Clientanwendung mit Clientzertifikat

Im Folgenden sehen Sie den Code zum Erstellen einer Anwendung mit einem Zertifikat:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

```CSharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

# <a name="pythontabpython"></a>[Python](#tab/python)

```Python
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance which maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential={"thumbprint": config["thumbprint"], "private_key": open(config['private_key_file']).read()},
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

# <a name="javatabjava"></a>[Java](#tab/java)

In MSAL.Java gibt es zwei Generatoren zum Instanziieren der vertraulichen Clientanwendung mit Zertifikaten:

```Java

InputStream pkcs12Certificate = ... ; /* containing PCKS12 formatted certificate*/
string certificatePassword = ... ;    /* contains the password to access the certificate */

ConfidentialClientApplication app = ConfidentialClientApplication.builder(
        TestData.CONFIDENTIAL_CLIENT_ID,
        ClientCredentialFactory.create(pkcs12Certificate, certificatePassword))
        .authority(TestData.TENANT_SPECIFIC_AUTHORITY)
        .build();
```

oder

```Java
PrivateKey key = getPrivateKey(); /* RSA private key to sign the assertion */
X509Certificate publicCertificate = getPublicCertificate(); /* x509 public certificate used as a thumbprint */

ConfidentialClientApplication app = ConfidentialClientApplication.builder(
        TestData.CONFIDENTIAL_CLIENT_ID,
        ClientCredentialFactory.create(rsaPrivateKey, publicKeyCertificate))
        .authority(TestData.TENANT_SPECIFIC_AUTHORITY)
        .build();
```

---

#### <a name="advanced-scenario---instantiate-the-confidential-client-application-with-client-assertions"></a>Erweitertes Szenario: Instanziieren der vertraulichen Clientanwendung mit Clientassertionen

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

Die vertrauliche Clientanwendung kann ihre Identität statt mit einem geheimen Clientschlüssel oder einem Zertifikat auch mithilfe von Clientassertionen nachweisen.

MSAL.NET verfügt über zwei Methoden, um für die vertrauliche Client-App signierte Assertionen bereitzustellen:

- `.WithClientAssertion()`
- `.WithClientClaims()`

Wenn Sie `WithClientAssertion` verwenden, müssen Sie ein signiertes JSON Web Token (JWT) bereitstellen. Dieses erweiterte Szenario wird unter [Clientassertionen](msal-net-client-assertions.md) ausführlich erläutert.

```CSharp
string signedClientAssertion = ComputeAssertion();
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientAssertion(signedClientAssertion)
                                          .Build();
```

Bei Verwendung von `WithClientClaims` berechnet MSAL.NET eine signierte Assertion, die die von Azure AD erwarteten Ansprüche sowie zusätzliche Clientansprüche enthält, die Sie senden möchten.
Im folgenden Codeausschnitt wird die Vorgehensweise gezeigt:

```CSharp
string ipAddress = "192.168.1.2";
var claims = new Dictionary<string, string> { { "client_ip", ipAddress } };
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithAuthority(new Uri(config.Authority))
                                          .WithClientClaims(certificate, claims)
                                          .Build();```
```

Auch hierzu finden Sie ausführliche Informationen unter [Clientassertionen](msal-net-client-assertions.md).

# <a name="pythontabpython"></a>[Python](#tab/python)

In MSAL.Python können Sie Clientansprüche mithilfe der Ansprüche bereitstellen, die mit dem privaten Schlüssel von `ConfidentialClientApplication` signiert werden.

```Python
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance which maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential={"thumbprint": config["thumbprint"], "private_key": open(config['private_key_file']).read()},
    client_claims = {"client_ip": "x.x.x.x"}
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

Einzelheiten finden Sie in der Referenzdokumentation zu MSAL Python für die [Vertrauliche Clientanwendung (ConfidentialClientApplication)](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.__init__).

# <a name="javatabjava"></a>[Java](#tab/java)

MSAL Java befindet sich in der öffentlichen Vorschau. Signierte Assertionen werden noch nicht unterstützt.

---

## <a name="next-steps"></a>Nächste Schritte

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [Daemon app - acquiring tokens for the app (Daemon-App: Abrufen von Token für die App)](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-acquire-token?tabs=dotnet)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Daemon app - acquiring tokens for the app (Daemon-App: Abrufen von Token für die App)](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-acquire-token?tabs=python)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Daemon app - acquiring tokens for the app (Daemon-App: Abrufen von Token für die App)](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-acquire-token?tabs=java)

---
