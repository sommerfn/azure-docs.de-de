---
title: Aufrufen der Maschinelles Sehen-API
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mithilfe der REST-API in Azure Cognitive Services die Maschinelles Sehen-API aufrufen.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 09/09/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 298228eedb73298f00654f4f72c201d9ed671090
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177063"
---
# <a name="call-the-computer-vision-api"></a>Aufrufen der Maschinelles Sehen-API

In diesem Artikel wird beschrieben, wie die Maschinelles Sehen-API mithilfe der Rest-API aufgerufen wird. Die Beispiele wurden in C# sowohl mithilfe der Clientbibliothek der Maschinelles Sehen-API als auch als HTTP POST- oder GET-Aufrufe geschrieben. Der Schwerpunkt dieses Artikels liegt auf Folgendem:

- Abrufen von Tags, einer Beschreibung und Kategorien
- Abrufen domänenspezifischer Informationen bzw. „Prominenten“

## <a name="prerequisites"></a>Voraussetzungen

- Eine Bild-URL oder ein Pfad zum lokal gespeicherten Bild
- Unterstützte Eingabemethoden: rohe Bildbinärdaten in Form einer Anwendung/eines Oktettdatenstroms oder einer Bild-URL
- Unterstützte Bildformate: JPEG, PNG, GIF und BMP
- Bilddateigröße: maximal 4 MB
- Bildmaße: mindestens 50 &times; 50 Pixel
  
In den Beispielen dieses Artikels werden die folgenden Features gezeigt:

* Analysieren eines Bilds zur Rückgabe eines Arrays an Tags und einer Beschreibung
* Analysieren eines Bilds mit einem domänenspezifischen Modell (insbesondere dem „Prominenten“-Modell) zur Rückgabe des entsprechenden Ergebnisses in JSON

Die Features bieten die folgenden Optionen:

- **Option 1**: Bereichsbezogene Analyse – Analyse nur für ein angegebenes Modell
- **Option 2**: Erweiterte Analyse – Analyse zur Bereitstellung zusätzlicher Details mit der [Taxonomie mit 86 Kategorien](../Category-Taxonomy.md)
  
## <a name="authorize-the-api-call"></a>Autorisieren des API-Aufrufs

Für jeden Aufruf der Maschinelles Sehen-API ist ein Abonnementschlüssel erforderlich. Dieser Schlüssel muss entweder über einen Parameter mit der Abfragezeichenfolge übergeben oder im Anforderungsheader angegeben werden.

Um einen kostenlosen Testschlüssel zu erhalten, führen Sie einen der folgenden Schritte aus:
* Navigieren Sie zur Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). 
* Wechseln Sie zur Seite [Erstellen einer Cognitive Services-Ressource](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account), um maschinelles Sehen zu abonnieren.

Sie können den Abonnementschlüssel übergeben, indem Sie eine der folgenden Aktionen ausführen:

* Übergeben Sie ihn wie in diesem Beispiel für die Maschinelles Sehen-API über eine Abfragezeichenfolge:

  ```
  https://westus.api.cognitive.microsoft.com/vision/v2.1/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
  ```

* Geben Sie ihn im HTTP-Anforderungsheader an:

  ```
  ocp-apim-subscription-key: <Your subscription key>
  ```

* Übergeben Sie den Schlüssel bei Verwendung der Clientbibliothek über den ComputerVisionClient-Konstruktor, und geben Sie die Region in einer Eigenschaft des Clients an:

    ```
    var visionClient = new ComputerVisionClient(new ApiKeyServiceClientCredentials("Your subscriptionKey"))
    {
        Endpoint = "https://westus.api.cognitive.microsoft.com"
    }
    ```

## <a name="upload-an-image-to-the-computer-vision-api-service"></a>Hochladen eines Bilds zum Maschinelles Sehen-API-Dienst

Die einfachste Möglichkeit für den Aufruf der Maschinelles Sehen-API und die Rückgabe von Tags, eine Beschreibung und Prominenten ist das direkte Hochladen eines Bilds. Senden Sie hierzu eine „POST“-Anforderung mit dem Bild im HTTP-Text zusammen mit den aus dem Bild gelesenen Daten. Die Uploadmethode ist für alle Maschinelles Sehen-API-Aufrufe identisch. Der einzige Unterschied besteht in den von Ihnen festgelegten Abfrageparametern. 

Rufen Sie für ein bestimmtes Bild Tags und eine Beschreibung mithilfe einer der folgenden Optionen ab:

### <a name="option-1-get-a-list-of-tags-and-a-description"></a>Option 1: Abrufen einer Liste mit Tags und einer Beschreibung

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.1/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
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

### <a name="option-2-get-a-list-of-tags-only-or-a-description-only"></a>Option 2: Abrufen einer Liste mit nur Tags oder nur einer Beschreibung

Führen Sie für nur Tags Folgendes aus:

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.1/tag?subscription-key=<Your subscription key>
var tagResults = await visionClient.TagImageAsync("http://contoso.com/example.jpg");
```

Führen Sie für nur eine Beschreibung Folgendes aus:

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.1/describe?subscription-key=<Your subscription key>
using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  imageDescription = await visionClient.DescribeImageInStreamAsync(fs);
}
```

## <a name="get-domain-specific-analysis-celebrities"></a>Abrufen domänenspezifischer Analysen (Prominente)

