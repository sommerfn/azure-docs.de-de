---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 10/28/2019
ms.author: aahi
ms.openlocfilehash: 5c5c2ed6b7806095dada40cc921a773d28421424
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73750156"
---
<a name="HOLTop"></a>

[Referenzdokumentation](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/cognitiveServicesTextAnalytics) | [Paket (NPM)](https://www.npmjs.com/package/azure-cognitiveservices-textanalytics) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Die aktuelle Version von [Node.js](https://nodejs.org/).

## <a name="setting-up"></a>Einrichten

### <a name="create-a-text-analytics-azure-resource"></a>Erstellen einer Azure-Ressource für die Textanalyse

[!INCLUDE [text-analytics-resource-creation](resource-creation.md)]

### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Erstellen Sie in einem Konsolenfenster (etwa cmd, PowerShell oder Bash) ein neues Verzeichnis für Ihre App, und rufen Sie es auf. 

```console
mkdir myapp && cd myapp
```

Führen Sie den Befehl `npm init` aus, um eine Knotenanwendung mit der Datei `package.json` zu erstellen. 

```console
npm init
```

Erstellen Sie eine Datei namens `index.js`, und fügen Sie die folgenden Bibliotheken hinzu:

[!code-javascript[Const statements](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=constStatements)]

Erstellen Sie Variablen für den Azure-Endpunkt und den Abonnementschlüssel Ihrer Ressource. Rufen Sie diese Werte aus den Umgebungsvariablen `TEXT_ANALYTICS_SUBSCRIPTION_KEY` und `TEXT_ANALYTICS_ENDPOINT` ab. Wenn Sie diese Umgebungsvariablen erstellt haben, nachdem Sie mit der Bearbeitung der Anwendung begonnen haben, muss der Editor, die IDE oder die Shell, den bzw. die Sie für den Zugriff auf die Variablen verwenden, geschlossen und erneut geöffnet werden.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

[!code-javascript[Key and endpoint vars](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=keyVars)]

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Installieren Sie die npm-Pakete `@azure/ms-rest-js` und `@azure/cognitiveservices-textanalytics`:

```console
npm install @azure/cognitiveservices-textanalytics @azure/ms-rest-js
```

Die Datei `package.json` Ihrer App wird mit den Abhängigkeiten aktualisiert.

## <a name="object-model"></a>Objektmodell

Der Textanalyse-Client ist ein [TextAnalyticsClient](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/textanalyticsclient?view=azure-node-latest)-Objekt, das sich mit Ihrem Schlüssel bei Azure authentifiziert. Der Client verfügt über verschiedene Methoden zum Analysieren von Text als einzelne Zeichenfolge oder als Batch.

Text wird als `documents`-Liste an die API gesendet. Hierbei handelt es sich um `dictionary`-Objekte, die je nach verwendeter Methode eine Kombination aus den Attributen `id`, `text` und `language` enthalten. Das `text`-Attribut speichert den zu analysierenden Text in der Ausgangssprache (`language`), und `id` kann ein beliebiger Wert sein. 

Das Antwortobjekt ist eine Liste mit den Analyseinformationen für jedes Dokument. 

## <a name="code-examples"></a>Codebeispiele

* [Authentifizieren des Clients](#authenticate-the-client)
* [Standpunktanalyse](#sentiment-analysis)
* [Sprachenerkennung](#language-detection)
* [Entitätserkennung](#entity-recognition)
* [Schlüsselbegriffserkennung](#key-phrase-extraction)


## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Erstellen Sie ein [TextAnalyticsClient](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/textanalyticsclient?view=azure-node-latest)-Objekt mit `credentials` und `endpoint` als Parameter.

[!code-javascript[Authentication and client creation](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=authentication)]


## <a name="sentiment-analysis"></a>Stimmungsanalyse

Erstellen Sie eine Liste von Wörterbuchobjekten, die die Dokumente enthalten, die Sie analysieren möchten. Rufen Sie die [Sentiment()](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/textanalyticsclient?view=azure-node-latest#sentiment-object-)-Methode des Clients auf, und rufen Sie das zurückgegebene [SentimentBatchResult](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/sentimentbatchresult?view=azure-node-latest) ab. Durchlaufen Sie die Liste der Ergebnisse, und drucken Sie die ID und den Stimmungswert jedes Dokuments. Eine gegen 0 tendierende Punktzahl deutet auf eine negative Stimmung hin, eine gegen 1 tendierende Punktzahl auf eine positive.

[!code-javascript[Sentiment analysis](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=sentimentAnalysis)]

Führen Sie in Ihrem Konsolenfenster Ihren Code mit `node index.js` aus.

### <a name="output"></a>Output

```console
[ { id: '1', score: 0.87 } ]
[ { id: '2', score: 0.11 } ]
[ { id: '3', score: 0.44 } ]
[ { id: '4', score: 1.00 } ]
```

## <a name="language-detection"></a>Spracherkennung

Erstellen Sie eine Liste mit Wörterbuchobjekten, die Ihre Dokumente enthalten. Rufen Sie die [detectLanguage()](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/textanalyticsclient?view=azure-node-latest#detectlanguage-object-)-Methode des Clients auf, und rufen Sie das zurückgegebene [LanguageBatchResult](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/languagebatchresult?view=azure-node-latest) ab. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die Sprache aus.

[!code-javascript[Language detection](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=languageDetection)]

Führen Sie in Ihrem Konsolenfenster Ihren Code mit `node index.js` aus.

### <a name="output"></a>Output

```console
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

## <a name="entity-recognition"></a>Entitätserkennung

Erstellen Sie eine Liste mit Objekten, die Ihre Dokumente enthält. Rufen Sie die [entities()](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/textanalyticsclient?view=azure-node-latest#entities-object-)-Methode des Clients auf, und rufen Sie das [EntitiesBatchResult](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/entitiesbatchresult?view=azure-node-latest)-Objekt ab. Durchlaufen Sie die Liste der Ergebnisse, und geben Sie die ID jedes Dokuments aus. Drucken Sie für jede erkannte Entität ihren Wikipedia-Namen, den Typ und Untertypen (falls vorhanden) sowie die Fundstellen im Originaltext.

[!code-javascript[Entity recognition](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=entityRecognition)]

Führen Sie in Ihrem Konsolenfenster Ihren Code mit `node index.js` aus.

### <a name="output"></a>Output

```console
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

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Erstellen Sie eine Liste mit Objekten, die Ihre Dokumente enthält. Rufen Sie die [keyPhrases()](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/textanalyticsclient?view=azure-node-latest#keyphrases-object-)-Methode des Clients auf, und rufen Sie das zurückgegebene [KeyPhraseBatchResult](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/keyphrasebatchresult?view=azure-node-latest)-Objekt ab. Durchlaufen Sie die Ergebnisse, und geben Sie die ID und alle erkannten Schlüsselbegriffe aus.

[!code-javascript[Key phrase extraction](~/cognitive-services-node-sdk-samples/Samples/textAnalytics.js?name=keyPhraseExtraction)]

Führen Sie in Ihrem Konsolenfenster Ihren Code mit `node index.js` aus.

### <a name="output"></a>Output

```console
[
    { id: '1', keyPhrases: [ '幸せ' ] }
    { id: '2', keyPhrases: [ 'Stuttgart', "hotel", "Fahrt", "Fu" ] }
    { id: '3', keyPhrases: [ 'cat', 'veterinarian' ] }
    { id: '3', keyPhrases: [ 'fútbol' ] }
]
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `node` für die Schnellstartdatei aus.

```console
node index.js
```
