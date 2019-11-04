---
title: 'Schnellstart: Abfragen eines Vorhersageendpunkts mit dem C# SDK: LUIS'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie das C# SDK, um eine Benutzeräußerung an LUIS zu senden und eine Vorhersage zu empfangen.
author: diberry
manager: nitinme
ms.service: cognitive-services
services: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: d7cf46add464a089f15a5bd15a20cea7d17eed37
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488676"
---
# <a name="quickstart-query-v2-prediction-endpoint-with-c-net-sdk"></a>Schnellstart: Abfragen eines V2-Vorhersageendpunkts mit dem C# .NET SDK

Verwenden Sie das auf [NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/) verfügbare .NET SDK, um eine Benutzeräußerung an Language Understanding (LUIS) zu senden und eine Vorhersage der Benutzerabsicht zu empfangen. 

In dieser Schnellstartanleitung wird eine Benutzeräußerung (beispielsweise `turn on the bedroom light`) an eine öffentliche Language Understanding-Anwendung gesendet. Anschließend wird die Vorhersage empfangen, und die Absicht mit der höchsten Punktzahl (`HomeAutomation.TurnOn`) und die in der Äußerung gefundene Entität (`HomeAutomation.Room`) werden angezeigt. 

## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio Community 2017 Edition](https://visualstudio.microsoft.com/vs/community/)
* Programmiersprache C# (in VS Community 2017 enthalten)
* ID der öffentlichen App: df67dcdb-c37d-46af-88e1-8b97951ca1c2

> [!Note]
> Die vollständige Lösung steht im GitHub-Repository [cognitive-services-language-understanding](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/sdk-quickstarts/c%23/UsePredictionRuntime) zur Verfügung.

Weitere Dokumentationen:

 * [SDK-Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet)


## <a name="get-cognitive-services-or-language-understanding-key"></a>Abrufen eines Schlüssels für Cognitive Services oder Language Understanding

Um die öffentliche App für die Gebäudeautomatisierung verwenden zu können, benötigen Sie einen gültigen Schlüssel für Endpunktvorhersagen. Sie können entweder einen (über die Azure-Befehlszeilenschnittstelle erstellten) Cognitive Services-Schlüssel verwenden, der für viele Cognitive Services gültig ist, oder einen `Language Understanding`-Schlüssel. 

Verwenden Sie den folgenden [Azure CLI-Befehl zum Erstellen eines Cognitive Services-Schlüssels](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create):

```azurecli-interactive
az cognitiveservices account create \
    -n my-cog-serv-resource \
    -g my-cog-serv-resource-group \
    --kind CognitiveServices \
    --sku S0 \
    -l WestEurope \ 
    --yes
```

## <a name="create-net-core-project"></a>Erstellen eines .NET Core-Projekts

Erstellen Sie in Visual Studio Community 2017 ein .NET Core-Konsolenprojekt.

1. Öffnen Sie Visual Studio Community 2017.
1. Erstellen Sie ein neues Projekt, indem Sie im Abschnitt **Visual C#** die Option **Konsolen-App (.NET Core)** auswählen.
1. Geben Sie den Projektnamen `QueryPrediction` ein, behalten Sie die übrigen Standardwerte bei, und wählen Sie **OK** aus.
    Dadurch wird ein einfaches Projekt mit einer primären Codedatei namens **Program.cs** erstellt.

## <a name="add-sdk-with-nuget"></a>Hinzufügen des SDK mit NuGet

1. Wählen Sie in der Strukturansicht des **Projektmappen-Explorers** das Projekt mit dem Namen **QueryPrediction** aus, und klicken Sie mit der rechten Maustaste darauf. Wählen Sie im Menü die Option **NuGet-Pakete verwalten...** aus.
1. Wählen Sie **Durchsuchen** aus, und geben Sie `Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime` ein. Wenn die Paketinformationen angezeigt werden, wählen Sie **Installieren** aus, um das Paket im Projekt zu installieren. 
1. Fügen Sie am Anfang von **Program.cs** die folgenden _using_-Anweisungen hinzu. Behalten Sie die bereits vorhandene _using_-Anweisung für `System` bei. 

```csharp
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime;
using Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime.Models;
```

## <a name="create-a-new-method-for-the-prediction"></a>Erstellen einer neuen Methode für die Vorhersage

