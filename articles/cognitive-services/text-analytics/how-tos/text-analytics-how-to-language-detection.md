---
title: Sprachenerkennung mit der Textanalyse-REST-API | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe der Textanalyse-REST-API von Azure Cognitive Services Sprachen erkennen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: e1adeb34cf999f471bb183e4d7de9c65427252bb
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "67986509"
---
# <a name="example-detect-language-with-text-analytics"></a>Beispiel: Sprachenerkennung mithilfe der Textanalyse

Das Feature für die [Sprachenerkennung](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) der Azure-Textanalyse-REST-API wertet Texteingaben für jedes Dokument aus und gibt Sprachen-IDs mit einer Punktzahl zurück, die die Stärke der Analyse angibt.

Diese Funktion ist hilfreich für Inhaltsspeicher, die willkürliche Texte mit unbekannter Sprache sammeln. Sie können die Analyseergebnisse analysieren, um die Sprache des Eingabedokuments zu bestimmen. Die Antwort gibt außerdem eine Punktzahl zurück, die die Zuverlässigkeit des Modells widerspiegelt. Der Wert liegt zwischen 0 und 1.

Mit dem Feature für die Sprachenerkennung können eine Vielzahl von Sprachen, Varianten und Dialekten sowie einige Regional- und Kultursprachen erkannt werden. Für dieses Feature wird keine genaue Liste mit Sprachen veröffentlicht.

Bei Inhalten in einer seltener verwendeten Sprache können Sie das Feature für die Sprachenerkennung ausprobieren, um zu sehen, ob es einen Code zurückgibt. Die Antwort bei Sprachen, die nicht erkannt werden können, lautet `unknown`.

> [!TIP]
> Die Textanalyse bietet darüber hinaus ein Linux-basiertes Docker-Containerimage für die Spracherkennung, damit Sie [den Textanalysecontainer nah bei Ihren Daten installieren und ausführen können](text-analytics-how-to-install-containers.md).

## <a name="preparation"></a>Vorbereitung

Sie benötigen JSON-Dokumente im folgenden Format: ID und Text.

Ein Dokument darf maximal 5.119 Zeichen enthalten. Pro Sammlung können bis zu 1.000 Elemente (IDs) vorhanden sein. Die Sammlung wird im Hauptteil der Anforderung übermittelt. Das folgende Beispiel zeigt Inhalte, die Sie für die Sprachenerkennung übermitteln können:

   ```
    {
        "documents": [
            {
                "id": "1",
                "text": "This document is in English."
            },
            {
                "id": "2",
                "text": "Este documento está en inglés."
            },
            {
                "id": "3",
                "text": "Ce document est en anglais."
            },
            {
                "id": "4",
                "text": "本文件为英文"
            },                
            {
                "id": "5",
                "text": "Этот документ на английском языке."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>Schritt 1: Strukturieren der Anforderung

Weitere Informationen zur Anforderungsdefinition finden Sie unter [Aufrufen der Textanalyse-REST-API](text-analytics-how-to-call-api.md). Der Einfachheit halber sind hier noch einmal einige Punkte aufgeführt:

+ Erstellen Sie eine POST-Anforderung. Die API-Dokumentation für diese Anforderung finden Sie [hier](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7).

+ Legen Sie den HTTP-Endpunkt für die Sprachenerkennung fest. Verwenden Sie entweder eine Textanalyseressource in Azure oder einen instanziierten [Textanalysecontainer](text-analytics-how-to-install-containers.md). Er muss die Ressource `/languages` enthalten: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/languages`.

+ Legen Sie einen Anforderungsheader fest, der den Zugriffsschlüssel für Textanalysevorgänge enthält. Weitere Informationen finden Sie unter [Suchen von Endpunkten und Zugriffsschlüsseln für die Textanalyse von Cognitive Service](text-analytics-how-to-access-key.md).

+ Geben Sie im Anforderungstext die JSON-Dokumentsammlung an, die Sie für diese Analyse vorbereitet haben.

> [!Tip]
> Verwenden Sie [Postman](text-analytics-how-to-call-api.md), oder öffnen Sie die **API-Testkonsole** in der [Dokumentation](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7), um eine Anforderung zu strukturieren und mittels POST an den Dienst zu übermitteln.

## <a name="step-2-post-the-request"></a>Schritt 2: Senden der Anforderung per POST

