---
title: 'Schnellstart: Verwenden von Ruby zum Aufrufen der Textanalyse-API'
titleSuffix: Azure Cognitive Services
description: Informationen und Codebeispiele für die ersten Schritte mit der Textanalyse-API in Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: aahi
ms.openlocfilehash: 9c7c85ae9573efa202f5fc27ae78aee57fa67ab8
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70142697"
---
# <a name="quickstart-using-ruby-to-call-the-text-analytics-cognitive-service"></a>Schnellstart: Verwenden von Ruby zum Aufrufen der Textanalyse von Cognitive Services
<a name="HOLTop"></a>

Dieser Artikel veranschaulicht, wie Sie mithilfe der  [Textanalyse-APIs](//go.microsoft.com/fwlink/?LinkID=759711) mit Ruby eine [Sprache erkennen](#Detect), [Stimmungen analysieren](#SentimentAnalysis), [Schlüsselbegriffe extrahieren](#KeyPhraseExtraction) und [verknüpfte Entitäten erkennen](#Entities).

Die technische Dokumentation für die APIs finden Sie in den [API-Definitionen](//go.microsoft.com/fwlink/?LinkID=759346).

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

<a name="Detect"></a>

## <a name="detect-language"></a>Sprache erkennen

Die Sprachenerkennungs-API erfasst die Sprache eines Textdokuments mithilfe der [Sprachenerkennungsmethode](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7).

1. Erstellen Sie die Umgebungsvariablen `TEXT_ANALYTICS_SUBSCRIPTION_KEY` und `TEXT_ANALYTICS_ENDPOINT` für den Azure-Endpunkt und -Abonnementschlüssel Ihrer Ressource. Wenn Sie diese Umgebungsvariablen erstellt haben, nachdem Sie mit der Bearbeitung der Anwendung begonnen haben, müssen der Editor, die IDE oder die Shell, den bzw. die Sie für den Zugriff auf die Umgebungsvariablen verwenden, geschlossen und erneut geöffnet werden.
1. Erstellen Sie in Ihrer bevorzugten IDE ein neues Ruby-Projekt.
1. Fügen Sie den unten stehenden Code hinzu.
1. Führen Sie das Programm aus.

```ruby
# encoding: UTF-8

require 'net/https'
require 'uri'
require 'json'

key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end

path = '/text/analytics/v2.1/languages'

uri = URI(endpoint + path)

documents = { 'documents': [
    { 'id' => '1', 'text' => 'This is a document written in English.' },
    { 'id' => '2', 'text' => 'Este es un document escrito en Español.' },
    { 'id' => '3', 'text' => '这是一个用中文写的文件' }
]}

puts 'Please wait a moment for the results to appear.'

request = Net::HTTP::Post.new(uri)
request['Content-Type'] = "application/json"
request['Ocp-Apim-Subscription-Key'] = subscription_key
request.body = documents.to_json

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

puts JSON::pretty_generate (JSON (response.body))
```

**Antwort der Spracherkennung**

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt: 

```json

{
   "documents": [
      {
         "id": "1",
         "detectedLanguages": [
            {
               "name": "English",
               "iso6391Name": "en",
               "score": 1.0
            }
         ]
      },
      {
         "id": "2",
         "detectedLanguages": [
            {
               "name": "Spanish",
               "iso6391Name": "es",
               "score": 1.0
            }
         ]
      },
      {
         "id": "3",
         "detectedLanguages": [
            {
               "name": "Chinese_Simplified",
               "iso6391Name": "zh_chs",
               "score": 1.0
            }
         ]
      }
   ],
   "errors": [

   ]
}


```
<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Analysieren von Stimmungen

Die Standpunktanalyse-API erkennt die Stimmung in einer Gruppe von Textdatensätzen mithilfe der [Sentiment-Methode](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9). Im folgenden Beispiel werden zwei Dokumente bewertet, ein englisches und ein spanisches.

1. Erstellen Sie die Umgebungsvariablen `TEXT_ANALYTICS_SUBSCRIPTION_KEY` und `TEXT_ANALYTICS_ENDPOINT` für den Azure-Endpunkt und -Abonnementschlüssel Ihrer Ressource. Wenn Sie diese Umgebungsvariablen erstellen, nachdem Sie mit der Bearbeitung der Anwendung begonnen haben, müssen der Editor, die IDE oder die Shell, den bzw. die Sie für den Zugriff auf die Variablen verwenden, geschlossen und erneut geöffnet werden.
1. Erstellen Sie in Ihrer bevorzugten IDE ein neues Ruby-Projekt.
1. Fügen Sie den unten stehenden Code hinzu.
1. Führen Sie das Programm aus.

```ruby
# encoding: UTF-8

require 'net/https'
require 'uri'
require 'json'

key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end

path = '/text/analytics/v2.1/sentiment'

uri = URI(endpoint + path)

documents = { 'documents': [
    { 'id' => '1', 'language' => 'en', 'text' => 'I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable.' },
    { 'id' => '2', 'language' => 'es', 'text' => 'Este ha sido un dia terrible, llegué tarde al trabajo debido a un accidente automobilistico.' }
]}

puts 'Please wait a moment for the results to appear.'

request = Net::HTTP::Post.new(uri)
request['Content-Type'] = "application/json"
request['Ocp-Apim-Subscription-Key'] = subscription_key
request.body = documents.to_json

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

puts JSON::pretty_generate (JSON (response.body))
```

**Antwort der Stimmungsanalyse**

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt: 

```json
{
   "documents": [
      {
         "score": 0.99984133243560791,
         "id": "1"
      },
      {
         "score": 0.024017512798309326,
         "id": "2"
      },
   ],
   "errors": [   ]
}
```

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Extrahieren von Schlüsselbegriffen

Die API zur Schlüsselbegriffserkennung extrahiert Schlüsselbegriffe aus einem Textdokument mithilfe der [Schlüsselbegriffsmethode](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6). Im folgenden Beispiel werden Schlüsselbegriffe sowohl für englische als auch für spanische Dokumente extrahiert.

1. Erstellen Sie die Umgebungsvariablen `TEXT_ANALYTICS_SUBSCRIPTION_KEY` und `TEXT_ANALYTICS_ENDPOINT` für den Azure-Endpunkt und -Abonnementschlüssel Ihrer Ressource. Wenn Sie diese Umgebungsvariablen erstellen, nachdem Sie mit der Bearbeitung der Anwendung begonnen haben, müssen der Editor, die IDE oder die Shell, den bzw. die Sie für den Zugriff auf die Variablen verwenden, geschlossen und erneut geöffnet werden.
1. Erstellen Sie in Ihrer bevorzugten IDE ein neues Ruby-Projekt.
1. Fügen Sie den unten stehenden Code hinzu.
1. Führen Sie das Programm aus.


```ruby
# encoding: UTF-8

require 'net/https'
require 'uri'
require 'json'

key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end

path = '/text/analytics/v2.1/keyPhrases'

uri = URI(endpoint + path)

documents = { 'documents': [
    { 'id' => '1', 'language' => 'en', 'text' => 'I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable.' },
    { 'id' => '2', 'language' => 'es', 'text' => 'Si usted quiere comunicarse con Carlos, usted debe de llamarlo a su telefono movil. Carlos es muy responsable, pero necesita recibir una notificacion si hay algun problema.' },
    { 'id' => '3', 'language' => 'en', 'text' => 'The Grand Hotel is a new hotel in the center of Seattle. It earned 5 stars in my review, and has the classiest decor I\'ve ever seen.' },
]}

puts 'Please wait a moment for the results to appear.'

request = Net::HTTP::Post.new(uri)
request['Content-Type'] = "application/json"
request['Ocp-Apim-Subscription-Key'] = subscription_key
request.body = documents.to_json

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

puts JSON::pretty_generate (JSON (response.body))
```

**Antwort der Schlüsselwortextraktion**

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt: 

```json
{
   "documents": [
      {
         "keyPhrases": [
            "HDR resolution",
            "new XBox",
            "clean look"
         ],
         "id": "1"
      },
      {
         "keyPhrases": [
            "Carlos",
            "notificacion",
            "algun problema",
            "telefono movil"
         ],
         "id": "2"
      },
      {
         "keyPhrases": [
            "new hotel",
            "Grand Hotel",
            "review",
            "center of Seattle",
            "classiest decor",
            "stars"
         ],
         "id": "3"
      }
   ],
   "errors": [  ]
}
```
<a name="Entities"></a>

## <a name="entity-recognition"></a>Entitätserkennung

Die Entitäts-API extrahiert Entitäten in einem Textdokument mithilfe der [Entitätsmethode](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634). Im folgenden Beispiel werden Entitäten für englische Dokumente erkannt.

1. Erstellen Sie die Umgebungsvariablen `TEXT_ANALYTICS_SUBSCRIPTION_KEY` und `TEXT_ANALYTICS_ENDPOINT` für den Azure-Endpunkt und -Abonnementschlüssel Ihrer Ressource. Wenn Sie diese Umgebungsvariablen erstellen, nachdem Sie mit der Bearbeitung der Anwendung begonnen haben, müssen der Editor, die IDE oder die Shell, den bzw. die Sie für den Zugriff auf die Variablen verwenden, geschlossen und erneut geöffnet werden.
1. Erstellen Sie in Ihrer bevorzugten IDE ein neues Ruby-Projekt.
1. Fügen Sie den unten stehenden Code hinzu.
1. Führen Sie das Programm aus.

```ruby
# encoding: UTF-8

require 'net/https'
require 'uri'
require 'json'

key_var = "TEXT_ANALYTICS_SUBSCRIPTION_KEY"
if (!ENV[key_var])
    raise "Please set/export the following environment variable: " + key_var
else
    subscription_key = ENV[key_var]
end

endpoint_var = "TEXT_ANALYTICS_ENDPOINT"
if (!ENV[endpoint_var])
    raise "Please set/export the following environment variable: " + endpoint_var
else
    endpoint = ENV[endpoint_var]
end

path = '/text/analytics/v2.1/entities'

uri = URI(endpoint + path)

documents = { 'documents': [
    { 'id' => '1', 'language' => 'en', 'text' => 'Microsoft is an It company.' }
]}

puts 'Please wait a moment for the results to appear.'

request = Net::HTTP::Post.new(uri)
request['Content-Type'] = "application/json"
request['Ocp-Apim-Subscription-Key'] = subscription_key
request.body = documents.to_json

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

puts JSON::pretty_generate (JSON (response.body))
```

**Antwort der Entitätsextraktion**

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt: 

```json
{  
   "documents":[  
      {  
         "id":"1",
         "entities":[  
            {  
               "name":"Microsoft",
               "matches":[  
                  {  
                     "wikipediaScore":0.20872054383103444,
                     "entityTypeScore":0.99996185302734375,
                     "text":"Microsoft",
                     "offset":0,
                     "length":9
                  }
               ],
               "wikipediaLanguage":"en",
               "wikipediaId":"Microsoft",
               "wikipediaUrl":"https://en.wikipedia.org/wiki/Microsoft",
               "bingId":"a093e9b9-90f5-a3d5-c4b8-5855e1b01f85",
               "type":"Organization"
            },
            {  
               "name":"Technology company",
               "matches":[  
                  {  
                     "wikipediaScore":0.82123868042800585,
                     "text":"It company",
                     "offset":16,
                     "length":10
                  }
               ],
               "wikipediaLanguage":"en",
               "wikipediaId":"Technology company",
               "wikipediaUrl":"https://en.wikipedia.org/wiki/Technology_company",
               "bingId":"bc30426e-22ae-7a35-f24b-454722a47d8f"
            }
         ]
      }
   ],
    "errors":[]
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Textanalyse mit Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Weitere Informationen 

 [Übersicht über die Textanalyse](../overview.md)  
 [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)
