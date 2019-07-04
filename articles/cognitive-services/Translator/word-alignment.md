---
title: Wortausrichtung – Textübersetzungs-API
titlesuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie über die Textübersetzungs-API Informationen zur Wortausrichtung erhalten.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.custom: seodec18
ms.openlocfilehash: 3db5e9651e307211e9dccee20bb8d69586bb9ef1
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448159"
---
# <a name="how-to-receive-word-alignment-information"></a>Empfangen von Informationen zur Wortausrichtung

## <a name="receiving-word-alignment-information"></a>Empfangen von Informationen zur Wortausrichtung
Verwenden Sie zum Empfangen von Informationen zur Wortausrichtung die Translate-Methode, und binden Sie den optionalen Parameter includeAlignment ein.

## <a name="alignment-information-format"></a>Format der Informationen zur Ausrichtung
Die Ausrichtung wird als Zeichenfolgenwert des folgenden Formats für jedes Wort der Quelle zurückgegeben. Die Informationen für jedes Wort sind durch ein Leerzeichen getrennt. Dies gilt auch für Sprachen (Skripts), die keine Leerzeichen als Trennung enthalten, z.B. Chinesisch:

[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]] *

Beispiel für Ausrichtungszeichenfolge: „0:0-7:10 1:2-11:20 3:4-0:3 3:4-4:6 5:5-21:21“.

Anders ausgedrückt: Der Doppelpunkt trennt Start- und Endindex, der Bindestrich trennt die Sprachen, und Leerzeichen trennen die Wörter. Ein Wort kann mit Null, einem oder mehreren Wörtern in der anderen Sprache übereinstimmen, und die ausgerichteten Wörter sind ggf. nicht zusammenhängend. Wenn keine Informationen für die Ausrichtung verfügbar sind, ist das Ausrichtungselement leer. In diesem Fall gibt die Methode keinen Fehler zurück.

## <a name="restrictions"></a>Einschränkungen
Die Ausrichtung wird derzeit nur für eine Teilmenge der Sprachpaare zurückgegeben:
* aus dem Englischen in eine andere Sprache
* aus einer beliebigen anderen Sprache ins Englische mit Ausnahme von Chinesisch (vereinfacht) und Chinesisch (traditionell) sowie aus dem Lettischen ins Englische
* aus dem Japanischen ins Koreanische oder aus dem Koreanischen ins Japanische Sie erhalten keine Ausrichtungsinformationen, wenn es sich bei dem Satz um eine vordefinierte Übersetzung handelt. Beispiele für eine vordefinierte Übersetzung im Englischen sind „This is a test“, „I love you“ und andere häufig verwendete Sätze.

## <a name="example"></a>Beispiel

JSON-Beispiel

```json
[
  {
    "translations": [
      {
        "text": "Kann ich morgen Ihr Auto fahren?",
        "to": "de",
        "alignment": {
          "proj": "0:2-0:3 4:4-5:7 6:10-25:30 12:15-16:18 17:19-20:23 21:28-9:14 29:29-31:31"
        }
      }
    ]
  }
]
```
