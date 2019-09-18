---
title: 'Beispiel: Aufrufen der API für die Bildanalyse – Maschinelles Sehen-API'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mithilfe von REST in Azure Cognitive Services die Maschinelles Sehen-API aufrufen.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 09/09/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 386503a7089c910b52a87cca8d9f2f2203ae0cad
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70859054"
---
# <a name="example-how-to-call-the-computer-vision-api"></a>Beispiel: Aufrufen der Maschinelles Sehen-API

Diese Anleitung veranschaulicht, wie Sie die Maschinelles Sehen-API mithilfe von REST aufrufen. Die Beispiele wurden in C# sowohl mithilfe der Clientbibliothek der Maschinelles Sehen-API als auch als HTTP POST/GET-Aufrufe geschrieben. Wir konzentrieren uns auf folgende Themen:

- Abrufen von „Tags“, „Description“ und „Category“
- Abrufen von „domänenspezifischen“ Informationen (Prominente)

## <a name="prerequisites"></a>Voraussetzungen

- Bild-URL oder Pfad zum lokal gespeicherten Bild.
- Unterstützte Eingabemethoden: Unformatierte Bildbinärdaten in Form einer Anwendung/eines Oktettdatenstroms oder einer Bild-URL
- Unterstützte Bildformate: JPEG, PNG, GIF, BMP
- Bilddateigröße: Weniger als 4 MB
- Bildgröße: Mindestens 50 x 50 Pixel
  
Im Beispiel unten werden die folgenden Funktionen demonstriert:

1. Analysieren eines Bildes und Rückgabe eines Arrays an Tags und einer Beschreibung
2. Analysieren eines Bildes mit einem domänenspezifischen Modell (speziell dem „Prominenten“-Modell) und Erhalten des entsprechenden Ergebnisses in JSON Retune.

Funktionen werden in folgende Bereiche unterteilt:

- **Option 1:** Bereichsbezogene-Analyse – Analyse nur für ein angegebenes Modell
- **Option 2:** Erweiterte Analyse – Analyse zur Bereitstellung zusätzlicher Details mit [86 Kategorien der Taxonomie](../Category-Taxonomy.md)
  
## <a name="authorize-the-api-call"></a>Autorisieren des API-Aufrufs

Für jeden Aufruf der Maschinelles Sehen-API ist ein Abonnementschlüssel erforderlich. Dieser Schlüssel muss entweder über einen Abfragezeichenfolgen-Parameter übergeben oder im Anforderungsheader angegeben werden.

Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) können Sie einen Schlüssel für eine kostenlose Testversion abrufen. Alternativ gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um „Maschinelles Sehen“ zu abonnieren und Ihren Schlüssel zu erhalten.

1. Informationen zur Übergabe des Abonnementschlüssels über eine Abfragezeichenfolge finden Sie weiter unten als Beispiel für die Maschinelles Sehen-API:

    ```https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>```

1. Die Übergabe des Abonnementschlüssels kann auch im HTTP-Anforderungsheader angegeben werden:

    ```ocp-apim-subscription-key: <Your subscription key>```

1. Bei Verwendung der Clientbibliothek wird der Abonnementschlüssel über den Konstruktor von „ComputerVisionClient“ übergeben, und die Region wird in einer Eigenschaft des Clients angegeben:

    ```
    var visionClient = new ComputerVisionClient(new ApiKeyServiceClientCredentials("Your subscriptionKey"))
    {
        Endpoint = "https://westus.api.cognitive.microsoft.com"
    }
    ```

## <a name="upload-an-image-to-the-computer-vision-api-service-and-get-back-tags-descriptions-and-celebrities"></a>Hochladen eines Bilds in den Maschinelles Sehen-API-Dienst und Rückgabe von Tags, Beschreibungen und Prominenten

