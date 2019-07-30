---
title: 'Schnellstart: Aufrufen der Bing-Websuche-REST-API mithilfe von Java'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung, um Bildsuchanforderungen mithilfe von Java an die Bing-Websuche-REST-API zu senden und eine JSON-Antwort zu empfangen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 07/22/2019
ms.author: aahi
ms.custom: seodec2018, seo-java-july2019
ms.openlocfilehash: 42cb270955846f1333b79389c3ca979dc4808f05
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489632"
---
# <a name="quickstart-search-the-web-using-the-bing-web-search-rest-api-and-java"></a>Schnellstart: Suchen im Internet unter Verwendung von Bing-Websuche-REST-API und Java

In dieser Schnellstartanleitung erfahren Sie, wie Sie die Bing-Websuche-API erstmals aufrufen und die JSON-Antwort empfangen. Diese Java-Anwendung sendet eine Suchanforderung an die API und zeigt die Antwort an. Diese Anwendung ist zwar in Java geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist.

## <a name="prerequisites"></a>Voraussetzungen

Im Folgenden sind die Tools aufgeführt, die Sie zum Ausführen dieser Schnellstartanleitung benötigen:

* [JDK 7 oder 8](https://aka.ms/azure-jdks)
* [Gson-Bibliothek](https://github.com/google/gson)
* Abonnementschlüssel

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="create-a-project-and-import-dependencies"></a>Erstellen eines Projekts und Importieren von Abhängigkeiten

Erstellen Sie in Ihrer bevorzugten IDE bzw. in einem Editor ein neues Java-Projekt, und importieren Sie die folgenden Bibliotheken. Gson ist erforderlich, um Java-Objekte in das JSON-Format zu konvertieren.

```java
import java.net.*;
import java.util.*;
import java.io.*;
import javax.net.ssl.HttpsURLConnection;
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
```

### <a name="declare-gson-in-the-maven-pom-file"></a>Deklarieren von Gson in der Maven-POM-Datei

Bei Verwendung von Maven deklarieren Sie Gson in der Datei `POM.xml`. Überspringen Sie diesen Schritt, wenn Sie Gson lokal installiert haben.

```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.5</version>
</dependency>
```

## <a name="declare-the-bingwebsearch-class"></a>Deklarieren der BingWebSearch-Klasse

Deklarieren Sie die `BingWebSearch`-Klasse. Sie enthält den Großteil des Codes, den wir in dieser Schnellstartanleitung behandeln, z.B. auch die `main`-Methode.  

```java
public class BingWebSearch {

// The code in the following sections goes here.

}
```

## <a name="define-variables"></a>Definieren von Variablen

Mit diesem Code werden `subscriptionKey`, `host`, `path` und `searchTerm` festgelegt. Vergewissern Sie sich, dass der Endpunkt korrekt ist, und ersetzen Sie den Wert `subscriptionKey` durch einen gültigen Abonnementschlüssel aus Ihrem Azure-Konto. Sie können die Suchabfrage auch anpassen, indem Sie den Wert für `searchTerm` ersetzen. Vergessen Sie nicht, diesen Code wie oben bereits erwähnt der Klasse `BingWebSearch` hinzuzufügen.

```java
// Enter a valid subscription key.
static String subscriptionKey = "enter key here";

/*
 * If you encounter unexpected authorization errors, double-check these values
 * against the endpoint for your Bing Web search instance in your Azure
 * dashboard.
 */
static String host = "https://api.cognitive.microsoft.com";
static String path = "/bing/v7.0/search";
static String searchTerm = "Microsoft Cognitive Services";
```

## <a name="construct-a-request"></a>Erstellen einer Anforderung

Diese Methode, die in der `BingWebSearch`-Klasse enthalten ist, wird verwendet, um die `url` zu erstellen, die Antwort zu empfangen und zu analysieren und Bing-bezogene HTTP-Header zu extrahieren.  

```java
public static SearchResults SearchWeb (String searchQuery) throws Exception {
    // Construct the URL.
    URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8"));

    // Open the connection.
    HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

    // Receive the JSON response body.
    InputStream stream = connection.getInputStream();
    String response = new Scanner(stream).useDelimiter("\\A").next();

    // Construct the result object.
    SearchResults results = new SearchResults(new HashMap<String, String>(), response);

    // Extract Bing-related HTTP headers.
    Map<String, List<String>> headers = connection.getHeaderFields();
    for (String header : headers.keySet()) {
        if (header == null) continue;      // may have null key
        if (header.startsWith("BingAPIs-") || header.startsWith("X-MSEdge-")){
            results.relevantHeaders.put(header, headers.get(header).get(0));
        }
    }
    stream.close();
    return results;
}
```

## <a name="handle-the-response"></a>Verarbeiten der Antwort

Verwenden Sie Gson, um die Antwort zu analysieren und neu zu serialisieren.

```java
public static String prettify(String json_text) {
    JsonParser parser = new JsonParser();
    JsonObject json = parser.parse(json_text).getAsJsonObject();
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
}
```

## <a name="declare-the-main-method"></a>Deklarieren der Main-Methode

Diese Methode ist erforderlich. Es ist die erste Methode, die beim Starten des Programms aufgerufen wird. In dieser Anwendung enthält sie Code, mit dem das `subscriptionKey`-Element überprüft, eine Anforderung gesendet und die JSON-Antwort ausgegeben wird.

```java
public static void main (String[] args) {
    // Confirm the subscriptionKey is valid.
    if (subscriptionKey.length() != 32) {
        System.out.println("Invalid Bing Search API subscription key!");
        System.out.println("Please paste yours into the source code.");
        System.exit(1);
    }

    // Call the SearchWeb method and print the response.
    try {
        System.out.println("Searching the Web for: " + searchTerm);
        SearchResults result = SearchWeb(searchTerm);
        System.out.println("\nRelevant HTTP Headers:\n");
        for (String header : result.relevantHeaders.keySet())
        System.out.println(header + ": " + result.relevantHeaders.get(header));
        System.out.println("\nJSON Response:\n");
        System.out.println(prettify(result.jsonResponse));
    }
    catch (Exception e) {
        e.printStackTrace(System.out);
        System.exit(1);
    }
}
```

## <a name="create-a-container-class-for-search-results"></a>Erstellen einer Containerklasse für Suchergebnisse

Die `SearchResults`-Containerklasse befindet sich außerhalb der `BingWebSearch`-Klasse. Sie enthält relevante Header und JSON-Daten für die Antwort.

```java
class SearchResults{
    HashMap<String, String> relevantHeaders;
    String jsonResponse;
    SearchResults(HashMap<String, String> headers, String json) {
        relevantHeaders = headers;
        jsonResponse = json;
    }
}
```

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Der letzte Schritt ist das Kompilieren Ihres Codes und dessen Ausführung! Hier sind die Befehle angegeben:

```powershell
javac BingWebSearch.java -classpath ./gson-2.8.5.jar -encoding UTF-8
java -cp ./gson-2.8.5.jar BingWebSearch
```

[Auf GitHub finden Sie Beispielcode](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingWebSearchv7.java), falls Sie Ihren Code mit unserem Code vergleichen möchten.

## <a name="sample-response"></a>Beispiel für eine Antwort

Antworten der Bing-Websuche-API werden im JSON-Format zurückgegeben. Diese Beispielantwort wurde gekürzt, damit nur ein Ergebnis angezeigt wird.

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Microsoft Cognitive Services"
  },
  "webPages": {
    "webSearchUrl": "https://www.bing.com/search?q=Microsoft+cognitive+services",
    "totalEstimatedMatches": 22300000,
    "value": [
      {
        "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0",
        "name": "Microsoft Cognitive Services",
        "url": "https://www.microsoft.com/cognitive-services",
        "displayUrl": "https://www.microsoft.com/cognitive-services",
        "snippet": "Knock down barriers between you and your ideas. Enable natural and contextual interaction with tools that augment users' experiences via the power of machine-based AI. Plug them in and bring your ideas to life.",
        "deepLinks": [
          {
            "name": "Face API",
            "url": "https://azure.microsoft.com/services/cognitive-services/face/",
            "snippet": "Add facial recognition to your applications to detect, identify, and verify faces using a Face API from Microsoft Azure. ... Cognitive Services; Face API;"
          },
          {
            "name": "Text Analytics",
            "url": "https://azure.microsoft.com/services/cognitive-services/text-analytics/",
            "snippet": "Cognitive Services; Text Analytics API; Text Analytics API . Detect sentiment, ... you agree that Microsoft may store it and use it to improve Microsoft services, ..."
          },
          {
            "name": "Computer Vision API",
            "url": "https://azure.microsoft.com/services/cognitive-services/computer-vision/",
            "snippet": "Extract the data you need from images using optical character recognition and image analytics with Computer Vision APIs from Microsoft Azure."
          },
          {
            "name": "Emotion",
            "url": "https://www.microsoft.com/cognitive-services/en-us/emotion-api",
            "snippet": "Cognitive Services Emotion API - microsoft.com"
          },
          {
            "name": "Bing Speech API",
            "url": "https://azure.microsoft.com/services/cognitive-services/speech/",
            "snippet": "Add speech recognition to your applications, including text to speech, with a speech API from Microsoft Azure. ... Cognitive Services; Bing Speech API;"
          },
          {
            "name": "Get Started for Free",
            "url": "https://azure.microsoft.com/services/cognitive-services/",
            "snippet": "Add vision, speech, language, and knowledge capabilities to your applications using intelligence APIs and SDKs from Cognitive Services."
          }
        ]
      }
    ]
  },
  "relatedSearches": {
    "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches",
    "value": [
      {
        "text": "microsoft bot framework",
        "displayText": "microsoft bot framework",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+bot+framework"
      },
      {
        "text": "microsoft cognitive services youtube",
        "displayText": "microsoft cognitive services youtube",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+youtube"
      },
      {
        "text": "microsoft cognitive services search api",
        "displayText": "microsoft cognitive services search api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+search+api"
      },
      {
        "text": "microsoft cognitive services news",
        "displayText": "microsoft cognitive services news",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+news"
      },
      {
        "text": "ms cognitive service",
        "displayText": "ms cognitive service",
        "webSearchUrl": "https://www.bing.com/search?q=ms+cognitive+service"
      },
      {
        "text": "microsoft cognitive services text analytics",
        "displayText": "microsoft cognitive services text analytics",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+text+analytics"
      },
      {
        "text": "microsoft cognitive services toolkit",
        "displayText": "microsoft cognitive services toolkit",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+toolkit"
      },
      {
        "text": "microsoft cognitive services api",
        "displayText": "microsoft cognitive services api",
        "webSearchUrl": "https://www.bing.com/search?q=microsoft+cognitive+services+api"
      }
    ]
  },
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "WebPages",
          "resultIndex": 0,
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#WebPages.0"
          }
        }
      ]
    },
    "sidebar": {
      "items": [
        {
          "answerType": "RelatedSearches",
          "value": {
            "id": "https://api.cognitive.microsoft.com/api/v7/#RelatedSearches"
          }
        }
      ]
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Einseitige Web-App für die Bing-Websuche](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]  
