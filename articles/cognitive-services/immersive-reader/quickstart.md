---
title: 'Schnellstart: Erstellen einer Web-App zum Starten des plastischen Readers mit C#'
titlesuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erstellen Sie eine Web-App von Grund auf neu und fügen die Funktion der API für den plastischen Reader hinzu.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 6386c22044483a0ac4a324397cf2f9d22e83b579
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442860"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-c"></a>Schnellstart: Erstellen einer Web-App zum Starten des plastischen Readers (C#)

Der [plastische Reader](https://www.onenote.com/learningtools) ist ein inklusiv konzipiertes Tool, das bewährte Techniken zur Verbesserung des Leseverständnisses implementiert.

In diesem Schnellstart erstellen Sie eine Web-App von Grund auf neu und integrieren den plastischen Reader anhand des SDK für den plastischen Reader. Eine vollständiges Arbeitsbeispiel für diesen Schnellstart finden Sie [hier](https://github.com/microsoft/immersive-reader-sdk/tree/master/samples/quickstart-csharp).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads)
* Eine Ressource des plastischen Readers, die für die Authentifizierung mit Azure Active Directory (Azure AD) konfiguriert ist. Folgen Sie [diesen Anweisungen](./azure-active-directory-authentication.md) für die Einrichtung. Einige der hier erstellten Werte benötigen Sie bei der Konfiguration der Beispielprojekteigenschaften. Speichern Sie die Ausgabe Ihrer Sitzung zur späteren Verwendung in einer Textdatei.

## <a name="create-a-web-app-project"></a>Erstellen eines Web-App-Projekts

Erstellen Sie mit der Vorlage für ASP.NET Core-Webanwendungen mit dem integrierten Model View Controller ein neues Projekt in Visual Studio.

![Neues Projekt](./media/vswebapp.png)

![Neue ASP.NET Core-Webanwendung](./media/vsmvc.png)

## <a name="acquire-an-azure-ad-authentication-token"></a>Abrufen eines Azure AD-Authentifizierungstokens

Für diesen Teil benötigen Sie einige Werte aus dem obigen Schritt zur Konfiguration der Azure AD-Authentifizierung. Greifen Sie auf die Textdatei zurück, die Sie in dieser Sitzung gespeichert haben.

````text
TenantId     => Azure subscription TenantId
ClientId     => Azure AD ApplicationId
ClientSecret => Azure AD Application Service Principal password
Subdomain    => Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')
````

Klicken Sie im _Projektmappen-Explorer_ mit der rechten Maustaste auf das Projekt, und wählen Sie anschließend **Geheimen Benutzerschlüssel verwalten** aus. Damit wird eine Datei namens _secrets.json_ geöffnet. Ersetzen Sie den Inhalt dieser Datei durch Folgendes, und geben Sie Ihre benutzerdefinierten Eigenschaftswerte von oben an.

```json
{
  "TenantId": YOUR_TENANT_ID,
  "ClientId": YOUR_CLIENT_ID,
  "ClientSecret": YOUR_CLIENT_SECRET,
  "Subdomain": YOUR_SUBDOMAIN
}
```

Öffnen Sie _Controllers\HomeController.cs_, und ersetzen Sie die Datei durch den folgenden Code.

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.IdentityModel.Clients.ActiveDirectory;

namespace QuickstartSampleWebApp.Controllers
{
    public class HomeController : Controller
    {
        private readonly string TenantId;     // Azure subscription TenantId
        private readonly string ClientId;     // Azure AD ApplicationId
        private readonly string ClientSecret; // Azure AD Application Service Principal password
        private readonly string Subdomain;    // Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')

        public HomeController(Microsoft.Extensions.Configuration.IConfiguration configuration)
        {
            TenantId = configuration["TenantId"];
            ClientId = configuration["ClientId"];
            ClientSecret = configuration["ClientSecret"];
            Subdomain = configuration["Subdomain"];

            if (string.IsNullOrWhiteSpace(TenantId))
            {
                throw new ArgumentNullException("TenantId is null! Did you add that info to secrets.json?");
            }

            if (string.IsNullOrWhiteSpace(ClientId))
            {
                throw new ArgumentNullException("ClientId is null! Did you add that info to secrets.json?");
            }

            if (string.IsNullOrWhiteSpace(ClientSecret))
            {
                throw new ArgumentNullException("ClientSecret is null! Did you add that info to secrets.json?");
            }

            if (string.IsNullOrWhiteSpace(Subdomain))
            {
                throw new ArgumentNullException("Subdomain is null! Did you add that info to secrets.json?");
            }
        }

        public IActionResult Index()
        {
            return View();
        }

        [Route("subdomain")]
        public string GetSubdomain()
        {
            return Subdomain;
        }

        [Route("token")]
        public async Task<string> GetToken()
        {
            return await GetTokenAsync();
        }

        /// <summary>
        /// Get an Azure AD authentication token
        /// </summary>
        private async Task<string> GetTokenAsync()
        {
            string authority = $"https://login.windows.net/{TenantId}";
            const string resource = "https://cognitiveservices.azure.com/";

            AuthenticationContext authContext = new AuthenticationContext(authority);
            ClientCredential clientCredential = new ClientCredential(ClientId, ClientSecret);

            AuthenticationResult authResult = await authContext.AcquireTokenAsync(resource, clientCredential);

            return authResult.AccessToken;
        }
    }
}
```

## <a name="add-the-microsoftidentitymodelclientsactivedirectory-nuget-package"></a>Hinzufügen des NuGet-Pakets Microsoft.IdentityModel.Clients.ActiveDirectory

Der obige Code verwendet Objekte aus dem NuGet-Paket **Microsoft.IdentityModel.Clients.ActiveDirectory**, sodass Sie eine Referenz auf dieses Paket in Ihrem Projekt hinzufügen müssen.

Öffnen Sie die NuGet-Paket-Manager-Konsole über **Tools -> NuGet-Paket-Manager -> Paket-Manager-Konsole**, und geben Sie Folgendes ein:

```powershell
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 5.1.0
```

## <a name="add-sample-content"></a>Hinzufügen von Beispielinhalten

Nun fügen wir einige Beispielinhalte zu dieser Web-App hinzu. Öffnen Sie _Views\Home\Index.cshtml_, und ersetzen Sie den automatisch generierten Code durch dieses Beispiel:

```html
<h1 id='title'>Geography</h1>
<span id='content'>
    <p>The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers. Landforms are sometimes called physical features. It is important for students to know about the physical geography of Earth. The seasons, the atmosphere and all the natural processes of Earth affect where people are able to live. Geography is one of a combination of factors that people use to decide where they want to live.</p>
</span>

<div class='immersive-reader-button' data-button-style='iconAndText' onclick='launchImmersiveReader()'></div>

@section scripts {
    <script type='text/javascript' src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.2.js'></script>
    <script type='text/javascript' src='https://code.jquery.com/jquery-3.3.1.min.js'></script>
    <script type='text/javascript'>
    function getImmersiveReaderTokenAsync() {
        return new Promise((resolve) => {
            $.ajax({
                url: '/token',
                type: 'GET',
                success: token => {
                    resolve(token);
                }
            });
        });
    }

    function getSubdomainAsync() {
        return new Promise((resolve) => {
            $.ajax({
                url: '/subdomain',
                type: 'GET',
                success: subdomain => {
                    resolve(subdomain);
                }
            });
        });
    }

    async function launchImmersiveReader() {
        const content = {
            title: document.getElementById('title').innerText,
            chunks: [ {
                content: document.getElementById('content').innerText,
                lang: 'en'
            } ]
        };

        const token = await getImmersiveReaderTokenAsync();
        var subdomain = await getSubdomainAsync();

        ImmersiveReader.launchAsync(token, subdomain, content, { uiZIndex: 1000000 });
    }
    </script>
}
```

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

Wählen Sie auf der Menüleiste **Debuggen > Debuggen starten** aus, oder drücken Sie **F5**, um die Anwendung zu starten.

In Ihrem Browser sollte Folgendes angezeigt werden:

![Beispiel-App](./media/quickstart-result.png)

Wenn Sie auf die Schaltfläche „Plastischer Reader“ klicken, sehen Sie, dass der plastische Reader mit dem Inhalt auf der Seite gestartet wurde.

![Plastischer Reader](./media/quickstart-immersive-reader.png)

## <a name="next-steps"></a>Nächste Schritte

* Schauen Sie sich das [Tutorial](./tutorial.md) an, um zu sehen, welche weiteren Möglichkeiten Sie mit dem SDK für den plastischen Reader haben.
* Schauen Sie sich das [SDK für den plastischen Reader](https://github.com/Microsoft/immersive-reader-sdk) und die [Referenz für das SDK für den plastischen Reader](./reference.md) an.
