---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 10/02/2019
ms.author: aahi
ms.openlocfilehash: 847b2d0489dc04b4275465dbe957b72418bbf1a4
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73750172"
---
[Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-ruby/tree/master/data/azure_cognitiveservices_textanalytics) | [Paket (RubyGems)](https://rubygems.org/gems/azure_cognitiveservices_textanalytics) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-quickstart-code)

<a name="HOLTop"></a>

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement: [Kostenloses Azure-Konto](https://azure.microsoft.com/free/)
* Die aktuelle Version von [Ruby](https://www.ruby-lang.org/)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-text-analytics-azure-resource"></a>Erstellen einer Azure-Ressource für die Textanalyse 

[!INCLUDE [text-analytics-resource-creation](resource-creation.md)]

### <a name="create-a-new-ruby-application"></a>Erstellen einer neuen Ruby-Anwendung

Erstellen Sie in einem Konsolenfenster (etwa cmd, PowerShell oder Bash) ein neues Verzeichnis für Ihre App, und rufen Sie es auf. Erstellen Sie dann eine Datei mit dem Namen `GemFile` und eine Ruby-Datei für Ihren Code.

```console
mkdir myapp && cd myapp
```

Fügen Sie in Ihrer `GemFile` die folgenden Zeilen hinzu, um die Clientbibliothek als Abhängigkeit hinzuzufügen.

```ruby
source 'https://rubygems.org'
gem 'azure_cognitiveservices_textanalytics', '~>0.17.3'
```

Importieren Sie in Ihrer Ruby-Datei die folgenden Pakete.

[!code-ruby[Import statements](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=includeStatement)]

Erstellen Sie Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource namens `TEXT_ANALYTICS_ENDPOINT` und `TEXT_ANALYTICS_SUBSCRIPTION_KEY`. Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellt haben, müssen der Editor, die IDE oder die Shell, in dem bzw. der sie ausgeführt wird, geschlossen und erneut geöffnet werden, damit der Zugriff auf die Variable möglich ist. 

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]


[!code-ruby[endpoint, key variables](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=vars)]

## <a name="object-model"></a>Objektmodell 

Der Textanalyse-Client führt die Authentifizierung für Azure mit Ihrem Schlüssel durch. Der Client verfügt über verschiedene Methoden zum Analysieren von Text als einzelne Zeichenfolge oder als Batch. 

Text wird als `documents`-Liste an die API gesendet. Hierbei handelt es sich um `dictionary`-Objekte, die je nach verwendeter Methode eine Kombination aus den Attributen `id`, `text` und `language` enthalten. Das `text`-Attribut speichert den zu analysierenden Text in der Ausgangssprache (`language`), und `id` kann ein beliebiger Wert sein. 

Das Antwortobjekt ist eine Liste mit den Analyseinformationen für jedes Dokument. 

## <a name="code-examples"></a>Codebeispiele

Mit den Codeausschnitten wird veranschaulicht, wie die folgenden Vorgänge mit der Textanalyse-Clientbibliothek für Python durchgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Standpunktanalyse](#sentiment-analysis)
* [Sprachenerkennung](#language-detection)
* [Entitätserkennung](#entity-recognition)
* [Schlüsselbegriffserkennung](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Erstellen Sie eine Klasse namens `TextAnalyticsClient`. 

```ruby
class TextAnalyticsClient
  @textAnalyticsClient
  #...
end
```

Erstellen Sie in dieser Klasse eine Funktion namens `initialize`, um den Client zu authentifizieren. Verwenden Sie Ihre Umgebungsvariablen `TEXT_ANALYTICS_SUBSCRIPTION_KEY` und `TEXT_ANALYTICS_ENDPOINT`. 

[!code-ruby[initialize function for authentication](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=initialize)]

Außerhalb der Klasse verwenden Sie die Funktion `new()` des Clients, um ihn zu instanziieren.

[!code-ruby[client creation](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=clientCreation)] 

<a name="SentimentAnalysis"></a>

## <a name="sentiment-analysis"></a>Stimmungsanalyse

Erstellen Sie im Clientobjekt eine Funktion namens `AnalyzeSentiment()`, die eine Liste von Eingabedokumenten übernimmt, die später erstellt werden. Rufen Sie die `sentiment()`-Funktion des Clients auf, und rufen Sie das Ergebnis ab. Durchlaufen Sie dann die Ergebnisse, und drucken Sie die ID und den Stimmungswert jedes Dokuments. Eine gegen 0 tendierende Punktzahl deutet auf eine negative Stimmung hin, eine gegen 1 tendierende Punktzahl auf eine positive.

[!code-ruby[client method for sentiment analysis](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=analyzeSentiment)] 

Erstellen Sie außerhalb der Clientfunktion eine neue Funktion namens `SentimentAnalysisExample()`, die das zuvor erstellte `TextAnalyticsClient`-Objekt übernimmt. Erstellen Sie eine Liste mit `MultiLanguageInput`-Objekten, die die zu analysierenden Dokumente enthält. Jedes Objekt enthält die Attribute `id`, `Language` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Dokuments, und `id` kann ein beliebiger Wert sein. Rufen Sie dann die `AnalyzeSentiment()`-Funktion des Clients auf.

[!code-ruby[sentiment analysis document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=sentimentCall)] 

Rufen Sie die Funktion `SentimentAnalysisExample()` auf.

```ruby
SentimentAnalysisExample(textAnalyticsClient)
```

### <a name="output"></a>Output

```console
===== SENTIMENT ANALYSIS =====
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

<a name="LanguageDetection"></a>

## <a name="language-detection"></a>Spracherkennung

Erstellen Sie im Clientobjekt eine Funktion namens `DetectLanguage()`, die eine Liste von Eingabedokumenten übernimmt, die später erstellt werden. Rufen Sie die `detect_language()`-Funktion des Clients auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die ermittelte Sprache aus.

[!code-ruby[client method for language detection](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=detectLanguage)] 

Erstellen Sie außerhalb der Clientfunktion eine neue Funktion namens `DetectLanguageExample()`, die das zuvor erstellte `TextAnalyticsClient`-Objekt übernimmt. Erstellen Sie eine Liste mit `LanguageInput`-Objekten, die die zu analysierenden Dokumente enthält. Jedes Objekt enthält die Attribute `id` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, und die `id` kann ein beliebiger Wert sein. Rufen Sie dann die `DetectLanguage()`-Funktion des Clients auf.

[!code-ruby[language detection document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=detectLanguageCall)] 

Rufen Sie die Funktion `DetectLanguageExample()` auf.

```ruby
DetectLanguageExample(textAnalyticsClient)
```

### <a name="output"></a>Output

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

<a name="EntityRecognition"></a>

## <a name="entity-recognition"></a>Entitätserkennung

Erstellen Sie im Clientobjekt eine Funktion namens `RecognizeEntities()`, die eine Liste von Eingabedokumenten übernimmt, die später erstellt werden. Rufen Sie die `entities()`-Funktion des Clients auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die erkannten Entitäten aus.

[!code-ruby[client method for entity recognition](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=recognizeEntities)]

Erstellen Sie außerhalb der Clientfunktion eine neue Funktion namens `RecognizeEntitiesExample()`, die das zuvor erstellte `TextAnalyticsClient`-Objekt übernimmt. Erstellen Sie eine Liste mit `MultiLanguageInput`-Objekten, die die zu analysierenden Dokumente enthält. Jedes Objekt enthält die Attribute `id`, `language` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Texts, und `id` kann ein beliebiger Wert sein. Rufen Sie dann die `RecognizeEntities()`-Funktion des Clients auf.

[!code-ruby[entity recognition documents and method call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=recognizeEntitiesCall)] 

Rufen Sie die Funktion `RecognizeEntitiesExample()` auf.

```ruby
RecognizeEntitiesExample(textAnalyticsClient)
```

### <a name="output"></a>Output

```console
===== ENTITY RECOGNITION =====
Document ID: 1
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 0, Length: 9,   Score: 1.0

        Name: Bill Gates,       Type: Person,   Sub-Type: N/A
        Offset: 25, Length: 10, Score: 0.999847412109375

        Name: Paul Allen,       Type: Person,   Sub-Type: N/A
        Offset: 40, Length: 10, Score: 0.9988409876823425

        Name: April 4,  Type: Other,    Sub-Type: N/A
        Offset: 54, Length: 7,  Score: 0.8

        Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
        Offset: 54, Length: 13, Score: 0.8

        Name: BASIC,    Type: Other,    Sub-Type: N/A
        Offset: 89, Length: 5,  Score: 0.8

        Name: Altair 8800,      Type: Other,    Sub-Type: N/A
        Offset: 116, Length: 11,        Score: 0.8

Document ID: 2
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 21, Length: 9,  Score: 0.999755859375

        Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
        Offset: 60, Length: 7,  Score: 0.9911284446716309

        Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
        Offset: 71, Length: 13, Score: 0.8

        Name: Seattle,  Type: Location, Sub-Type: N/A
        Offset: 88, Length: 7,  Score: 0.9998779296875
```

<a name="KeyPhraseExtraction"></a>

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Erstellen Sie im Clientobjekt eine Funktion namens `ExtractKeyPhrases()`, die eine Liste von Eingabedokumenten übernimmt, die später erstellt werden. Rufen Sie die `key_phrases()`-Funktion des Clients auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die extrahierten Schlüsselbegriffe aus.

[!code-ruby[key phrase extraction client method](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=extractKeyPhrases)] 

Erstellen Sie außerhalb der Clientfunktion eine neue Funktion namens `KeyPhraseExtractionExample()`, die das zuvor erstellte `TextAnalyticsClient`-Objekt übernimmt. Erstellen Sie eine Liste mit `MultiLanguageInput`-Objekten, die die zu analysierenden Dokumente enthält. Jedes Objekt enthält die Attribute `id`, `language` und `text`. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Texts, und `id` kann ein beliebiger Wert sein. Rufen Sie dann die `ExtractKeyPhrases()`-Funktion des Clients auf.

[!code-ruby[key phrase document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=keyPhrasesCall)]


Rufen Sie die Funktion `KeyPhraseExtractionExample()` auf.

```ruby
KeyPhraseExtractionExample(textAnalyticsClient)
```

### <a name="output"></a>Output

```console
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
                Fahrt
                Fu
Document ID: 3
         Key phrases:
                cat
                rock
Document ID: 4
         Key phrases:
                fútbol
```
