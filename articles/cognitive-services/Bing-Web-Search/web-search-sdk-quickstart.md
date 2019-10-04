---
title: 'Schnellstart: Verwenden des Bing-Websuche-SDK für C#'
titleSuffix: Azure Cognitive Services
description: Das Bing-Websuche-SDK erleichtert die Integration der Bing-Websuche in Ihre C#-Anwendung. In dieser Schnellstartanleitung erfahren Sie, wie Sie einen Client instanziieren, eine Anforderung senden und die Antwort ausgeben.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 04/15/2019
ms.author: aahi
ms.openlocfilehash: 5e893fda5a0e71703eb867c045de19d391c012ed
ms.sourcegitcommit: 71db032bd5680c9287a7867b923bf6471ba8f6be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2019
ms.locfileid: "71018226"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-c"></a>Schnellstart: Verwenden des Bing-Websuche-SDK für C#

Das Bing-Websuche-SDK erleichtert die Integration der Bing-Websuche in Ihre C#-Anwendung. In dieser Schnellstartanleitung erfahren Sie, wie Sie einen Client instanziieren, eine Anforderung senden und die Antwort ausgeben.

[!INCLUDE [bing-web-search-quickstart-signup](../../../includes/bing-web-search-quickstart-signup.md)]

Siehe auch [Cognitive Services-Preise – Bing-Suche-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)

## <a name="prerequisites"></a>Voraussetzungen
Im Folgenden sind die Tools aufgeführt, die Sie zum Ausführen dieser Schnellstartanleitung benötigen:

