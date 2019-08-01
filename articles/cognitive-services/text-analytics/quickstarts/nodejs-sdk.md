---
title: 'Schnellstart: Verwenden von Node.js zum Aufrufen der Textanalyse-API'
titleSuffix: Azure Cognitive Services
description: Informationen und Codebeispiele für den schnellen Einstieg in die Verwendung der Textanalyse-API.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/30/2019
ms.author: shthowse
ms.openlocfilehash: 9b8a713d58d5753e04de050e0bc961b5e8388123
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697484"
---
# <a name="quickstart-using-nodejs-to-call-the-text-analytics-cognitive-service"></a>Schnellstart: Verwenden von Node.js zum Aufrufen der Textanalyse von Cognitive Services
<a name="HOLTop"></a>

Verwenden Sie diese Schnellstartanleitung, um mit dem Textanalyse SDK für Node.js in die Sprachanalyse einzusteigen. Die [Textanalyse](//go.microsoft.com/fwlink/?LinkID=759711)-REST-API ist zwar mit den meisten Programmiersprachen kompatibel, das SDK bietet aber eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/textAnalytics.js).

Die technische Dokumentation für die APIs finden Sie in den [API-Definitionen](//go.microsoft.com/fwlink/?LinkID=759346).

## <a name="prerequisites"></a>Voraussetzungen

* [Node.js](https://nodejs.org/)
* Das [Node.js SDK](https://www.npmjs.com/package/azure-cognitiveservices-textanalytics) für die Textanalyse. Sie können das SDK mit folgendem Befehl installieren:

    `npm install azure-cognitiveservices-textanalytics`

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

Außerdem benötigen Sie den [Endpunkt und den Zugriffsschlüssel](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) – beide wurden der Registrierung für Sie generiert.

## <a name="create-a-nodejs-application-and-install-the-sdk"></a>Erstellen einer Node.js-Anwendung und Installieren des SDK

Erstellen Sie nach der Installation von Node.js ein Knotenprojekt. Erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie zu diesem Verzeichnis.

```mkdir myapp && cd myapp```

Führen Sie ```npm init``` aus, um eine Knotenanwendung mit einer Datei des Typs „package.json“ zu erstellen. Installieren Sie die npm-Pakete `ms-rest-azure` und `azure-cognitiveservices-textanalytics`:

```npm install azure-cognitiveservices-textanalytics ms-rest-azure```

Die Datei „package.json“ Ihrer App wird mit den Abhängigkeiten aktualisiert.

## <a name="authenticate-your-credentials"></a>Authentifizieren Ihrer Anmeldeinformationen

Erstellen Sie im Projektstammverzeichnis die neue Datei `index.js`, und importieren Sie die installierten Bibliotheken.

```javascript
const CognitiveServicesCredentials = require("ms-rest-azure").CognitiveServicesCredentials;
const TextAnalyticsAPIClient = require("azure-cognitiveservices-textanalytics");
```

Erstellen Sie eine Variable für Ihren Abonnementschlüssel für Textanalyse.

```javascript
let credentials = new CognitiveServicesCredentials(
    "enter-your-key-here"
);
```

> [!Tip]
> Für eine sichere Bereitstellung von Geheimnissen in Produktionssystemen empfehlen wir die Verwendung von [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

## <a name="create-a-text-analytics-client"></a>Erstellen eines Textanalyseclients

Erstellen Sie ein neues `TextAnalyticsClient`-Objekt mit `credentials` als Parameter. Verwenden Sie die richtige Azure-Region für Ihr Textanalyseabonnement.

```javascript
//Replace 'westus' with the correct region for your Text Analytics subscription
let client = new TextAnalyticsAPIClient(
    credentials,
    "https://westus.api.cognitive.microsoft.com/"
);
```

## <a name="sentiment-analysis"></a>Stimmungsanalyse

Erstellen Sie eine Liste mit Objekten, die die zu analysierenden Dokumente enthält. Die Nutzlast an die API besteht aus einer Liste von `documents`, die ein `id`-, `language`- und `text`-Attribut enthält. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Dokuments, und `id` kann ein beliebiger Wert sein. 

```javascript
const inputDocuments = {documents:[
    {language:"en", id:"1", text:"I had the best day of my life."},
    {language:"en", id:"2", text:"This was a waste of my time. The speaker put me to sleep."},
    {language:"es", id:"3", text:"No tengo dinero ni nada que dar..."},
    {language:"it", id:"4", text:"L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."}
]}
```

Rufen Sie `client.sentiment` auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und drucken Sie die ID und den Stimmungswert jedes Dokuments. Eine gegen 0 tendierende Punktzahl deutet auf eine negative Stimmung hin, eine gegen 1 tendierende Punktzahl auf eine positive.

```javascript
const operation = client.sentiment({multiLanguageBatchInput: inputDocuments})
operation
.then(result => {
    console.log(result.documents);
})
.catch(err => {
    throw err;
});
```

Führen Sie in Ihrem Konsolenfenster Ihren Code mit `node index.js` aus.

### <a name="output"></a>Output

```console
[ { id: '1', score: 0.8723785877227783 },
  { id: '2', score: 0.1059873104095459 },
  { id: '3', score: 0.43635445833206177 },
  { id: '4', score: 1 } ]
```

## <a name="language-detection"></a>Spracherkennung

Erstellen Sie eine Liste mit Objekten, die Ihre Dokumente enthält. Die Nutzlast an die API besteht aus einer Liste von `documents`, die ein `id`- und `text`-Attribut enthält. Das `text`-Attribut speichert den Text, der analysiert werden soll, und die `id` kann ein beliebiger Wert sein.

```javascript
// The documents to be submitted for language detection. The ID can be any value.
const inputDocuments = {
    documents: [
        { id: "1", text: "This is a document written in English." },
        { id: "2", text: "Este es un document escrito en Español." },
        { id: "3", text: "这是一个用中文写的文件" }
    ]
    };
```

Rufen Sie `client.detectLanguage()` auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und drucken Sie die ID und die erste zurückgegebene Sprache.

```javascript
const operation = client.detectLanguage({
    languageBatchInput: inputDocuments
});
operation
    .then(result => {
    result.documents.forEach(document => {
        console.log(`ID: ${document.id}`);
        document.detectedLanguages.forEach(language =>
        console.log(`\tLanguage: ${language.name}`)
        );
    });
    })
    .catch(err => {
    throw err;
    });
```

Führen Sie in Ihrem Konsolenfenster Ihren Code mit `node index.js` aus.

### <a name="output"></a>Output

```console
===== LANGUAGE EXTRACTION ======
ID: 1 Language English
ID: 2 Language Spanish
ID: 3 Language Chinese_Simplified
```

## <a name="entity-recognition"></a>Entitätserkennung

Erstellen Sie eine Liste mit Objekten, die Ihre Dokumente enthält. Die Nutzlast an die API besteht aus einer Liste von `documents`, die ein `id`-, `language`- und `text`-Attribut enthält. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Dokuments, und `id` kann ein beliebiger Wert sein.

```javascript

    const inputDocuments = {documents:[
        {language:"en", id:"1", text:"Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800"},
        {language:"es", id:"2", text:"La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."},
        ]}

}
```

Rufen Sie `client.entities()` auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und drucken Sie die ID jedes Dokuments. Drucken Sie für jede erkannte Entität ihren Wikipedia-Namen, den Typ und Untertypen (falls vorhanden) sowie die Fundstellen im Originaltext.

```javascript
const operation = client.entities({
    multiLanguageBatchInput: inputDocuments
});
operation
    .then(result => {
    result.documents.forEach(document => {
        console.log(`Document ID: ${document.id}`)
        document.entities.forEach(e =>{
        console.log(`\tName: ${e.name} Type: ${e.type} Sub Type: ${e.type}`)
        e.matches.forEach(match => (
            console.log(`\t\tOffset: ${match.offset} Length: ${match.length} Score: ${match.entityTypeScore}`)
        ))
        })
    });
    })
    .catch(err => {
    throw err;
    });
```

Führen Sie in Ihrem Konsolenfenster Ihren Code mit `node index.js` aus.

### <a name="output"></a>Output

```console
Document ID: 1
    Name: Microsoft Type: Organization Sub Type: Organization
            Offset: 0 Length: 9 Score: 1
    Name: Bill Gates Type: Person Sub Type: Person
            Offset: 25 Length: 10 Score: 0.999786376953125
    Name: Paul Allen Type: Person Sub Type: Person
            Offset: 40 Length: 10 Score: 0.9988105297088623
    Name: April 4 Type: Other Sub Type: Other
            Offset: 54 Length: 7 Score: 0.8
    Name: April 4, 1975 Type: DateTime Sub Type: DateTime
            Offset: 54 Length: 13 Score: 0.8
    Name: BASIC Type: Other Sub Type: Other
            Offset: 89 Length: 5 Score: 0.8
    Name: Altair 8800 Type: Other Sub Type: Other
            Offset: 116 Length: 11 Score: 0.8
Document ID: 2
    Name: Microsoft Type: Organization Sub Type: Organization
            Offset: 21 Length: 9 Score: 0.999755859375
    Name: Redmond (Washington) Type: Location Sub Type: Location
            Offset: 60 Length: 7 Score: 0.9911284446716309
    Name: 21 kilómetros Type: Quantity Sub Type: Quantity
            Offset: 71 Length: 13 Score: 0.8
    Name: Seattle Type: Location Sub Type: Location
            Offset: 88 Length: 7 Score: 0.9998779296875
```

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Erstellen Sie eine Liste mit Objekten, die Ihre Dokumente enthält. Die Nutzlast an die API besteht aus einer Liste von `documents`, die ein `id`-, `language`- und `text`-Attribut enthält. Das `text`-Attribut speichert den Text, der analysiert werden soll, `language` ist die Sprache des Dokuments, und `id` kann ein beliebiger Wert sein.

```javascript
    let inputLanguage = {
    documents: [
        {language:"ja", id:"1", text:"猫は幸せ"},
        {language:"de", id:"2", text:"Fahrt nach Stuttgart und dann zum Hotel zu Fu."},
        {language:"en", id:"3", text:"My cat might need to see a veterinarian."},
        {language:"es", id:"4", text:"A mi me encanta el fútbol!"}
    ]
    };
```

Rufen Sie `client.keyPhrases()` auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und geben Sie die ID und alle erkannten Schlüsselbegriffe aus.

```javascript
    let operation = client.keyPhrases({
    multiLanguageBatchInput: inputLanguage
    });
    operation
    .then(result => {
        console.log(result.documents);
    })
    .catch(err => {
        throw err;
    });
```

Führen Sie in Ihrem Konsolenfenster Ihren Code mit `node index.js` aus.

### <a name="output"></a>Output

```console
[ 
    { id: '1', keyPhrases: [ '幸せ' ] },
    { id: '2', keyPhrases: [ 'Stuttgart', 'Hotel', 'Fahrt', 'Fu' ] },
    { id: '3', keyPhrases: [ 'cat', 'veterinarian' ] },
    { id: '4', keyPhrases: [ 'fútbol' ] } 
]
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Textanalyse mit Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Weitere Informationen

 [Übersicht über die Textanalyse](../overview.md) [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)
