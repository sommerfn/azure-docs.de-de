---
title: 'Schnellstart: Ermitteln der Textsprache, C#: Textübersetzungs-API'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie die Sprache des bereitgestellten Texts mithilfe von .NET Core und der Textübersetzungs-REST-API ermitteln.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/13/2019
ms.author: swmachan
ms.openlocfilehash: eaf9fa86437d2c69a9a1a68fba797f69c1339dd1
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448252"
---
# <a name="quickstart-use-the-translator-text-api-to-detect-text-language-using-c"></a>Schnellstart: Verwenden der Textübersetzungs-API zur Erkennung der Textsprache mit C#

In dieser Schnellstartanleitung erfahren Sie, wie Sie die Sprache des bereitgestellten Texts mithilfe von .NET Core, C# 7.1 oder höher und der Textübersetzungs-REST-API ermitteln.

Für diese Schnellstartanleitung wird ein [Azure Cognitive Services-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit einer Textübersetzungsressource benötigt. Wenn Sie über kein Konto verfügen, können Sie über die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/) einen Abonnementschlüssel abrufen.

## <a name="prerequisites"></a>Voraussetzungen

* C#7.1 oder höher
* [.NET SDK](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Json.NET-NuGet-Paket](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) oder ein anderer Editor
* Ein Azure-Abonnementschlüssel für die Textübersetzung

## <a name="create-a-net-core-project"></a>Erstellen eines .NET Core-Projekts

Öffnen Sie eine neue Eingabeaufforderung (oder Terminalsitzung), und führen Sie die folgenden Befehle aus:

```console
dotnet new console -o detect-sample
cd detect-sample
```

Mit dem ersten Befehl werden zwei Aufgaben ausgeführt. Es werden eine neue .NET-Konsolenanwendung und ein Verzeichnis mit dem Namen `detect-sample` erstellt. Mit dem zweiten Befehl wird in das Verzeichnis für Ihr Projekt gewechselt.

Als Nächstes müssen Sie Json.Net installieren. Führen Sie im Verzeichnis Ihres Projekts den folgenden Befehl aus:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="select-the-c-language-version"></a>Auswählen der C#-Sprachversion

Für diese Schnellstartanleitung wird C# 7.1 oder höher benötigt. Es gibt verschiedene Möglichkeiten, die C#-Version für Ihr Projekt zu ändern. In diesem Leitfaden wird gezeigt, wie Sie die Datei `detect-sample.csproj` anpassen. Alle verfügbaren Optionen, z.B. das Ändern der Sprache in Visual Studio, finden Sie unter [Auswählen der C#-Sprachversion](https://docs.microsoft.com/dotnet/csharp/language-reference/configure-language-version).

Öffnen Sie Ihr Projekt und anschließend `detect-sample.csproj`. Stellen Sie sicher, dass `LangVersion` auf 7.1 oder höher festgelegt ist. Wenn keine Eigenschaftengruppe für die Sprachversion vorhanden ist, fügen Sie diese Zeilen hinzu:

```xml
<PropertyGroup>
   <LangVersion>7.1</LangVersion>
</PropertyGroup>
```

## <a name="add-required-namespaces-to-your-project"></a>Hinzufügen von erforderlichen Namespaces zu Ihrem Projekt

Mit dem zuvor ausgeführten Befehl `dotnet new console` wurde ein Projekt (einschließlich `Program.cs`) erstellt. In diese Datei fügen Sie Ihren Anwendungscode ein. Öffnen Sie `Program.cs`, und ersetzen Sie die vorhandenen using-Anweisungen. Mit diesen Anweisungen wird sichergestellt, dass Sie Zugriff auf alle Typen haben, die zum Erstellen und Ausführen der Beispiel-App erforderlich sind.

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
// Install Newtonsoft.Json with NuGet
using Newtonsoft.Json;
```

## <a name="create-classes-for-the-json-response"></a>Erstellen von Klassen für die JSON-Antwort

Als Nächstes erstellen wir eine Klasse, die bei der Deserialisierung der von der Textübersetzungs-API zurückgegebenen JSON-Antwort verwendet wird.

```csharp
/// <summary>
/// The C# classes that represents the JSON returned by the Translator Text API.
/// </summary>
public class DetectResult
{
    public string Language { get; set; }
    public float Score { get; set; }
    public bool IsTranslationSupported { get; set; }
    public bool IsTransliterationSupported { get; set; }
    public AltTranslations[] Alternatives { get; set; }
}
public class AltTranslations
{
    public string Language { get; set; }
    public float Score { get; set; }
    public bool IsTranslationSupported { get; set; }
    public bool IsTransliterationSupported { get; set; }
}
```

## <a name="create-a-function-to-detect-the-source-texts-language"></a>Erstellen einer Funktion zum Ermitteln der Sprache des Quelltexts

Erstellen Sie in der `Program`-Klasse eine Funktion mit dem Namen `DetectTextRequest()`. Diese Klasse kapselt den zum Aufrufen der Ermittlungsressource verwendeten Code und gibt das Ergebnis in der Konsole aus.

```csharp
static public async Task DetectTextRequest(string subscriptionKey, string host, string route, string inputText)
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="serialize-the-detect-request"></a>Serialisieren der Ermittlungsanforderung

Als Nächstes müssen Sie das JSON-Objekt erstellen und serialisieren, das den Text enthält, für den die Sprachenerkennung ausgeführt wird.