Erstellen Sie eine neue Methode (`GetPrediction`), um die Abfrage an den Vorhersageendpunkt zu senden. Die Methode erstellt und konfiguriert alle erforderlichen Objekte und gibt anschließend eine Aufgabe (`Task`) mit den Vorhersageergebnissen ([`LuisResult`](https://docs.microsoft.com/en-us/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.luisresult?view=azure-python)) zurück. 

```csharp
static async  Task<LuisResult> GetPrediction() {
}
```

## <a name="create-credentials-object"></a>Erstellen des Anmeldeinformationsobjekts

Fügen Sie der Methode `GetPrediction` den folgenden Code hinzu, um die Clientanmeldeinformationen mit Ihrem Cognitive Services-Schlüssel zu erstellen.

Ersetzen Sie `<REPLACE-WITH-YOUR-KEY>` durch die Region Ihres Cognitive Services-Schlüssels. Der Schlüssel befindet sich im [Azure-Portal](https://portal.azure.com) auf der Schlüsselseite für diese Ressource.

```csharp
// Use Language Understanding or Cognitive Services key
// to create authentication credentials
var endpointPredictionkey = "<REPLACE-WITH-YOUR-KEY>";
var credentials = new ApiKeyServiceClientCredentials(endpointPredictionkey);
```

## <a name="create-language-understanding-client"></a>Erstellen des Language Understanding-Clients

Fügen Sie in der Methode `GetPrediction` nach dem vorherigen Code den folgenden Code hinzu, um die neuen Anmeldeinformationen zu verwenden. Dadurch wird ein [`LUISRuntimeClient`](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient.-ctor?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_LUIS_Runtime_LUISRuntimeClient__ctor_Microsoft_Rest_ServiceClientCredentials_System_Net_Http_DelegatingHandler___)-Clientobjekt erstellt. 

Ersetzen Sie `<REPLACE-WITH-YOUR-KEY-REGION>` durch die Region Ihres Schlüssels (etwa `westus`). Die Schlüsselregion befindet sich im [Azure-Portal](https://portal.azure.com) auf der Übersichtsseite für diese Ressource.

```csharp
// Create Luis client and set endpoint
// region of endpoint must match key's region, for example `westus`
var luisClient = new LUISRuntimeClient(credentials, new System.Net.Http.DelegatingHandler[] { });
luisClient.Endpoint = "https://<REPLACE-WITH-YOUR-KEY-REGION>.api.cognitive.microsoft.com";
```

## <a name="set-query-parameters"></a>Festlegen der Abfrageparameter

Fügen Sie in der Methode `GetPrediction` nach dem vorherigen Code den folgenden Code hinzu, um die Abfrageparameter festzulegen:

```csharp
// public Language Understanding Home Automation app
var appId = "df67dcdb-c37d-46af-88e1-8b97951ca1c2";

// query specific to home automation app
var query = "turn on the bedroom light";

// common settings for remaining parameters
Double? timezoneOffset = null;
var verbose = true;
var staging = false;
var spellCheck = false;
String bingSpellCheckKey = null;
var log = false;
```

## <a name="query-prediction-endpoint"></a>Abfragen eines Vorhersageendpunkts

Fügen Sie in der Methode `GetPrediction` nach dem vorherigen Code den folgenden Code hinzu, um die Abfrageparameter festzulegen:

```csharp
// Create prediction client
var prediction = new Prediction(luisClient);

// get prediction
return await prediction.ResolveAsync(appId, query, timezoneOffset, verbose, staging, spellCheck, bingSpellCheckKey, log, CancellationToken.None);
```

## <a name="display-prediction-results"></a>Anzeigen der Vorhersageergebnisse

Ändern Sie die Methode **Main**, um die neue Methode `GetPrediction` aufzurufen und die Vorhersageergebnisse zurückzugeben:

```csharp
static void Main(string[] args)
{

    var luisResult = GetPrediction().Result;

    // Display query
    Console.WriteLine("Query:'{0}'", luisResult.Query);

    // Display most common properties of query result
    Console.WriteLine("Top intent is '{0}' with score {1}", luisResult.TopScoringIntent.Intent,luisResult.TopScoringIntent.Score);

    // Display all entities detected in query utterance
    foreach (var entity in luisResult.Entities)
    {
        Console.WriteLine("{0}:'{1}' begins at position {2} and ends at position {3}", entity.Type, entity.Entity, entity.StartIndex, entity.EndIndex);
    }

    Console.Write("done");

}
```

## <a name="run-the-project"></a>Ausführen des Projekts

Erstellen Sie das Projekt in Studio, und führen Sie das Projekt aus, um die Ausgabe der Abfrage anzuzeigen:

```console
Query:'turn on the bedroom light'
Top intent is 'HomeAutomation.TurnOn' with score 0.809439957
HomeAutomation.Room:'bedroom' begins at position 12 and ends at position 18
```

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich näher mit dem [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/) und der [.NET-Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet) vertraut. 

> [!div class="nextstepaction"] 
> [Tutorial: Erstellen einer LUIS-App zum Bestimmen von Benutzerabsichten](luis-quickstart-intents-only.md) 