### <a name="option-1-scoped-analysis---analyze-only-a-specified-model"></a>Option 1: Bereichsbezogene Analyse – Analyse nur für ein angegebenes Modell
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.1/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```

Bei dieser Option sind alle anderen Abfrageparameter {VisualFeatures, details} ungültig. Wenn alle unterstützten Modelle angezeigt werden sollen, verwenden Sie:

```
GET https://westus.api.cognitive.microsoft.com/vision/v2.1/models 
var models = await visionClient.ListModelsAsync();
```

### <a name="option-2-enhanced-analysis---analyze-to-provide-additional-details-by-using-86-categories-taxonomy"></a>Option 2: Erweiterte Analyse – Analyse zur Bereitstellung zusätzlicher Details mit der 86-Kategorien-Taxonomie

Für Anwendungen, bei denen Sie neben Details aus mindestens einem domänenspezifischen Modell auch eine generische Bildanalyse wünschen, erweitern Sie die v1-API mithilfe des Modellabfrageparameters.

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.1/analyze?details=celebrities
```

Wenn Sie diese Methode aufrufen, rufen Sie zuerst den Klassifizierer [86 Kategorien](../Category-Taxonomy.md) auf. Wenn eine der Kategorien mit der eines bekannten oder übereinstimmenden Modells übereinstimmt, wird der Klassifizierer ein zweites Mal aufgerufen. Wenn beispielsweise „celebrities“ in „details=all“ oder „details“ enthalten ist, rufen Sie nach dem 86-Kategorien-Klassifizierer das Prominentenmodell auf. Das Ergebnis enthält die Kategorieperson. Im Gegensatz zu Option 1 erhöht diese Methode die Wartezeit für Benutzer, die an Prominenten interessiert sind.

Alle v1-Abfrageparameter verhalten sich in diesem Fall gleich. Wenn Sie „visualFeatures=categories“ nicht angeben, ist es implizit aktiviert.

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
`Tags`  | `object` | Das Objekt der obersten Ebene für das Array von Tags
tags[].Name | `string`  | Das Schlüsselwort vom Tags-Klassifizierer
tags[].Score    | `number`  | Die Zuverlässigkeitsbewertung, zwischen 0 und 1
description  | `object` | Das Objekt der obersten Ebene für eine Beschreibung
description.tags[] |    `string`    | Die Liste der Tags.  Wenn nicht sicher ist, dass eine Beschriftung erstellt werden kann, sind diese Tags ggf. die einzigen Informationen, die dem Aufrufenden zur Verfügung stehen.
description.captions[].text | `string`  | Ein Ausdruck, der das Bild beschreibt.
description.captions[].confidence   | `number`  | Die Zuverlässigkeit für den Ausdruck

## <a name="retrieve-and-understand-the-json-output-of-domain-specific-models"></a>Abrufen und Nachvollziehen der JSON-Ausgabe für domänenspezifische Modelle

### <a name="option-1-scoped-analysis---analyze-only-a-specified-model"></a>Option 1: Bereichsbezogene Analyse – Analyse nur für ein angegebenes Modell

Bei der Ausgabe handelt es sich um ein Array von Tags, wie im folgenden Beispiel gezeigt:

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

### <a name="option-2-enhanced-analysis---analyze-to-provide-additional-details-by-using-the-86-categories-taxonomy"></a>Option 2: Erweiterte Analyse – Analyse zur Bereitstellung zusätzlicher Details mit der 86-Kategorien-Taxonomie

Bei domänenspezifischen Modellen mit Option 2 (erweiterte Analyse) wird der Rückgabetyp „categories“ wie im folgenden Beispiel gezeigt erweitert:

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

Das Feld „categories“ ist eine Liste einer oder mehrerer der [86 Kategorien](../Category-Taxonomy.md) in der ursprünglichen Taxonomie. Kategorien, die mit einem Unterstrich enden, stimmen mit dieser Kategorie und ihren untergeordneten Kategorien überein (z.B. „people_“ sowie „people_group“, für Prominentenmodell).

Feld   | type  | Inhalt
------|------|------|
categories | `object`   | Das Objekt der obersten Ebene
categories[].name    | `string` | Der Name aus der Taxonomieliste mit 86 Kategorien
categories[].score  | `number`  | Die Zuverlässigkeitsbewertung, zwischen 0 und 1
categories[].detail  | `object?`      | (Optional) Das Detailobjekt

Wenn mehrere Kategorien übereinstimmen (z. B. wenn der 86-Kategorie-Klassifizierer eine Bewertung für „people_“ und „people_young“ zurückgibt, wenn „model=celebrities“), werden die Details an die allgemeinste Übereinstimmung angehängt („people_“ in diesem Beispiel).

## <a name="error-responses"></a>Fehlercodes

Diese Fehler sind identisch mit den Fehlern in „vision.analyze“ mit dem zusätzlichen Fehler „NotSupportedModel“ (HTTP 400), der sowohl in Szenarien in Option 1 als auch in Option 2 zurückgegeben werden kann. Für Option 2 (erweiterte Analyse) gibt die API „NotSupportedModel“ zurück, wenn eines der in den Details angegebenen Modelle nicht erkannt wird, auch wenn mindestens eines davon gültig ist. Rufen Sie „listModels“ auf, um zu sehen, welche Modelle unterstützt werden.

## <a name="next-steps"></a>Nächste Schritte

Um die REST-API zu verwenden, wechseln Sie zur [Referenz für die Maschinelles Sehen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44).
