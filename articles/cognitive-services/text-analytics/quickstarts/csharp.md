---
title: 'Schnellstart: Aufrufen des Textanalysediensts mithilfe des Azure SDK für .NET und C#'
titleSuffix: Azure Cognitive Services
description: Informationen und Codebeispiele für den Einstieg in die Verwendung des Textanalysediensts mit C#.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/30/2019
ms.author: assafi
ms.openlocfilehash: 6bd3907392dad626c1eeb1823c929f1a35d544dd
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697673"
---
# <a name="quickstart-use-the-net-sdk-and-c-to-call-the-text-analytics-service"></a>Schnellstart: Verwenden des .NET SDK mit C# für das Aufrufen des Textanalysediensts
<a name="HOLTop"></a>

Dieser Schnellstart unterstützt Sie bei den ersten Schritten mit dem Azure SDK für .NET und C# für die Sprachanalyse. Obwohl die [Textanalyse](//go.microsoft.com/fwlink/?LinkID=759711)-REST-API mit den meisten Programmiersprachen kompatibel ist, bietet das SDK eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren.

> [!NOTE]
> Der Democode in diesem Artikel enthält der Einfachheit halber die synchronen Methoden des .NET SDK für die Textanalyse. In Produktionsszenarien sollten dagegen zur Gewährleistung der Skalierbarkeit und Reaktionsfähigkeit der eigenen Anwendungen die asynchronen Batchmethoden verwendet werden. So können Sie beispielsweise `SentimentBatchAsync` anstelle von `Sentiment` verwenden.
>
> Eine asynchrone Batchversion dieses Beispiels finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics).

Technische Informationen finden Sie im SDK für .NET unter der [Referenz für die Textanalyse](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet).

## <a name="prerequisites"></a>Voraussetzungen