Die einfachste Möglichkeit für den Aufruf der Maschinelles Sehen-API ist das direkte Hochladen eines Bilds. Dies erfolgt durch Senden einer POST-Anforderung des Inhaltstyps „application/octet-stream“ beim Lesen von Daten aus einem Bild. Für „Tags“ und „Description“ ist diese Uploadmethode für alle Aufrufe der Maschinelles Sehen-API gleich. Der einzige Unterschied besteht in den Abfrageparametern, die der Benutzer angibt. 

So rufen Sie „Tags“ und „Description“ für ein bestimmtes Bild ab:

**Option 1:** Abrufen der Liste mit „Tags“ und einer „Description“

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
```

```csharp
using System.IO;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

ImageAnalysis imageAnalysis;
var features = new VisualFeatureTypes[] { VisualFeatureTypes.Tags, VisualFeatureTypes.Description };

using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  imageAnalysis = await visionClient.AnalyzeImageInStreamAsync(fs, features);
}
```

**2. Option** Abrufen der Liste mit nur „Tags“ oder einer Liste mit nur „Description“:

###### <a name="tags-only"></a>Nur „Tags“:

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/tag?subscription-key=<Your subscription key>
var tagResults = await visionClient.TagImageAsync("http://contoso.com/example.jpg");
```

###### <a name="description-only"></a>Nur „Description“:

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/describe?subscription-key=<Your subscription key>
using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  imageDescription = await visionClient.DescribeImageInStreamAsync(fs);
}
```

### <a name="get-domain-specific-analysis-celebrities"></a>Abrufen domänenspezifischer Analysen (Prominente)

**Option 1:** Bereichsbezogene-Analyse – Analyse nur für ein angegebenes Modell
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```

Bei dieser Option sind alle anderen Abfrageparameter {VisualFeatures, details} ungültig. Wenn alle unterstützten Modelle angezeigt werden sollen, verwenden Sie:

```
GET https://westus.api.cognitive.microsoft.com/vision/v2.0/models 
var models = await visionClient.ListModelsAsync();
```

**Option 2:** Erweiterte Analyse – Analyse zur Bereitstellung zusätzlicher Details mit [86 Kategorien der Taxonomie](../Category-Taxonomy.md)