Die Analyse erfolgt, wenn die Anforderung eingeht. Informationen zur Größe und Anzahl von Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie in der Übersicht im Abschnitt [Datengrenzwerte](../overview.md#data-limits).

Vergessen Sie nicht, dass der Dienst zustandslos ist. In Ihrem Konto werden keine Daten gespeichert. Die Ergebnisse werden direkt in der Antwort zurückgegeben.


## <a name="step-3-view-the-results"></a>Schritt 3: Zeigen Sie die Ergebnisse an

Alle POST-Anforderungen geben eine Antwort im JSON-Format mit den IDs und erkannten Eigenschaften zurück.

Die Ausgabe wird umgehend zurückgegeben. Sie können die Ergebnisse an eine Anwendung streamen, die JSON akzeptiert, oder die Ausgabe in einer Datei im lokalen System speichern. Importieren Sie dann die Ausgabe in eine Anwendung, mit der Sie die Daten sortieren, durchsuchen und bearbeiten können.

Ergebnisse für die Beispielanforderung sollten wie im folgenden JSON-Code aussehen. Beachten Sie, dass es sich hierbei um ein einzelnes Dokument mit mehreren Elementen handelt. Die Ausgabe erfolgt in englischer Sprache. Sprachen-IDs enthalten einen Anzeigenamen und einen Sprachcode im Format [ISO 639-1](https://www.iso.org/standard/22109.html).

Das positive Ergebnis 1,0 steht für die höchstmögliche Zuverlässigkeit der Analyse.



```
{
    "documents": [
        {
            "id": "1",
            "detectedLanguages": [
                {
                    "name": "English",
                    "iso6391Name": "en",
                    "score": 1
                }
            ]
        },
        {
            "id": "2",
            "detectedLanguages": [
                {
                    "name": "Spanish",
                    "iso6391Name": "es",
                    "score": 1
                }
            ]
        },
        {
            "id": "3",
            "detectedLanguages": [
                {
                    "name": "French",
                    "iso6391Name": "fr",
                    "score": 1
                }
            ]
        },
        {
            "id": "4",
            "detectedLanguages": [
                {
                    "name": "Chinese_Simplified",
                    "iso6391Name": "zh_chs",
                    "score": 1
                }
            ]
        },
        {
            "id": "5",
            "detectedLanguages": [
                {
                    "name": "Russian",
                    "iso6391Name": "ru",
                    "score": 1
                }
            ]
        }
    ],
```

### <a name="ambiguous-content"></a>Mehrdeutige Inhalte

Wenn das Analysetool die Eingabe nicht analysieren kann, wird `(Unknown)` zurückgegeben. Beispiel: Sie übermitteln einen Textblock, der ausschließlich aus arabischen Zahlen besteht.

```
    {
      "id": "5",
      "detectedLanguages": [
        {
          "name": "(Unknown)",
          "iso6391Name": "(Unknown)",
          "score": "NaN"
        }
      ]
```
### <a name="mixed-language-content"></a>Inhalt in verschiedenen Sprachen

Wenn in einem Dokument Inhalte in verschiedenen Sprachen enthalten sind, wird die Sprache zurückgegeben, die in den Inhalten überwiegt – allerdings mit einer niedrigeren positiven Bewertung. Die Bewertung spiegelt die marginale Stärke dieser Bewertung wider. Im folgenden Beispiel enthält die Eingabe eine Mischung aus Englisch, Spanisch und Französisch. Das Analysetool zählt die Zeichen der einzelnen Segmente, um die vorherrschende Sprache zu bestimmen.

**Input** (Eingabe)

```
{
  "documents": [
    {
      "id": "1",
      "text": "Hello, I would like to take a class at your University. ¿Se ofrecen clases en español? Es mi primera lengua y más fácil para escribir. Que diriez-vous des cours en français?"
    }
  ]
}
```

**Ausgabe**

Die resultierende Ausgabe enthält die vorherrschende Sprache mit einer Punktzahl von weniger als 1,0, wodurch eine niedrigere Zuverlässigkeit angegeben wird.

```
{
  "documents": [
    {
      "id": "1",
      "detectedLanguages": [
        {
          "name": "Spanish",
          "iso6391Name": "es",
          "score": 0.9375
        }
      ]
    }
  ],
  "errors": []
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie sich mit Konzepten und mit dem Workflow für die Sprachenerkennung unter Verwendung der Textanalyse in Azure Cognitive Services vertraut gemacht. Die folgenden Punkte wurden erläutert und veranschaulicht:

+ Die [Sprachenerkennung](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) ist für eine Vielzahl von Sprachen, Varianten und Dialekten sowie einige Regional- und Kultursprachen verfügbar.
+ JSON-Dokumente im Anforderungstext umfassen eine ID und Text.
+ Die POST-Anforderung wird an einen Endpunkt vom Typ `/languages` gesendet. Dabei werden ein personalisierter [Zugriffsschlüssel und ein Endpunkt](text-analytics-how-to-access-key.md) verwendet, der für Ihr Abonnement gültig ist.
+ Die Antwortausgabe besteht aus Sprachen-IDs für die jeweilige Dokument-ID. Die Ausgabe kann an eine beliebige JSON-fähige App gestreamt werden. Zu den Beispiel-Apps zählen u. a. Excel und Power BI.

## <a name="see-also"></a>Weitere Informationen 

 [Übersicht über die Textanalyse](../overview.md)  
 [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)</br>
 [Textanalysen (Produktseite)](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren von Stimmungen](text-analytics-how-to-sentiment-analysis.md)
