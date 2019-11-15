---
title: Markenerkennung – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird ein spezieller Modus der Objekterkennung, der Marken- und/oder Logoerkennung unter Verwendung der Maschinelles Sehen-API behandelt.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: pafarley
ms.openlocfilehash: 117beca1284f28c75c1ac772425423f732b8a236
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73718637"
---
# <a name="detect-popular-brands-in-images"></a>Erkennen von bekannten Marken in Bildern

Markenerkennung ist ein spezieller Modus der [Objekterkennung](concept-object-detection.md), der eine Datenbank mit Tausenden von Logos aus der ganzen Welt verwendet, um Handelsmarken in Bildern oder Videos zu identifizieren. Mit diesem Feature können Sie beispielsweise ermitteln, welche Marken in sozialen Medien besonders beliebt sind oder besonders gerne in Medien platziert werden.

Der Dienst „Maschinelles Sehen“ erkennt, ob in einem bestimmten Bild Markenlogos vorhanden sind. Falls ja, gibt er den Markennamen, eine Zuverlässigkeitsbewertung und die Koordinaten eines Begrenzungsrahmens um das Logo zurück.

Die integrierte Logodatenbank deckt bekannte Marken aus Bereichen wie u.a. Unterhaltungselektronik und Bekleidung ab. Wenn Sie feststellen, dass die gesuchte Marke nicht vom Dienst „Maschinelles Sehen“ erkannt wird, ist es möglicherweise besser, mit dem Dienst [Custom Vision](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) einen eigenen Logodetektor zu erstellen und zu trainieren.

## <a name="brand-detection-example"></a>Beispiel für die Markenerkennung

Die folgenden JSON-Antworten veranschaulichen, was vom Dienst „Maschinelles Sehen“ beim Erkennen von Objekten im Beispielbild zurückgegeben wird.

![Ein rotes T-Shirt mit Microsoft-Schriftzug und -Logo](./Images/red-shirt-logo.jpg)

```json
"brands":[  
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":20,
         "y":97,
         "w":62,
         "h":52
      }
   }
]
```

In einigen Fällen wird der Markendetektor sowohl das Logobild als auch den stilisierten Markennamen als zwei separate Logos auswerten.

![Ein graues Sweatshirt mit Microsoft-Schriftzug und -Logo](./Images/gray-shirt-logo.jpg)

```json
"brands":[  
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":58,
         "y":106,
         "w":55,
         "h":46
      }
   },
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":58,
         "y":86,
         "w":202,
         "h":63
      }
   }
]
```

## <a name="use-the-api"></a>Verwenden der API

Die Funktion zur Erkennung von Marken ist Teil der [Bildanalyse](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)-API. Sie können diese API über ein natives SDK oder REST-Aufrufe aufrufen. Beziehen Sie `Brands` in den Abfrageparameter **visualFeatures** ein. Nachdem Sie die vollständige JSON-Antwort erhalten haben, analysieren Sie einfach die Zeichenfolge auf den Inhalt im Abschnitt `"brands"`.

* [Schnellstart: Maschinelles Sehen: .NET SDK](./quickstarts-sdk/csharp-sdk.md)
* [Schnellstart: Analysieren eines Bilds (REST-API)](./quickstarts/csharp-analyze.md)
