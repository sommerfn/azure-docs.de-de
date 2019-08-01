---
title: 'Schnellstart: Aufrufen Ihres Endpunkts für die benutzerdefinierte Bing-Suche mit Java | Microsoft-Dokumentation'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung, um mit dem Anfordern von Suchergebnissen von Ihrer Instanz der benutzerdefinierten Bing-Suche in Java zu beginnen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 07/26/2019
ms.author: aahi
ms.openlocfilehash: afb804dd3fd0fecd676f04f18db577f87861a163
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564680"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-java"></a>Schnellstart: Aufrufen Ihres Endpunkts für die benutzerdefinierte Bing-Suche mit Java

Verwenden Sie diese Schnellstartanleitung, um mit dem Anfordern von Suchergebnissen von Ihrer Instanz der benutzerdefinierten Bing-Suche zu beginnen. Diese Anwendung ist zwar in Java geschrieben, aber die API für die benutzerdefinierte Bing-Suche ist ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist. Den Quellcode des Beispiels finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingCustomSearchv7.java).

## <a name="prerequisites"></a>Voraussetzungen

- Eine Instanz der benutzerdefinierten Bing-Suche. Weitere Informationen finden Sie unter [Schnellstart: Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).

- Das neueste [Java Development Kit](https://www.oracle.com/technetwork/java/javase/downloads/index.html)  

- Die [Gson-Bibliothek](https://github.com/google/gson)

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie in Ihrer bevorzugten IDE oder in Ihrem bevorzugten Editor ein neues Java-Projekt, und importieren Sie die folgenden Bibliotheken.

    ```java
    import java.io.InputStream;
    import java.net.URL;
    import java.net.URLEncoder;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Scanner;
    import javax.net.ssl.HttpsURLConnection;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    ```

2. Erstellen Sie eine Klasse namens `CustomSrchJava`, und erstellen Sie Variablen für Ihren Abonnementschlüssel, den Endpunkt für die benutzerdefinierte Suche und die benutzerdefinierte Konfigurations-ID Ihrer Suchinstanz. 
    ```java
    public class CustomSrchJava {
        static String host = "https://api.cognitive.microsoft.com";
        static String path = "/bingcustomsearch/v7.0/search";
        static String subscriptionKey = "YOUR-SUBSCRIPTION-KEY"; 
        static String customConfigId = "YOUR-CUSTOM-CONFIG-ID";
        static String searchTerm = "Microsoft";  
    ...
    ```

3. Erstellen Sie eine weitere Klasse namens `SearchResults`, die die Antwort von Ihrer Instanz der benutzerdefinierten Bing-Suche enthalten soll.

    ```csharp
    class SearchResults{
        HashMap<String, String> relevantHeaders;
        String jsonResponse;
        SearchResults(HashMap<String, String> headers, String json) {
            relevantHeaders = headers;
            jsonResponse = json;
        }
    }
    ```

4. Erstellen Sie eine Funktion namens `prettify()` zum Formatieren der JSON-Antwort von der API für die benutzerdefinierte Bing-Suche.

    ```java
        // pretty-printer for JSON; uses GSON parser to parse and re-serialize
        public static String prettify(String json_text) {
            JsonParser parser = new JsonParser();
            JsonObject json = parser.parse(json_text).getAsJsonObject();
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            return gson.toJson(json);
        }
    ```

## <a name="send-and-receive-a-search-request"></a>Senden und Empfangen einer Suchanforderung 

1. Erstellen Sie eine Funktion namens `SearchWeb()`, die eine Anforderung sendet und ein `SearchResults`-Objekt zurückgibt. Erstellen Sie die Anforderungs-URL, indem Sie Ihre benutzerdefinierte Konfigurations-ID, Abfrage- und Endpunktinformationen kombinieren. Fügen Sie dem `Ocp-Apim-Subscription-Key`-Header Ihren Abonnementschlüssel hinzu.

    ```java
    public class CustomSrchJava {
    ...
        public static SearchResults SearchWeb (String searchQuery) throws Exception {
            // construct the URL for your search request (endpoint + query string)
            URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchTerm, "UTF-8") + "&CustomConfig=" + customConfigId);
            HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
            connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
    ...
    ```

2. Erstellen Sie einen Stream, und speichern Sie die JSON-Antwort in einem `SearchResults`-Objekt.

    ```java
    public class CustomSrchJava {
    ...
        public static SearchResults SearchWeb (String searchQuery) throws Exception {
            ...
            // receive the JSON body
            InputStream stream = connection.getInputStream();
            String response = new Scanner(stream).useDelimiter("\\A").next();
        
            // construct result object for return
            SearchResults results = new SearchResults(new HashMap<String, String>(), response);
            
            stream.close();
            return results;
        }
    ```

3. Rufen Sie in der Main-Methode Ihrer Anwendung `SearchWeb()` mit dem Suchbegriff auf. 

    ```java
    System.out.println("\nJSON Response:\n");
    System.out.println(prettify(result.jsonResponse));
    ```

4. Führen Sie das Programm aus.
    
## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Web-App für die benutzerdefinierte Suche](./tutorials/custom-search-web-page.md)
