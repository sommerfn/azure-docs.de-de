---
title: 'Schnellstart: Projekt: Suche nach Antworten, Java'
titlesuffix: Azure Cognitive Services
description: Steigen Sie in Project Answer Search in Java ein.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: 26a9fc60c90f536e4ef9f03e96545e493a60f206
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698093"
---
# <a name="quickstart-project-answer-search-query-in-java"></a>Schnellstart: Project Answer Search-Abfrage in Java
In diesem Artikel wird Java verwendet, um die Bing-Antwortsuche-API zu veranschaulichen, die einen Teil der Microsoft Cognitive Services in Azure darstellt. Die API ist ein REST-Webdienst, der mit jeder Programmiersprache kompatibel ist, die HTTP-Anforderungen senden und JSON analysieren kann.
 
Der Beispielcode verwendet Java und weist nur minimale externe Abhängigkeiten auf.  Sie können auch unter Linux oder Mac OS X mit Mono ausführen.

## <a name="prerequisites"></a>Voraussetzungen

Rufen Sie einen Zugriffsschlüssel für die kostenlose Testversion von [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-answer-search) ab.

## <a name="request"></a>Anforderung 

Der folgende Code erstellt eine `WebRequest`, legt den Zugriffsschlüsselheader fest, und fügt eine Abfragezeichenfolge für „Gibraltar“ hinzu.  Dann übermittelt er die Anforderung und weist die Antwort einer Zeichenfolge zu, die den JSON-Text enthält.

```
    static String host = "https://api.labs.cognitive.microsoft.com";
    static String path = "/answerSearch/v7.0/search";

    // construct URL of search request (endpoint + query string)

    URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + &mkt=en-us");
    HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

    // receive JSON body
    InputStream stream = connection.getInputStream();
    String response = new Scanner(stream).useDelimiter("\\A").next();

    // construct result object for return
    SearchResults results = new SearchResults(new HashMap<String, String>(), response);
```

## <a name="complete-code"></a>Vollständiger Code

Die API der Bing-Antwortsuche gibt Ergebnisse der Bing-Suchmaschine zurück.
1. Laden Sie die gson-Bibliothek herunter, oder installieren Sie diese.
2. Erstellen Sie in Ihrer bevorzugten IDE oder in Ihrem bevorzugten Editor ein neues Java-Projekt.
3. Fügen Sie den unten stehenden Code hinzu.
4. Ersetzen Sie den Wert von „subscriptionKey“ durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
5. Führen Sie das Programm aus.

```
package knowledgeAPI;
import java.io.InputStream;
import java.net.*;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

import javax.net.ssl.HttpsURLConnection;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

public class KnowledgeSrch {

        // Replace the subscriptionKey string value with your valid subscription key.
        static String subscriptionKey = "YOUR-ACCESS-KEY";

        static String host = "https://api.labs.cognitive.microsoft.com";
        static String path = "/answerSearch/v7.0/search";

        static String searchTerm = "Gibraltar";

        public static SearchResults SearchKnowledge (String searchQuery) throws Exception {

            URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + "&mkt=en-us");
            
            HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
            connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

            // receive JSON body
            InputStream stream = connection.getInputStream();
            String response = new Scanner(stream).useDelimiter("\\A").next();

            // construct result object for return
            SearchResults results = new SearchResults(new HashMap<String, String>(), response);

            // extract Bing-related HTTP headers
            Map<String, List<String>> headers = connection.getHeaderFields();
            for (String header : headers.keySet()) {
                if (header == null) continue;      // may have null key
                if (header.startsWith("BingAPIs-") || header.startsWith("X-MSEdge-")) {
                    results.relevantHeaders.put(header, headers.get(header).get(0));
                }
            }

            stream.close();
            return results;
        }

        // pretty-printer for JSON; uses GSON parser to parse and re-serialize
        public static String prettify(String json_text) {
            JsonParser parser = new JsonParser();
            JsonObject json = parser.parse(json_text).getAsJsonObject();
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            return gson.toJson(json);
        }

        public static void main (String[] args) {
            try {
                System.out.println("Searching the Web for: " + searchTerm);

                SearchResults result = SearchKnowledge(searchTerm);

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
    }

    // Container class for search results encapsulates relevant headers and JSON data
    class SearchResults{
        HashMap<String, String> relevantHeaders;
        String jsonResponse;
        SearchResults(HashMap<String, String> headers, String json) {
            relevantHeaders = headers;
            jsonResponse = json;
        }
}

```

## <a name="next-steps"></a>Nächste Schritte
- [C#-Schnellstart](c-sharp-quickstart.md)
- [Java-Schnellstart](java-quickstart.md)
- [Node-Schnellstart](node-quickstart.md)