* Eine beliebige Edition von Visual Studio 2017 oder höher
* Das Textanalyse-[SDK für .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

## <a name="create-the-visual-studio-solution-and-install-the-sdk"></a>Erstellen der Visual Studio-Projektmappe und Installieren des SDK

1. Erstellen Sie ein neues Projekt vom Typ Konsolen-App (.NET Core). [Öffnen Sie Visual Studio.](https://visualstudio.microsoft.com/vs/)
1. Klicken Sie mit der rechten Maustaste auf die Projektmappe, und wählen Sie **NuGet-Pakete für Projektmappe verwalten** aus.
1. Wählen Sie die Registerkarte **Durchsuchen** aus. Suchen Sie nach **Microsoft.Azure.CognitiveServices.Language.TextAnalytics**.

## <a name="authenticate-your-credentials"></a>Authentifizieren Ihrer Anmeldeinformationen

1. Fügen Sie der Hauptklassendatei (standardmäßig „Program.cs“) die folgenden `using`-Anweisungen hinzu.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Net.Http;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    using Microsoft.Rest;
    ```

2. Erstellen Sie eine neue `ApiKeyServiceClientCredentials`-Klasse, um die Anmeldeinformationen zu speichern und sie jeder Anforderung hinzuzufügen.

    ```csharp
    class ApiKeyServiceClientCredentials : ServiceClientCredentials
    {
        private readonly string apiKey;

        public ApiKeyServiceClientCredentials(string apiKey)
        {
            this.apiKey = apiKey;
        }

        public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            if (request == null)
            {
                throw new ArgumentNullException("request");
            }
            request.Headers.Add("Ocp-Apim-Subscription-Key", this.apiKey);
            return base.ProcessHttpRequestAsync(request, cancellationToken);
        }
    }
    ```

3. Aktualisieren Sie die `Program`-Klasse. Fügen Sie ein Konstantenelement für Ihren Textanalyse-API-Schlüssel und ein weiteres für den Dienstendpunkt hinzu. Denken Sie daran, den richtigen Azure-Standort für Ihre Textanalyseressource zu verwenden.

    ```csharp
    //Enter your Text Analytics (TA) API Key (available in Azure Portal -> your TA resource -> Keys)
    private const string ApiKey = "enter-your-textanalytics-api-key-here";
    //You can get the resource location from Azure Portal -> your TA resource -> Overview
    private const string Endpoint = "enter-your-service-endpoint-here"; // For example: "https://<your-location>.api.cognitive.microsoft.com";
    ```

> [!Tip]
> Für eine sichere Bereitstellung von Geheimnissen in Produktionssystemen empfehlen wir die Verwendung von [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

## <a name="create-a-text-analytics-client"></a>Erstellen eines Textanalyseclients

Rufen Sie in der `Main`-Funktion des Projekts die gewünschte Beispielmethode auf. Übergeben Sie die Parameter `Endpoint` und `ApiKey`, die Sie definiert haben.

```csharp
    public static void Main(string[] args)
    {
        var credentials = new ApiKeyServiceClientCredentials(ApiKey);
        var client = new TextAnalyticsClient(credentials)
        {
            Endpoint = Endpoint
        };

        // Change the console encoding to display non-ASCII characters.
        Console.OutputEncoding = System.Text.Encoding.UTF8;
        SentimentAnalysisExample(client);
        // DetectLanguageExample(client);
        // RecognizeEntitiesExample(client);
        // KeyPhraseExtractionExample(client);
        Console.ReadLine();
    }
```

In den folgenden Abschnitten wird das Aufrufen der einzelnen Dienstfunktionen beschrieben.

## <a name="perform-sentiment-analysis"></a>Ausführen von Standpunktanalysen

1. Erstellen Sie eine neue `SentimentAnalysisExample()`-Funktion, die den zuvor erstellten Client annimmt.
2. Rufen Sie in der gleichen Funktion `client.Sentiment()` auf, und rufen Sie das Ergebnis ab. Das Ergebnis enthält bei erfolgreicher Ausführung die Stimmung `Score` und andernfalls `errorMessage`. Eine gegen 0 tendierende Punktzahl deutet auf eine negative Stimmung hin, eine gegen 1 tendierende Punktzahl auf eine positive.

    ```csharp
    var result = client.Sentiment("I had the best day of my life.", "en");

    // Printing sentiment results
    Console.WriteLine($"Sentiment Score: {result.Score:0.00}");
    ```

### <a name="output"></a>Output

```console
Sentiment Score: 0.87
```

## <a name="perform-language-detection"></a>Ausführen einer Sprachenerkennung

1. Erstellen Sie eine neue `DetectLanguageExample()`-Funktion, die den zuvor erstellten Client annimmt.
2. Rufen Sie in der gleichen Funktion `client.DetectLanguage()` auf, und rufen Sie das Ergebnis ab. Das Ergebnis enthält bei erfolgreicher Ausführung die Liste der erkannten Sprachen in `DetectedLanguages` und andernfalls `errorMessage`. Geben Sie dann die erste zurückgegebene Sprache aus.

    ```csharp
    var result = client.DetectLanguage("This is a document written in English.");

    // Printing detected languages
    Console.WriteLine($"Language: {result.DetectedLanguages[0].Name}");
    ```

> [!Tip]
> In einigen Fällen ist es unter Umständen schwierig, die Sprachen basierend auf den Eingaben eindeutig zu bestimmen. Mithilfe des Parameters `countryHint` können Sie einen zweistelligen Ländercode angeben. Die API verwendet standardmäßig „US“ als Wert für „countryHint“. Wenn Sie dieses Verhalten entfernen möchten, können Sie diesen Parameter zurücksetzen, indem Sie seinen Wert auf eine leere Zeichenfolge (`countryHint = ""`) festlegen.

### <a name="output"></a>Output

```console
Language: English
```

## <a name="perform-entity-recognition"></a>Ausführen von Entitätserkennungen

1. Erstellen Sie eine neue `RecognizeEntitiesExample()`-Funktion, die den zuvor erstellten Client annimmt.
2. Rufen Sie in der gleichen Funktion `client.Entities()` auf, und rufen Sie das Ergebnis ab. Durchlaufen Sie dann die Ergebnisse. Das Ergebnis enthält bei erfolgreicher Ausführung die Liste der erkannten Entitäten in `Entities` und andernfalls `errorMessage`. Geben Sie für jede erkannte Entität ihren Typ, Untertyp, Wikipedia-Namen (sofern vorhanden) sowie die Fundorte im Originaltext aus.

    ```csharp
    var result = client.Entities("Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.");

    // Printing recognized entities
    Console.WriteLine("Entities:");
    foreach (var entity in result.Entities)
    {
        Console.WriteLine($"\tName: {entity.Name},\tType: {entity.Type ?? "N/A"},\tSub-Type: {entity.SubType ?? "N/A"}");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.EntityTypeScore:F3}");
        }
    }
    ```

### <a name="output"></a>Output

```console
Entities:
    Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 0,      Length: 9,      Score: 1.000
    Name: Bill Gates,       Type: Person,   Sub-Type: N/A
        Offset: 25,     Length: 10,     Score: 1.000
    Name: Paul Allen,       Type: Person,   Sub-Type: N/A
        Offset: 40,     Length: 10,     Score: 0.999
    Name: April 4,  Type: Other,    Sub-Type: N/A
        Offset: 54,     Length: 7,      Score: 0.800
    Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
        Offset: 54,     Length: 13,     Score: 0.800
    Name: BASIC,    Type: Other,    Sub-Type: N/A
        Offset: 89,     Length: 5,      Score: 0.800
    Name: Altair 8800,      Type: Other,    Sub-Type: N/A
        Offset: 116,    Length: 11,     Score: 0.800
```

## <a name="perform-key-phrase-extraction"></a>Ausführen von Schlüsselwortextraktionen

1. Erstellen Sie eine neue `KeyPhraseExtractionExample()`-Funktion, die den zuvor erstellten Client annimmt.
2. Rufen Sie in der gleichen Funktion `client.KeyPhrases()` auf, und rufen Sie das Ergebnis ab. Das Ergebnis enthält bei erfolgreicher Ausführung die Liste der erkannten Schlüsselbegriffe in `KeyPhrases` und andernfalls `errorMessage`. Geben Sie dann alle erkannten Schlüsselbegriffe aus.

    ```csharp
    var result = client.KeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in result.KeyPhrases)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
    ```

### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Textanalyse mit Power BI](../tutorials/tutorial-power-bi-key-phrases.md)


* [Übersicht über die Textanalyse](../overview.md)
* [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)
