---
title: 'Schnellstart: Übersetzen von Text, Node.js: Textübersetzungs-API'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart übersetzen Sie Text von einer Sprache in eine andere. Dazu verwenden Sie die Textübersetzungs-API mit Node.js.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 06a58bc3d9b6383dd1c4cb6c33c8b24d62d5c851
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705486"
---
# <a name="quickstart-use-the-translator-text-api-to-translate-a-string-with-nodejs"></a>Schnellstart: Verwenden der Textübersetzungs-API zum Übersetzen einer Zeichenfolge mit Node.js

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Node.js und der Textübersetzungs-REST-API eine Textzeichenfolge vom Englischen ins Italienische und Deutsche übersetzen.

Für diese Schnellstartanleitung wird ein [Azure Cognitive Services-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit einer Textübersetzungsressource benötigt. Wenn Sie über kein Konto verfügen, können Sie über die [kostenlose Testversion](https://azure.microsoft.com/try/cognitive-services/) einen Abonnementschlüssel abrufen.

>[!TIP]
> Wenn Sie den gesamten Code sehen möchten: Der Quellcode für dieses Beispiel steht auf [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS) zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* [Node 8.12.x oder höher](https://nodejs.org/en/)
* Ein Azure-Abonnementschlüssel für die Textübersetzung

## <a name="create-a-project-and-import-required-modules"></a>Erstellen eines Projekts und Importieren der erforderlichen Module

Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Projekt, oder erstellen Sie einen neuen Ordner mit der Datei `translate-text.js` auf dem Desktop. Kopieren Sie dann den folgenden Codeausschnitt in Ihr Projekt bzw. Ihre Datei:

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> Wenn Sie diese Module bisher nicht verwendet haben, müssen Sie sie vor der Ausführung Ihres Programms installieren. Führen Sie zum Installieren dieser Pakete `npm install request uuidv4` aus.

Diese Module sind zum Erstellen der HTTP-Anforderung und eines eindeutigen Bezeichners für den `'X-ClientTraceId'`-Header erforderlich.

## <a name="set-the-subscription-key"></a>Festlegen des Abonnementschlüssels

Dieser Code liest den Textübersetzungs-Abonnementschlüssel aus der Umgebungsvariablen `TRANSLATOR_TEXT_KEY`. Wenn Sie mit Umgebungsvariablen nicht vertraut sind, können Sie `subscriptionKey` als Zeichenfolge festlegen und die Bedingungsanweisung auskommentieren.

Kopieren Sie diesen Code in Ihr Projekt:

```javascript
/* Checks to see if the subscription key is available
as an environment variable. If you are setting your subscription key as a
string, then comment these lines out.

If you want to set your subscription key as a string, replace the value for
the Ocp-Apim-Subscription-Key header as a string. */
const subscriptionKey = process.env.TRANSLATOR_TEXT_KEY;
if (!subscriptionKey) {
  throw new Error('Environment variable for your subscription key is not set.')
};
```

## <a name="configure-the-request"></a>Konfigurieren der Anforderung

Mit der über das Anforderungsmodul zur Verfügung gestellten `request()`-Methode können Sie die HTTP-Methode, die URL, Anforderungsparameter und den JSON-Text als `options`-Objekt übergeben. In diesem Codeausschnitt wird die Anforderung konfiguriert:

>[!NOTE]
> Weitere Informationen zu Endpunkten, Routen und Anforderungsparametern finden Sie unter [Textübersetzungs-API 3.0: Translate](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate).

```javascript
let options = {
    method: 'POST',
    baseUrl: 'https://api.cognitive.microsofttranslator.com/',
    url: 'translate',
    qs: {
      'api-version': '3.0',
      'to': ['de', 'it']
    },
    headers: {
      'Ocp-Apim-Subscription-Key': subscriptionKey,
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    body: [{
          'text': 'Hello World!'
    }],
    json: true,
};
```

Eine Anforderung lässt sich am einfachsten authentifizieren, indem Sie den Abonnementschlüssel als `Ocp-Apim-Subscription-Key`-Header übergeben. Diese Vorgehensweise wird in diesem Beispiel verwendet. Alternativ können Sie den Abonnementschlüssel durch ein Zugriffstoken ersetzen und dieses als `Authorization`-Header übergeben, um Ihre Anforderung zu überprüfen.

Wenn Sie ein Cognitive Services-Abonnement mehrerer Dienste verwenden, müssen Sie auch `Ocp-Apim-Subscription-Region` in Ihre Anforderungsheader aufnehmen.

Weitere Informationen finden Sie unter [Authentifizierung](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="make-the-request-and-print-the-response"></a>Senden der Anforderung und Ausgeben der Antwort

Erstellen Sie als Nächstes mithilfe der `request()`-Methode die Anforderung. Sie verwendet das im vorherigen Abschnitt erstellte `options`-Objekt als erstes Argument und gibt dann die übersichtlicher gestaltete JSON-Antwort aus.

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> In diesem Beispiel wird die HTTP-Anforderung im `options`-Objekt definiert. Das Anforderungsmodul unterstützt außerdem praktische Methoden wie `.post` und `.get`. Weitere Informationen finden Sie unter [Convenience methods](https://github.com/request/request#convenience-methods) (Praktische Methoden).

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Das war's: Sie haben ein einfaches Programm erstellt, das die Textübersetzungs-API aufruft und eine JSON-Antwort zurückgibt. Führen Sie das Programm jetzt aus:

```console
node translate-text.js
```

[Auf GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS) finden Sie das vollständige Beispiel, falls Sie Ihren Code mit unserem Code vergleichen möchten.

## <a name="sample-response"></a>Beispiel für eine Antwort

```json
[
    {
        "detectedLanguage": {
            "language": "en",
            "score": 1.0
        },
        "translations": [
            {
                "text": "Hallo Welt!",
                "to": "de"
            },
            {
                "text": "Salve, mondo!",
                "to": "it"
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Abonnementschlüssel in Ihrem Programm hartcodiert haben, entfernen Sie ihn unbedingt, wenn Sie diese Schnellstartanleitung abgeschlossen haben.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die API-Referenz an, um zu erfahren, welche Möglichkeiten die Textübersetzungs-API bietet.

> [!div class="nextstepaction"]
> [API-Referenz](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

## <a name="see-also"></a>Weitere Informationen

Sie können die Textübersetzungs-API nicht nur für die Spracherkennung, sondern auch für Folgendes verwenden:

* [Transliteration von Text](quickstart-nodejs-transliterate.md)
* [Identifizieren der Sprache anhand der Eingabe](quickstart-nodejs-detect.md)
* [Ermitteln alternativer Übersetzungen](quickstart-nodejs-dictionary.md)
* [Abrufen einer Liste der unterstützten Sprachen](quickstart-nodejs-languages.md)
* [Bestimmen der Satzlänge aus einer Eingabe](quickstart-nodejs-sentences.md)
