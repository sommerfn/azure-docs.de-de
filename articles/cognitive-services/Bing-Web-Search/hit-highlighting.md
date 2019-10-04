---
title: Verwendung von Dekorationsmarkierungen zum Hervorheben von Text – Bing-Websuche-API
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mit der Bing-Websuche-API Textverzierungen und Hervorhebungen in Ihren Suchergebnissen verwenden können.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 5365B568-EA55-4D97-8FBE-0AF60158D4D5
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: scottwhi
ms.openlocfilehash: a6d394fec6e7cf0a230f61ad05c236a1f84dad9d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854014"
---
# <a name="using-decoration-markers-to-highlight-text"></a>Verwenden von Dekorationsmarkierungen zum Markieren von Text

Für Bing wird die Treffermarkierung unterstützt, bei der Abfrageausdrücke (oder andere Ausdrücke, die von Bing als relevant eingestuft werden) in den Anzeigezeichenfolgen einiger Antworten markiert werden. Beispielsweise können die Felder `name`, `displayUrl` und `snippet` der Ergebnisse einer Webseite markierte Abfragebegriffe enthalten. 

Standardmäßig werden in Bing keine Hervorhebungsmarkierungen in Anzeigezeichenfolgen eingefügt. Fügen Sie zum Aktivieren der Markierungen den Abfrageparameter `textDecorations` in Ihre Anforderung ein, und legen Sie ihn auf `true` fest.

## <a name="hit-highlighting-example"></a>Beispiel für Treffermarkierung

Im folgenden Beispiel wird ein Webergebnis für `Sailing Dinghy` veranschaulicht. In Bing werden Anfang und Ende von Abfrageausdrücken mit den Unicode-Zeichen E000 und E001 markiert.
  
![Treffermarkierung](./media/cognitive-services-bing-web-api/bing-hit-highlighting.png) 

Bevor das Ergebnis auf Ihrer Benutzeroberfläche angezeigt wird, sollten Sie die Unicode-Zeichen durch Zeichen ersetzen, die für Ihr Anzeigeformat geeignet sind.

## <a name="marker-formatting"></a>Markerformatierung

In Bing haben Sie die Möglichkeit, Unicode-Zeichen oder HTML-Tags als Marker bereitzustellen. Fügen Sie den Abfrageparameter [textFormat](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#textformat) ein, um anzugeben, welche Marker verwendet werden sollen. 

| Wert             | Marker                       |
|-------------------|------------------------------|
| `textFormat=Raw`  | Unicode-Zeichen (Standard) |
| `textFormat=HTML` | HTML-Zeichen              |

## <a name="additional-text-decorations"></a>Zusätzliche Textdekorationen

Mit Bing können mehrere verschiedene Textdekorationen zurückgegeben werden. Eine `Computation`-Antwort kann z. B. tiefgestellte Marker für den Abfragebegriff `log(2)` im Feld `expression` enthalten.

![Computation-Markierungen](./media/cognitive-services-bing-web-api/bing-markers-computation.png) 

Wenn die Anforderung keine Dekorationen angibt, würde das Feld `expression` den Wert `log10(2)` enthalten. 

Wenn `textDecorations` auf `true` festgelegt ist, können in Bing die unten angegebenen Marker in die Anzeigezeichenfolgen von Antworten eingefügt werden. Falls keine Entsprechung als HTML-Tag vorhanden ist, enthält die Tabellenzelle keine Angabe.

|Unicode|HTML|BESCHREIBUNG
|-|-|-
|U+E000|\<b&gt;|Markiert den Anfang des Abfrageausdrucks (Treffermarkierung)
|U+E001|\</b&gt;|Markiert das Ende des Abfrageausdrucks
|U+E002|\<i&gt;|Markiert den Anfang von Text in Kursivdruck 
|U+E003|\</i&gt;|Markiert das Ende von Text in Kursivdruck
|U+E004|\<br/&gt;|Markiert einen Zeilenumbruch
|U+E005||Markiert den Anfang einer Telefonnummer
|U+E006||Markiert das Ende einer Telefonnummer
|U+E007||Markiert den Anfang einer Adresse
|U+E008||Markiert das Ende einer Adresse
|U+E009|\&nbsp;|Markiert ein geschütztes Leerzeichen
|U+E00C|\<strong&gt;|Markiert den Anfang von Text in Fettdruck
|U+E00D|\</strong&gt;|Markiert das Ende von Text in Fettdruck
|U+E00E||Markiert den Anfang von Text, dessen Hintergrund heller als der umgebende Hintergrund sein soll
|U+E00F||Markiert das Ende von Text, dessen Hintergrund heller als der umgebende Hintergrund sein soll
|U+E010||Markiert den Anfang von Text, dessen Hintergrund dunkler als der umgebende Hintergrund sein soll
|U+E011||Markiert das Ende von Text, dessen Hintergrund dunkler als der umgebende Hintergrund sein soll
|U+E012|\<del&gt;|Markiert den Anfang von Text, der durchgestrichen sein soll
|U+E013|\</del&gt;|Markiert das Ende von Text, der durchgestrichen sein soll
|U+E016|\<sub&gt;|Markiert den Anfang von tiefgestelltem Text
|U+E017|\</sub&gt;|Markiert das Ende von tiefgestelltem Text
|U+E018|\<sup&gt;|Markiert den Anfang von hochgestelltem Text
|U+E019|\</sup&gt;|Markiert das Ende von hochgestelltem Text

## <a name="next-steps"></a>Nächste Schritte

* [Was ist die Bing-Websuche-API?](overview.md) 
* [Ändern der Größe und Zuschneiden von Miniaturansichten](resize-and-crop-thumbnails.md)