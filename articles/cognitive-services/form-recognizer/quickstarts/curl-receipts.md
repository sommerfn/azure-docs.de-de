---
title: 'Schnellstart: Extrahieren von Verkaufsbelegdaten mithilfe von cURL – Formularerkennung'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die REST-API für die Formularerkennung mit cURL, um Daten aus Bildern von Verkaufsbelegen zu extrahieren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 07/01/2019
ms.author: pafarley
ms.openlocfilehash: c533949cf0ce69ddc5237dd893dd75e43447c4a9
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72931580"
---
# <a name="quickstart-extract-receipt-data-using-the-form-recognizer-rest-api-with-curl"></a>Schnellstart: Extrahieren von Verkaufsbelegdaten mithilfe der REST-API für die Formularerkennung mit cURL

In diesem Schnellstart verwenden Sie die REST-API der Azure-Formularerkennung mit cURL, um relevante Informationen in Verkaufsbelegen zu extrahieren und zu identifizieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
Für diesen Schnellstart benötigen Sie Folgendes:
- Zugriff auf die Vorschauversion der Formularerkennung mit eingeschränktem Zugriff. Um Zugriff auf die Vorschauversion zu erhalten, füllen Sie das [Formular zum Anfordern des Zugriffs auf die Formularerkennung](https://aka.ms/FormRecognizerRequestAccess) aus, und übermitteln Sie es.
- [cURL](https://curl.haxx.se/windows/) muss installiert sein.
- Eine URL für ein Bild eines Verkaufsbelegs. Für diesen Schnellstart können Sie ein [Beispielbild](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/contoso-receipt.png?raw=true) verwenden.

## <a name="create-a-form-recognizer-resource"></a>Erstellen einer Formularerkennungsressource

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="analyze-a-receipt"></a>Analysieren eines Verkaufsbelegs

Um mit der Analyse eines Verkaufsbelegs zu beginnen, rufen Sie die **Analyze Receipt**-API mit dem folgenden cURL-Befehl auf. Nehmen Sie die folgenden Änderungen vor, bevor Sie den Befehl ausführen:

1. Ersetzen Sie `<Endpoint>` durch den Endpunkt, den Sie mit Ihrem Abonnement für die Formularerkennung erhalten haben.
1. Ersetzen Sie `<your receipt URL>` mit der URL-Adresse eines Verkaufsbelegbilds.
1. Ersetzen Sie `<subscription key>` durch den Abonnementschlüssel, den Sie im vorherigen Schritt kopiert haben.

```bash
curl -i -X POST "https://<Endpoint>/formrecognizer/v1.0-preview/prebuilt/receipt/asyncBatchAnalyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"url\": \"<your receipt URL>\"}"
```

Sie erhalten die Antwort `202 (Success)` mit einem **Operation-Location**-Header. Der Wert dieses Headers enthält eine Vorgangs-ID, mit der Sie den Status des Vorgangs abfragen und die Ergebnisse abrufen können. Im folgenden Beispiel ist die Zeichenfolge nach `operations/` die Vorgangs-ID.

```console
https://cognitiveservice/formrecognizer/v1.0-preview/prebuilt/receipt/operations/54f0b076-4e38-43e5-81bd-b85b8835fdfb
```

## <a name="get-the-receipt-results"></a>Abrufen der Verkaufsbelegergebnisse

Nachdem Sie die **Analyze Receipt**-API aufgerufen haben, rufen Sie die **Get Receipt Result**-API auf, um den Status des Vorgangs und die extrahierten Daten abzurufen.

1. Ersetzen Sie `<operationId>` durch die Vorgangs-ID aus dem vorherigen Schritt.
1. Ersetzen Sie `<subscription key>` durch Ihren Abonnementschlüssel.

```bash
curl -X GET "https://<Endpoint>/formrecognizer/v1.0-preview/prebuilt/receipt/operations/<operationId>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

### <a name="examine-the-response"></a>Untersuchen der Antwort

Sie erhalten die Antwort `200 (Success)` mit der folgenden JSON-Ausgabe: Das erste Feld, `"status"`, gibt den Status des Vorgangs an. Wenn der Vorgang abgeschlossen ist, enthält das Feld `"recognitionResults"` jede Textzeile, die aus dem Beleg extrahiert wurde, und das Feld `"understandingResults"` enthält Schlüssel-/Wert-Informationen für die wichtigsten Teile des Belegs. Wenn der Vorgang nicht abgeschlossen ist, ist der Wert von `"status"` `"Running"` oder `"NotStarted"`, und Sie sollten die API Aufrufen entweder manuell oder über ein Skript erneut aufrufen. Ein Intervall von mindestens einer Sekunde zwischen den Aufrufen wird empfohlen.

Vergleichen Sie das folgende Bild des Belegs mit der entsprechenden JSON-Ausgabe. Die Ausgabe wurde zur besseren Lesbarkeit gekürzt.

![Ein Verkaufsbeleg vom Contoso-Laden](../media/contoso-receipt.png)

```json
{
  "status": "Succeeded",
  "recognitionResults": [{
    "page": 1,
    "clockwiseOrientation": 0.36,
    "width": 1688,
    "height": 3000,
    "unit": "pixel",
    "lines": [{
      "boundingBox": [616, 291, 1050, 278, 1053, 384, 620, 397],
      "text": "Contoso",
      "words": [{
        "boundingBox": [619, 292, 1051, 284, 1052, 382, 620, 398],
        "text": "Contoso"
      }]
    }, {
      "boundingBox": [322, 588, 501, 600, 497, 655, 318, 643],
      "text": "Contoso",
      "words": [{
        "boundingBox": [330, 590, 501, 602, 499, 654, 326, 644],
        "text": "Contoso"
      }]
    },
    ...
    ]
  }],
  "understandingResults": [{
    "pages": [1],
    "fields": {
      "Subtotal": {
        "valueType": "numberValue",
        "value": 1098.99,
        "text": "1098.99",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/14/words/1"
        }]
      },
      "Total": {
        "valueType": "numberValue",
        "value": 1203.39,
        "text": "1203.39",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/18/words/1"
        }]
      },
      "Tax": {
        "valueType": "numberValue",
        "value": 104.4,
        "text": "$104.40",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/16/words/0"
        }, {
          "$ref": "#/recognitionResults/0/lines/16/words/1"
        }]
      },
      "MerchantAddress": {
        "valueType": "stringValue",
        "value": "123 Main Street Redmond, WA 98052",
        "text": "123 Main Street Redmond, WA 98052",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/2/words/0"
        }, {
          "$ref": "#/recognitionResults/0/lines/2/words/1"
        }, {
          "$ref": "#/recognitionResults/0/lines/2/words/2"
        }, {
          "$ref": "#/recognitionResults/0/lines/3/words/0"
        }, {
          "$ref": "#/recognitionResults/0/lines/3/words/1"
        }, {
          "$ref": "#/recognitionResults/0/lines/3/words/2"
        }]
      },
      "MerchantName": {
        "valueType": "stringValue",
        "value": "Contoso",
        "text": "Contoso",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/1/words/0"
        }]
      },
      "MerchantPhoneNumber": {
        "valueType": "stringValue",
        "value": null,
        "text": "123-456-7890",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/4/words/0"
        }]
      },
      "TransactionDate": {
        "valueType": "stringValue",
        "value": "2019-06-10",
        "text": "6/10/2019",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/5/words/0"
        }]
      },
      "TransactionTime": {
        "valueType": "stringValue",
        "value": "13:59:00",
        "text": "13:59",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/5/words/1"
        }]
      }
    }
  }]
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie die REST-API für die Formularerkennung mit cURL verwendet, um den Inhalt eines Verkaufsbelegs zu extrahieren. Lesen Sie als Nächstes die Referenzdokumentation, um die Formularerkennungs-API eingehender zu erkunden.

> [!div class="nextstepaction"]
> [Referenzdokumentation zur Rest-API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeReceipt)