```csharp
System.Object[] body = new System.Object[] { new { Text = inputText } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>Instanziieren des Clients und Senden einer Anforderung

Diese Zeilen instanziieren `HttpClient` und `HttpRequestMessage`:

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>Erstellen der Anforderung und Ausgeben der Antwort

In `HttpRequestMessage` führen Sie die folgenden Aufgaben aus:

* Deklarieren der HTTP-Methode
* Erstellen des Anforderungs-URIs
* Einfügen des Anforderungstexts (serialisiertes JSON-Objekt)
* Hinzufügen der erforderlichen Header
* Senden einer asynchronen Anforderung
* Ausgeben der Antwort

Fügen Sie `HttpRequestMessage` den folgenden Code hinzu:

```csharp
// Build the request.
request.Method = HttpMethod.Post;
// Construct the URI and add headers.
request.RequestUri = new Uri(host + route);
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send the request and get response.
HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
// Read response as a string.
string result = await response.Content.ReadAsStringAsync();
// Deserialize the response using the classes created earlier.
DetectResult[] deserializedOutput = JsonConvert.DeserializeObject<DetectResult[]>(result);
// Iterate over the deserialized response.
foreach (DetectResult o in deserializedOutput)
{
    Console.WriteLine("The detected language is '{0}'. Confidence is: {1}.\nTranslation supported: {2}.\nTransliteration supported: {3}.\n",
        o.Language, o.Score, o.IsTranslationSupported, o.IsTransliterationSupported);
    // Create a counter
    int counter = 0;
    // Iterate over alternate translations.
    foreach (AltTranslations a in o.Alternatives)
    {
        counter++;
        Console.WriteLine("Alternative {0}", counter);
        Console.WriteLine("The detected language is '{0}'. Confidence is: {1}.\nTranslation supported: {2}.\nTransliteration supported: {3}.\n",
            a.Language, a.Score, a.IsTranslationSupported, a.IsTransliterationSupported);
    }
}
```

Wenn Sie ein Cognitive Services-Abonnement mehrerer Dienste verwenden, müssen Sie auch `Ocp-Apim-Subscription-Region` in Ihre Anforderungsparameter aufnehmen. [Erfahren Sie mehr über die Authentifizierung mit dem Abonnement für mehrere Dienste](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication). 

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Im letzten Schritt rufen Sie `DetectTextRequest()` in der `Main`-Funktion auf. Suchen Sie nach `static void Main(string[] args)`, und ersetzen Sie dieses Element durch diesen Code:

```csharp
static async Task Main(string[] args)
{
    // This is our main function.
    // Output languages are defined in the route.
    // For a complete list of options, see API reference.
    string subscriptionKey = "YOUR_TRANSLATOR_TEXT_KEY_GOES_HERE";
    string host = "https://api.cognitive.microsofttranslator.com";
    string route = "/detect?api-version=3.0";
    string breakSentenceText = @"How are you doing today? The weather is pretty pleasant. Have you been to the movies lately?";
    await DetectTextRequest(subscriptionKey, host, route, breakSentenceText);
}
```
## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Sie können Ihre Beispiel-App jetzt ausführen. Navigieren Sie über die Befehlszeile (oder Terminalsitzung) zu Ihrem Projektverzeichnis, und führen Sie Folgendes aus:

```console
dotnet run
```

## <a name="sample-response"></a>Beispiel für eine Antwort

Nachdem Sie das Beispiel ausgeführt haben, sollte Folgendes im Terminal ausgegeben werden:

> [!NOTE]
> Die Abkürzung für das Land bzw. die Region finden Sie in [dieser Liste der Sprachen](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).

```bash
The detected language is 'en'. Confidence is: 1.
Translation supported: True.
Transliteration supported: False.

Alternative 1
The detected language is 'fil'. Confidence is: 0.82.
Translation supported: True.
Transliteration supported: False.

Alternative 2
The detected language is 'ro'. Confidence is: 1.
Translation supported: True.
Transliteration supported: False.
```

Diese Nachricht wird aus den JSON-Rohdaten gebildet und sieht so aus:

```json
[  
    {  
        "language":"en",
        "score":1.0,
        "isTranslationSupported":true,
        "isTransliterationSupported":false,
        "alternatives":[  
            {  
                "language":"fil",
                "score":0.82,
                "isTranslationSupported":true,
                "isTransliterationSupported":false
            },
            {  
                "language":"ro",
                "score":1.0,
                "isTranslationSupported":true,
                "isTransliterationSupported":false
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Entfernen Sie unbedingt alle vertraulichen Informationen wie etwa Abonnementschlüssel aus dem Quellcode Ihrer Beispiel-App.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Beispielcode für diese und andere Schnellstartanleitungen (einschließlich Übersetzung und Transliteration) sowie weitere Beispielprojekte für die Textübersetzung auf GitHub an.

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)

## <a name="see-also"></a>Weitere Informationen

* [Übersetzen von Text](quickstart-csharp-translate.md)
* [Transliteration von Text](quickstart-csharp-transliterate.md)
* [Ermitteln alternativer Übersetzungen](quickstart-csharp-dictionary.md)
* [Abrufen einer Liste der unterstützten Sprachen](quickstart-csharp-languages.md)
* [Bestimmen der Satzlänge aus einer Eingabe](quickstart-csharp-sentences.md)
