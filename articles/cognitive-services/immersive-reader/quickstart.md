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
ms.openlocfilehash: 3b408de6b60e7e7704ee228b52c399e5b80e3a9e
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718414"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-c"></a>Schnellstart: Erstellen einer Web-App zum Starten des plastischen Readers (C#)

Der [plastische Reader](https://www.onenote.com/learningtools) ist ein inklusiv konzipiertes Tool, das bewährte Techniken zur Verbesserung des Leseverständnisses implementiert.

In diesem Schnellstart erstellen Sie eine Web-App von Grund auf neu und integrieren den plastischen Reader anhand des SDK für den plastischen Reader. Eine vollständiges Arbeitsbeispiel für diesen Schnellstart finden Sie [hier](https://github.com/microsoft/immersive-reader-sdk/tree/master/samples/quickstart-csharp).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads)
* Einen Abonnementschlüssel für den plastischen Reader. Anhand [dieser Anweisungen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) können Sie einen abrufen.

## <a name="create-a-web-app-project"></a>Erstellen eines Web-App-Projekts

Erstellen Sie mit der Vorlage für ASP.NET Core-Webanwendungen mit dem integrierten Model View Controller ein neues Projekt in Visual Studio.

![Neues Projekt](./media/vswebapp.png)

![Neue ASP.NET Core-Webanwendung](./media/vsmvc.png)

## <a name="acquire-an-access-token"></a>Abrufen eines Zugriffstokens

Für diesen nächsten Schritt benötigen Sie Ihren Abonnementschlüssel und einen Endpunkt. Sie finden Ihren Abonnementschlüssel auf der Seite „Schlüssel“ Ihres plastischen Readers im Azure-Portal. Ihren Endpunkt finden Sie auf der Seite „Übersicht“.

Klicken Sie im _Projektmappen-Explorer_ mit der rechten Maustaste auf das Projekt, und wählen Sie anschließend **Geheimen Benutzerschlüssel verwalten** aus. Damit wird eine Datei namens _secrets.json_ geöffnet. Ersetzen Sie den Inhalt dieser Datei durch Folgendes, und geben Sie gegebenenfalls Ihren Abonnementschlüssel und Endpunkt an.

```json
{
  "SubscriptionKey": YOUR_SUBSCRIPTION_KEY,
  "Endpoint": YOUR_ENDPOINT
}
```

Öffnen Sie _Controllers\HomeController.cs_, und ersetzen Sie die `HomeController`-Klasse durch folgenden Code.

```csharp
public class HomeController : Controller
{
    private readonly string SubscriptionKey;
    private readonly string Endpoint;

    public HomeController(Microsoft.Extensions.Configuration.IConfiguration configuration)
    {
        SubscriptionKey = configuration["SubscriptionKey"];
        Endpoint = configuration["Endpoint"];

        if (string.IsNullOrEmpty(Endpoint) || string.IsNullOrEmpty(SubscriptionKey))
        {
            throw new ArgumentNullException("Endpoint or subscriptionKey is null!");
        }
    }

    public IActionResult Index()
    {
        return View();
    }

    [Route("token")]
    public async Task<string> Token()
    {
        return await GetTokenAsync();
    }

    /// <summary>
    /// Exchange your Azure subscription key for an access token
    /// </summary>
    private async Task<string> GetTokenAsync()
    {
        using (var client = new System.Net.Http.HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", SubscriptionKey);
            using (var response = await client.PostAsync(Endpoint, null))
            {
                return await response.Content.ReadAsStringAsync();
            }
        }
    }
}
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
<script type='text/javascript' src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.1.js'></script>
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

    async function launchImmersiveReader() {
        const content = {
            title: document.getElementById('title').innerText,
            chunks: [ {
                content: document.getElementById('content').innerText,
                lang: 'en'
            } ]
        };

        const token = await getImmersiveReaderTokenAsync();
        ImmersiveReader.launchAsync(token, content, { uiZIndex: 1000000 });
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