* [Visual Studio](https://visualstudio.microsoft.com/downloads/) oder
* [Visual Studio Code 2017](https://code.visualstudio.com/download)
  * [C# für Visual Studio Code](https://visualstudio.microsoft.com/downloads/)
  * [NuGet-Paket-Manager](https://github.com/jmrog/vscode-nuget-package-manager)
* [.NET Core SDK](https://www.microsoft.com/net/download)

## <a name="create-a-project-and-install-dependencies"></a>Erstellen eines Projekts und Installieren der Abhängigkeiten

> [!TIP]
> Laden Sie den aktuellen Code als Visual Studio-Projektmappe von [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/) herunter.

Der erste Schritt ist die Erstellung eines neuen Konsolenprojekts. Falls Sie Hilfe beim Einrichten eines Konsolenprojekts benötigen, lesen Sie [Hallo Welt – Ihr erstes Programm (C#-Programmierhandbuch)](https://docs.microsoft.com/dotnet/csharp/programming-guide/inside-a-program/hello-world-your-first-program). Um das Bing-Websuche-SDK in Ihrer Anwendung verwenden zu können, müssen Sie `Microsoft.Azure.CognitiveServices.Search.WebSearch` mithilfe des NuGet-Paket-Managers installieren.

Beim Installieren des [Pakets mit dem SDK für die Websuche](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.WebSearch/1.2.0) werden auch die folgenden Komponenten installiert:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="declare-dependencies"></a>Deklarieren von Abhängigkeiten

Öffnen Sie Ihr Projekt in Visual Studio oder Visual Studio Code, und importieren Sie die folgenden Abhängigkeiten:

```csharp
using System;
using System.Collections.Generic;
using Microsoft.Azure.CognitiveServices.Search.WebSearch;
using Microsoft.Azure.CognitiveServices.Search.WebSearch.Models;
using System.Linq;
```

## <a name="create-project-scaffolding"></a>Erstellen des Projektgerüstbaus

Wenn Sie Ihr neues Konsolenprojekt erstellt haben, sollten ein Namespace und eine Klasse für Ihre Anwendung erstellt worden sein. Ihr Programm sollte wie das folgende Beispiel aussehen:

```csharp
namespace WebSearchSDK
{
    class YOUR_PROGRAM
    {

        // The code in the following sections goes here.

    }
}
```

In den folgenden Abschnitten erstellen wir unsere Beispielanwendung innerhalb dieser Klasse.

## <a name="construct-a-request"></a>Erstellen einer Anforderung

Durch diesen Code wird die Suchabfrage erstellt.

```csharp
public static async void WebResults(WebSearchClient client)
{
    try
    {
        var webData = await client.Web.SearchAsync(query: "Yosemite National Park");
        Console.WriteLine("Searching for \"Yosemite National Park\"");

        // Code for handling responses is provided in the next section...

    }
    catch (Exception ex)
    {
        Console.WriteLine("Encountered exception. " + ex.Message);
    }
}
```

## <a name="handle-the-response"></a>Verarbeiten der Antwort

Als Nächstes fügen Sie Code zum Analysieren der Antwort und Ausgeben der Ergebnisse hinzu. Der `Name` und die `Url` für die erste Webseite, das erste Bild, den ersten Nachrichtenartikel und das erste Video werden ausgegeben, wenn sie im Antwortobjekt vorhanden sind.

```csharp
if (webData?.WebPages?.Value?.Count > 0)
{
    // find the first web page
    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

    if (firstWebPagesResult != null)
    {
        Console.WriteLine("Webpage Results # {0}", webData.WebPages.Value.Count);
        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
    }
    else
    {
        Console.WriteLine("Didn't find any web pages...");
    }
}
else
{
    Console.WriteLine("Didn't find any web pages...");
}

/*
 * Images
 * If the search response contains images, the first result's name
 * and url are printed.
 */
if (webData?.Images?.Value?.Count > 0)
{
    // find the first image result
    var firstImageResult = webData.Images.Value.FirstOrDefault();

    if (firstImageResult != null)
    {
        Console.WriteLine("Image Results # {0}", webData.Images.Value.Count);
        Console.WriteLine("First Image result name: {0} ", firstImageResult.Name);
        Console.WriteLine("First Image result URL: {0} ", firstImageResult.ContentUrl);
    }
    else
    {
        Console.WriteLine("Didn't find any images...");
    }
}
else
{
    Console.WriteLine("Didn't find any images...");
}

/*
 * News
 * If the search response contains news articles, the first result's name
 * and url are printed.
 */
if (webData?.News?.Value?.Count > 0)
{
    // find the first news result
    var firstNewsResult = webData.News.Value.FirstOrDefault();

    if (firstNewsResult != null)
    {
        Console.WriteLine("\r\nNews Results # {0}", webData.News.Value.Count);
        Console.WriteLine("First news result name: {0} ", firstNewsResult.Name);
        Console.WriteLine("First news result URL: {0} ", firstNewsResult.Url);
    }
    else
    {
        Console.WriteLine("Didn't find any news articles...");
    }
}
else
{
    Console.WriteLine("Didn't find any news articles...");
}

/*
 * Videos
 * If the search response contains videos, the first result's name
 * and url are printed.
 */
if (webData?.Videos?.Value?.Count > 0)
{
    // find the first video result
    var firstVideoResult = webData.Videos.Value.FirstOrDefault();

    if (firstVideoResult != null)
    {
        Console.WriteLine("\r\nVideo Results # {0}", webData.Videos.Value.Count);
        Console.WriteLine("First Video result name: {0} ", firstVideoResult.Name);
        Console.WriteLine("First Video result URL: {0} ", firstVideoResult.ContentUrl);
    }
    else
    {
        Console.WriteLine("Didn't find any videos...");
    }
}
else
{
    Console.WriteLine("Didn't find any videos...");
}
```

## <a name="declare-the-main-method"></a>Deklarieren der Main-Methode

In dieser Anwendung enthält die main-Methode Code, der den Client instanziiert, `subscriptionKey` überprüft und `WebResults` aufruft. Geben Sie einen gültigen Abonnementschlüssel für Ihr Azure-Konto ein, bevor Sie fortfahren.

```csharp
static void Main(string[] args)
{
    var client = new WebSearchClient(new ApiKeyServiceClientCredentials("YOUR_SUBSCRIPTION_KEY"));

    WebResults(client);

    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();
}
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie nun die Anwendung aus.

```console
dotnet run
```

## <a name="define-functions-and-filter-results"></a>Definieren von Funktionen und Filtern der Ergebnisse

Nachdem Sie Ihren ersten Aufruf der Bing-Websuche-API durchgeführt haben, sehen wir uns nun einige der SDK-Funktionen zum Verfeinern von Abfragen und Filtern der Ergebnisse an. Jede dieser Funktionen kann der im vorherigen Abschnitt erstellten C#-Anwendung hinzugefügt werden.

### <a name="limit-the-number-of-results-returned-by-bing"></a>Einschränken der von Bing zurückgegebenen Anzahl von Ergebnissen

In diesem Beispiel wird mit den Parametern `count` und `offset` die Anzahl von Ergebnissen eingeschränkt, die für „Best restaurants in Seattle“ zurückgegeben werden. Der `Name` und die `Url` für das erste Ergebnis werden ausgegeben.

1. Fügen Sie Ihrem Konsolenprojekt den folgenden Code hinzu:

    ```csharp
    public static async void WebResultsWithCountAndOffset(WebSearchClient client)
    {
        try
        {
            var webData = await client.Web.SearchAsync(query: "Best restaurants in Seattle", offset: 10, count: 20);
            Console.WriteLine("\r\nSearching for \" Best restaurants in Seattle \"");

            if (webData?.WebPages?.Value?.Count > 0)
            {
                var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

                if (firstWebPagesResult != null)
                {
                    Console.WriteLine("Web Results #{0}", webData.WebPages.Value.Count);
                    Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
                    Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
                }
                else
                {
                    Console.WriteLine("Couldn't find first web result!");
                }
            }
            else
            {
                Console.WriteLine("Didn't see any Web data..");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine("Encountered exception. " + ex.Message);
        }
    }
    ```

2. Fügen Sie `WebResultsWithCountAndOffset` zu `main` hinzu:

    ```csharp
    static void Main(string[] args)
    {
        var client = new WebSearchClient(new ApiKeyServiceClientCredentials("YOUR_SUBSCRIPTION_KEY"));

        WebResults(client);
        // Search with count and offset...
        WebResultsWithCountAndOffset(client);  

        Console.WriteLine("Press any key to exit...");
        Console.ReadKey();
    }
    ```

3. Führen Sie die Anwendung aus.

### <a name="filter-for-news"></a>Filtern nach Nachrichtenartikeln

In diesem Beispiel wird der `response_filter`-Parameter zum Filtern der Suchergebnisse verwendet. Die zurückgegebenen Suchergebnisse sind auf Nachrichtenartikel für „Microsoft“ beschränkt. Der `Name` und die `Url` für das erste Ergebnis werden ausgegeben.

1. Fügen Sie Ihrem Konsolenprojekt den folgenden Code hinzu:

    ```csharp
    public static async void WebSearchWithResponseFilter(WebSearchClient client)
    {
        try
        {
            IList<string> responseFilterstrings = new List<string>() { "news" };
            var webData = await client.Web.SearchAsync(query: "Microsoft", responseFilter: responseFilterstrings);
            Console.WriteLine("\r\nSearching for \" Microsoft \" with response filter \"news\"");

            if (webData?.News?.Value?.Count > 0)
            {
                var firstNewsResult = webData.News.Value.FirstOrDefault();

                if (firstNewsResult != null)
                {
                    Console.WriteLine("News Results #{0}", webData.News.Value.Count);
                    Console.WriteLine("First news result name: {0} ", firstNewsResult.Name);
                    Console.WriteLine("First news result URL: {0} ", firstNewsResult.Url);
                }
                else
                {
                    Console.WriteLine("Couldn't find first News results!");
                }
            }
            else
            {
                Console.WriteLine("Didn't see any News data..");
            }

        }
        catch (Exception ex)
        {
            Console.WriteLine("Encountered exception. " + ex.Message);
        }
    }
    ```

2. Fügen Sie `WebResultsWithCountAndOffset` zu `main` hinzu:

    ```csharp
    static void Main(string[] args)
    {
        var client = new WebSearchClient(new ApiKeyServiceClientCredentials("YOUR_SUBSCRIPTION_KEY"));

        WebResults(client);
        // Search with count and offset...
        WebResultsWithCountAndOffset(client);  
        // Search with news filter...
        WebSearchWithResponseFilter(client);

        Console.WriteLine("Press any key to exit...");
        Console.ReadKey();
    }
    ```

3. Führen Sie die Anwendung aus.

### <a name="use-safe-search-answer-count-and-the-promote-filter"></a>Verwenden der Filter für sichere Suche, Antwortanzahl und Höherstufen

In diesem Beispiel werden die Parameter `answer_count`, `promote` und `safe_search` zum Filtern der Suchergebnisse für „Music Videos“ verwendet. Der `Name` und die `ContentUrl` für das erste Ergebnis werden angezeigt.

1. Fügen Sie Ihrem Konsolenprojekt den folgenden Code hinzu:

    ```csharp
    public static async void WebSearchWithAnswerCountPromoteAndSafeSearch(WebSearchClient client)
    {
        try
        {
            IList<string> promoteAnswertypeStrings = new List<string>() { "videos" };
            var webData = await client.Web.SearchAsync(query: "Music Videos", answerCount: 2, promote: promoteAnswertypeStrings, safeSearch: SafeSearch.Strict);
            Console.WriteLine("\r\nSearching for \"Music Videos\"");

            if (webData?.Videos?.Value?.Count > 0)
            {
                var firstVideosResult = webData.Videos.Value.FirstOrDefault();

                if (firstVideosResult != null)
                {
                    Console.WriteLine("Video Results # {0}", webData.Videos.Value.Count);
                    Console.WriteLine("First Video result name: {0} ", firstVideosResult.Name);
                    Console.WriteLine("First Video result URL: {0} ", firstVideosResult.ContentUrl);
                }
                else
                {
                    Console.WriteLine("Couldn't find videos results!");
                }
            }
            else
            {
                Console.WriteLine("Didn't see any data..");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine("Encountered exception. " + ex.Message);
        }
    }
    ```

2. Fügen Sie `WebResultsWithCountAndOffset` zu `main` hinzu:

    ```csharp
    static void Main(string[] args)
    {
        var client = new WebSearchClient(new ApiKeyServiceClientCredentials("YOUR_SUBSCRIPTION_KEY"));

        WebResults(client);
        // Search with count and offset...
        WebResultsWithCountAndOffset(client);  
        // Search with news filter...
        WebSearchWithResponseFilter(client);
        // Search with answer count, promote, and safe search
        WebSearchWithAnswerCountPromoteAndSafeSearch(client);

        Console.WriteLine("Press any key to exit...");
        Console.ReadKey();
    }
    ```

3. Führen Sie die Anwendung aus.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Denken Sie daran, nach dem Fertigstellen dieses Projekts Ihren Abonnementschlüssel aus dem Anwendungscode zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Cognitive Services SDK-Beispiele für Node.js](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/)
