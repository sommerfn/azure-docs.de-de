---
title: 'Schnellstart: Durchführen einer Neuigkeitensuche mit Ruby und der REST-API der Bing-News-Suche'
titlesuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung zum Senden einer Anforderung an die REST-API der Bing-News-Suche mit Ruby, um eine JSON-Antwort zu erhalten.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 6/19/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 5db8d9055ff973c3de1b995340d77004d889aa3d
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272729"
---
# <a name="quickstart-perform-a-news-search-using-ruby-and-the-bing-news-search-rest-api"></a>Schnellstart: Durchführen einer Neuigkeitensuche mit Ruby und der REST-API der Bing-News-Suche

Verwenden Sie diese Schnellstartanleitung, um die Bing-News-Suche-API zum ersten Mal aufzurufen und eine JSON-Antwort zu erhalten. Diese einfache JavaScript-Anwendung sendet eine Suchabfrage an die API und verarbeitet die Ergebnisse.

Diese Anwendung ist zwar in Python geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist. Den Quellcode des Beispiels finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingNewsSearchv7.rb).

## <a name="prerequisites"></a>Voraussetzungen

* Ruby [2.4 oder höher](https://www.ruby-lang.org/en/downloads/)

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

Siehe auch [Cognitive Services-Preise – Bing-Suche-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)

## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Importieren Sie die folgenden Pakete in Ihre Codedatei.

    ```ruby
    require 'net/https'
    require 'uri'
    require 'json'
    ```

2. Erstellen Sie Variablen für den API-Endpunkt, die News-Suche-URL, Ihren Abonnementschlüssel und einen Suchbegriff.

    ```ruby
    accessKey = "enter key here"
    uri  = "https://api.cognitive.microsoft.com"
    path = "/bing/v7.0/news/search"
    term = "Microsoft"
    ```

## <a name="format-and-make-an-api-request"></a>Formatieren und Senden einer API-Anforderung

Verwenden Sie die Variablen aus dem vorherigen Schritt, um eine Such-URL für die API-Anforderung zu formatieren. Senden Sie dann die Anforderung.

```ruby
uri = URI(uri + path + "?q=" + URI.escape(term))
request = Net::HTTP::Get.new(uri)
request['Ocp-Apim-Subscription-Key'] = accessKey
response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
   http.request(request)
end
```

## <a name="process-and-print-the-json-response"></a>Verarbeiten und Ausgeben der JSON-Antwort

Nachdem Sie die Antwort erhalten haben, können Sie den JSON-Code analysieren und den Antworttext und seine Header ausgeben:

```ruby
puts "\nRelevant Headers:\n\n"
response.each_header do |key, value|
   # header names are coerced to lowercase
   if key.start_with?("bingapis-") or key.start_with?("x-msedge-") then
      puts key + ": " + value
   end
end
puts "\nJSON Response:\n\n"
puts JSON::pretty_generate(JSON(response.body))
```

## <a name="json-response"></a>JSON-Antwort

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

```json
{
   "_type": "News",
   "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft",
   "totalEstimatedMatches": 36,
   "sort": [
      {
         "name": "Best match",
         "id": "relevance",
         "isSelected": true,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft"
      },
      {
         "name": "Most recent",
         "id": "date",
         "isSelected": false,
         "url": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/news\/search?q=Microsoft&sortby=date"
      }
   ],
   "value": [
      {
         "name": "Microsoft to open flagship London brick-and-mortar retail store",
         "url": "http:\/\/www.contoso.com\/article\/microsoft-to-open-flagshi...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.F9E4A49EC010417...",
               "width": 220,
               "height": 146
            }
         },
         "description": "After years of rumors about Microsoft opening a brick-and-mortar...", 
         "about": [
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entiti...", 
             "name": "Microsoft"
           }, 
           {
             "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entit...", 
             "name": "London"
           }
         ], 
         "provider": [
           {
             "_type": "Organization", 
             "name": "Contoso"
           }
         ], 
          "datePublished": "2017-09-21T21:16:00.0000000Z", 
          "category": "ScienceAndTechnology"
      }, 

      . . .
      
      {
         "name": "Microsoft adds Availability Zones to its Azure cloud platform",
         "url": "https:\/\/contoso.com\/2017\/09\/21\/microsoft-adds-availability...",
         "image": {
            "thumbnail": {
               "contentUrl": "https:\/\/www.bing.com\/th?id=ON.0AE7595B9720...",
               "width": 700,
               "height": 466
            }
         },
         "description": "Microsoft has begun adding Availability Zones to its...",
         "about": [
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/a093e9b...",
               "name": "Microsoft"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/cf3abf7d-e379-...",
               "name": "Windows Azure"
            },
            {
               "readLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/entities\/9cdd061c-1fae-d0...",
               "name": "Cloud"
            }
         ],
         "provider": [
            {
               "_type": "Organization",
               "name": "Contoso"
            }
         ],
         "datePublished": "2017-09-21T09:01:00.0000000Z",
         "category": "ScienceAndTechnology"
      }
   ]
}
```

 
## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer einseitigen App](tutorial-bing-news-search-single-page-app.md)