Für Anwendungen, bei denen Sie neben Details aus einem oder mehreren domänenspezifischen Modellen auch eine generische Bildanalyse wünschen, erweitern wir die v1-API um den Modellabfrageparameter.

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?details=celebrities
```

Wenn diese Methode aufgerufen wird, werden wir zuerst die 86-Kategorie-Klassifizierung aufrufen. Wenn eine der Kategorien mit der eines bekannten/übereinstimmenden Modells übereinstimmt, wird die Klassifizierung ein zweites Mal aufgerufen. Wenn zum Beispiel „details=all“ oder „details“ „celebrities“ beinhalten, rufen wir das Prominentenmodell auf, nachdem der 86-Kategorie-Klassifizierer aufgerufen wurde und das Ergebnis die Kategorie „person“ beinhaltet. Dies wird die Latenzzeit für Benutzer, die sich für Prominente interessieren, im Vergleich zu Option 1 erhöhen.

Alle v1-Abfrageparameter verhalten sich in diesem Fall gleich.  Wenn „visualFeatures=categories“ nicht angegeben ist, wird es implizit aktiviert.

## <a name="retrieve-and-understand-the-json-output-for-analysis"></a>Abrufen und Nachvollziehen der JSON-Ausgabe für die Analyse

Hier sehen Sie ein Beispiel:

```json
{  
  "tags":[  
    {  
      "name":"outdoor",
      "score":0.976
    },
    {  
      "name":"bird",
      "score":0.95
    }
  ],
  "description":{  
    "tags":[  
      "outdoor",
      "bird"
    ],
    "captions":[  
      {  
        "text":"partridge in a pear tree",
        "confidence":0.96
      }
    ]
  }
}
```

Feld | type | Inhalt
------|------|------|
`Tags`  | `object` | Objekt der obersten Ebene für das Array von Tags
tags[].Name | `string`  | Schlüsselwort von Tags-Klassifizierung
tags[].Score    | `number`  | Zuverlässigkeitsbewertung, zwischen 0 und 1.
description  | `object` | Objekt der obersten Ebene für eine Beschreibung.
description.tags[] |    `string`    | Liste der Tags.  Wenn nicht sicher ist, dass eine Beschriftung erstellt werden kann, sind diese Tags ggf. die einzigen Informationen, die dem Aufrufenden zur Verfügung stehen.
description.captions[].text | `string`  | Ein Ausdruck, der das Bild beschreibt.
description.captions[].confidence   | `number`  | Zuverlässigkeit für den Ausdruck.

## <a name="retrieve-and-understand-the-json-output-of-domain-specific-models"></a>Abrufen und Nachvollziehen der JSON-Ausgabe für domänenspezifische Modelle

**Option 1:** Bereichsbezogene-Analyse – Analyse nur für ein angegebenes Modell

Die Ausgabe wird ein Array von Tags sein, wie beispielsweise unten:

```json
{  
  "result":[  
    {  
      "name":"golden retriever",
      "score":0.98
    },
    {  
      "name":"Labrador retriever",
      "score":0.78
    }
  ]
}
```

**Option 2:** Erweiterte Analyse – Analyse zur Bereitstellung zusätzlicher Details mit 86 Kategorien der Taxonomie

Bei domänenspezifischen Modellen mit Option 2 (Erweiterte Analyse) wird der Rückgabetyp „categories“ erweitert. Ein Beispiel folgt:

```json
{  
  "requestId":"87e44580-925a-49c8-b661-d1c54d1b83b5",
  "metadata":{  
    "width":640,
    "height":430,
    "format":"Jpeg"
  },
  "result":{  
    "celebrities":[  
      {  
        "name":"Richard Nixon",
        "faceRectangle":{  
          "left":107,
          "top":98,
          "width":165,
          "height":165
        },
        "confidence":0.9999827
      }
    ]
  }
}
```

Das Feld „categories“ ist eine Liste einer oder mehrerer der [86 Kategorien](../Category-Taxonomy.md) in der ursprünglichen Taxonomie. Beachten Sie auch, dass Kategorien, die mit einem Unterstrich enden, mit dieser Kategorie und ihren untergeordneten Kategorien übereinstimmen (z.B. „people_“ sowie „people_group“, für Prominentenmodell).

Feld   | type  | Inhalt
------|------|------|
categories | `object`   | Objekt der obersten Ebene
categories[].name    | `string` | Name aus der 86-Kategorie-Taxonomie
categories[].score  | `number`  | Zuverlässigkeitsbewertung, zwischen 0 und 1
categories[].detail  | `object?`      | Optionales Detailobjekt

Beachten Sie, dass, wenn mehrere Kategorien übereinstimmen (z.B. gibt die 86-Kategorie-Klassifizierung eine Bewertung für „people_“ und „people_young“ zurück, wenn „model=celebrities“), die Details an die allgemeinste Übereinstimmung angehängt werden („people_“ in diesem Beispiel.).

## <a name="errors-responses"></a>Fehlermeldungen

Diese sind identisch mit vision.analyze, mit dem zusätzlichen Fehler „NotSupportedModel Error (HTTP 400)“, der sowohl in Szenarios in Option 1 als auch in Option 2 zurückgegeben werden kann. Für Option 2 (Erweiterte Analyse) gibt die API ein „NotSupportedModel zurück“, wenn eines der in den Details angegebenen Modelle nicht erkannt wird, auch wenn eines oder mehrere von ihnen gültig sind.  Benutzer können „listModels“ aufrufen, um zu sehen, welche Modelle unterstützt werden.

## <a name="next-steps"></a>Nächste Schritte

Um die REST-API zu verwenden, wechseln Sie zur [Referenz für die Maschinelles Sehen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44).
