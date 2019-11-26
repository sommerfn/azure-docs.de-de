---
title: Ausführen von Standpunktanalysen mit der Textanalyse-REST-API
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie die Textanalyse-REST-API von Azure Cognitive Services verwenden können, um einen Standpunkt (Stimmung) in Text zu erkennen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 09/23/2019
ms.author: aahi
ms.openlocfilehash: f7a62d1d2b695726421ba990d0f118eb3ae199da
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74031461"
---
# <a name="example-detect-sentiment-with-text-analytics"></a>Beispiel: Ermitteln von Standpunkten mit der Textanalyse

Die [Azure Standpunktanalyse-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) wertet eine Texteingabe aus und gibt für jedes Dokument eine Stimmungspunktzahl zurück. Die Werte liegen zwischen 0 (negativ) und 1 (positiv).

Diese Funktion ermöglicht die Erkennung positiver und negativer Standpunkte in sozialen Medien, Kundenbewertungen und Diskussionsforen. Der Inhalt wird von Ihnen bereitgestellt. Der Dienst liefert die Modelle und Trainingsdaten.

Aktuell unterstützt die Standpunktanalyse-API Englisch, Deutsch, Spanisch und Französisch. Andere Sprachen befinden sich in der Vorschauphase. Weitere Informationen finden Sie unter [Unterstützte Sprachen](../text-analytics-supported-languages.md).

> [!TIP]
> Die Azure Textanalyse-API bietet darüber hinaus ein Linux-basiertes Docker-Containerimage für die Standpunktanalyse, damit Sie [den Textanalysecontainer nah bei Ihren Daten installieren und ausführen können](text-analytics-how-to-install-containers.md).

## <a name="concepts"></a>Konzepte

Die Textanalyse generiert mithilfe eines Machine Learning-Klassifizierungsalgorithmus eine Stimmungspunktzahl zwischen 0 und 1. Eine gegen 1 tendierende Punktzahl deutet auf einen positiven Standpunkt hin, eine gegen 0 tendierende Punktzahl auf einen negativen. Das Modell wurde vorab mit umfangreichen standpunktbezogenen Texten trainiert. Eigene Trainingsdaten können derzeit nicht bereitgestellt werden. Das Modell verwendet während der Textanalyse eine Kombination aus verschiedenen Techniken. Zu den Techniken gehören Textverarbeitung, Sprachanalyse, Wortplatzierung und Wortzuordnungen. Weitere Informationen zum Algorithmus finden Sie unter [Introducing Text Analytics in the Azure ML Marketplace](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/) (Vorstellung der Textanalyse im Azure Machine Learning-Marketplace).

Die Standpunktanalyse wird für das gesamte Dokument durchgeführt, nicht für eine bestimmte Entität im Text. In der Praxis verbessert sich tendenziell die Genauigkeit der Bewertung, wenn Dokumente einen oder zwei Sätze enthalten anstatt einen großen Textblock. Im Rahmen einer Objektivitätsbewertungsphase bestimmt das Modell, ob ein Dokument insgesamt objektiv ist oder einen Standpunkt enthält. Für ein überwiegend objektives Dokument wird keine Standpunkterkennungsphase gestartet: Es erhält die Punktzahl 0,50 und wird nicht weiter verarbeitet. Für Dokumente, die die Pipeline weiter durchlaufen, generiert die nächste Phase eine Bewertung über oder unter 0,50. Das Ergebnis hängt vom im Dokument erkannten Grad der Stimmung ab.

## <a name="sentiment-analysis-v3-public-preview"></a>Öffentliche Vorschau der Standpunktanalyse v3

Die [nächste Version der Standpunktanalyse](https://cognitiveusw2ppe.portal.azure-api.net/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment) ist nun als öffentliche Vorschauversion verfügbar. Sie bietet deutliche Verbesserungen hinsichtlich der Genauigkeit und Details der Textkategorisierung und -bewertung der API.

> [!NOTE]
> * Anforderungsformat und [Datenlimits](../overview.md#data-limits) der Standpunktanalyse v3 haben sich gegenüber der vorherigen Version nicht geändert.
> * Zurzeit gilt Folgendes für die Standpunktanalyse v3:
>    * Unterstützt derzeit Englisch (`en`), Japanisch (`ja`), vereinfachtes Chinesisch (`zh-Hans`), traditionelles Chinesisch (`zh-Hant`), Französisch (`fr`), Italienisch (`it`), Spanisch (`es`), Niederländisch (`nl`), Portugiesisch (`pt`) und Deutsch (`de`).
>    * Sie ist in den folgenden Regionen verfügbar: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe` und `West US 2`.

|Feature |BESCHREIBUNG  |
|---------|---------|
|Höhere Genauigkeit     | Deutliche Verbesserung bei der Erkennung positiver, neutraler, negativer und gemischter Stimmungen in Textdokumenten gegenüber früheren Versionen.           |
|Stimmungsbewertung auf Dokument- und Satzebene     | Erkennen der Stimmung eines Dokuments und der darin enthaltenen Sätze. Wenn das Dokument mehrere Sätze enthält, wird jedem einzelnen Satz eine Stimmungsbewertung zugeordnet.         |
|Bezeichnung und Bewertung von Stimmungen     | Die API gibt neben der Stimmungsbewertung jetzt auch Stimmungskategorien für einen Text zurück. Diese Kategorien sind `positive`, `negative`, `neutral` und `mixed`.       |
| Verbesserte Ausgabe | Die Standpunktanalyse gibt jetzt Informationen für ein vollständiges Textdokument und für die darin enthaltenen einzelnen Sätze zurück. |
| Modellversionsparameter | Ein optionaler Parameter, mit dem Sie auswählen, welche Version des Textanalysemodells für Ihre Daten verwendet werden soll. |

### <a name="sentiment-labeling"></a>Bezeichnung von Stimmungen

Die Standpunktanalyse v3 kann Bewertungen und Bezeichnungen auf Satz- und Dokumentebene zurückgeben. Die Bewertungen und Bezeichnungen sind `positive`, `negative`und `neutral`. Auf Dokumentebene kann auch die Stimmungsbezeichnung `mixed` (keine Bewertung) zurückgegeben werden. Die Stimmung eines Dokuments wird durch Zusammenfassen der Bewertungen der einzelnen Sätze bestimmt.

| Stimmung von Sätzen                                                        | Zurückgegebene Dokumentbezeichnung |
|---------------------------------------------------------------------------|----------------|
| Mindestens ein positiver Satz, alle anderen Sätze sind neutrale. | `positive`     |
| Mindestens ein negativer Satz, alle anderen Sätze sind neutrale.  | `negative`     |
| Mindestens ein negativer und mindestens ein positiver Satz.         | `mixed`        |
| Alle Sätze sind neutral.                                                 | `neutral`      |

### <a name="model-versioning"></a>Versionsverwaltung der Modelle

> [!NOTE]
> Die Modellversionsverwaltung für die Stimmungsanalyse ist ab Version `v3.0-preview.1` verfügbar.

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

### <a name="sentiment-analysis-v3-example-request"></a>Beispielanforderung der Standpunktanalyse v3

Der folgende JSON-Code ist ein Beispiel für eine Anforderung, die an die neue Version der Standpunktanalyse gesendet wurde. Die Formatierung der Anforderung ist mit der vorherigen Version identisch:

```json
    {
        "documents": [
        {
            "language": "en",
            "id": "1",
            "text": "Hello world. This is some input text that I love."
        },
        {
            "language": "en",
            "id": "2",
            "text": "It's incredibly sunny outside! I'm so happy."
        }
        ],
    }
```

### <a name="sentiment-analysis-v3-example-response"></a>Beispielantwort der Standpunktanalyse v3

Während das Anforderungsformat das gleiche ist wie in der vorherigen Version, hat sich das Antwortformat geändert. Der folgende JSON-Code ist eine Beispielantwort der neuen API-Version:

```json
    {
        "documents": [
            {
                "id": "1",
                "sentiment": "positive",
                "documentScores": {
                    "positive": 0.98570585250854492,
                    "neutral": 0.0001625834556762,
                    "negative": 0.0141316400840878
                },
                "sentences": [
                    {
                        "sentiment": "neutral",
                        "sentenceScores": {
                            "positive": 0.0785155147314072,
                            "neutral": 0.89702343940734863,
                            "negative": 0.0244610067456961
                        },
                        "offset": 0,
                        "length": 12
                    },
                    {
                        "sentiment": "positive",
                        "sentenceScores": {
                            "positive": 0.98570585250854492,
                            "neutral": 0.0001625834556762,
                            "negative": 0.0141316400840878
                        },
                        "offset": 13,
                        "length": 36
                    }
                ]
            },
            {
                "id": "2",
                "sentiment": "positive",
                "documentScores": {
                    "positive": 0.89198976755142212,
                    "neutral": 0.103382371366024,
                    "negative": 0.0046278294175863
                },
                "sentences": [
                    {
                        "sentiment": "positive",
                        "sentenceScores": {
                            "positive": 0.78401315212249756,
                            "neutral": 0.2067587077617645,
                            "negative": 0.0092281140387058
                        },
                        "offset": 0,
                        "length": 30
                    },
                    {
                        "sentiment": "positive",
                        "sentenceScores": {
                            "positive": 0.99996638298034668,
                            "neutral": 0.0000060341349126,
                            "negative": 0.0000275444017461
                        },
                        "offset": 31,
                        "length": 13
                    }
                ]
            }
        ],
        "errors": []
    }
```

### <a name="example-c-code"></a>C#-Beispielcode

Eine C#-Beispielanwendung, die diese Version der Standpunktanalyse aufruft, finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/dotnet/Language/SentimentV3.cs).

## <a name="preparation"></a>Vorbereitung

Die Standpunktanalyse liefert bessere Ergebnisse, wenn Sie ihr kleinere Textblöcke zuführen. Bei der Schlüsselbegriffserkennung verhält es sich genau umgekehrt: Sie funktioniert besser, wenn sie für große Textblöcke durchgeführt wird. Um für beide Vorgänge optimale Ergebnisse zu erzielen, empfiehlt es sich ggf., die Eingaben entsprechend umzustrukturieren.

Sie benötigen JSON-Dokumente im folgenden Format: ID, Text und Sprache.

Ein Dokument darf maximal 5.120 Zeichen enthalten. Pro Sammlung können bis zu 1.000 Elemente (IDs) vorhanden sein. Die Sammlung wird im Hauptteil der Anforderung übermittelt. Das folgende Beispiel zeigt Inhalte, die Sie ggf. für die Standpunktanalyse übermitteln können:

```json
    {
        "documents": [
            {
                "language": "en",
                "id": "1",
                "text": "We love this trail and make the trip every year. The views are breathtaking and well worth the hike!"
            },
            {
                "language": "en",
                "id": "2",
                "text": "Poorly marked trails! I thought we were goners. Worst hike ever."
            },
            {
                "language": "en",
                "id": "3",
                "text": "Everyone in my family liked the trail but thought it was too challenging for the less athletic among us. Not necessarily recommended for small children."
            },
            {
                "language": "en",
                "id": "4",
                "text": "It was foggy so we missed the spectacular views, but the trail was ok. Worth checking out if you are in the area."
            },
            {
                "language": "en",
                "id": "5",
                "text": "This is my favorite trail. It has beautiful views and many places to stop and rest"
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>Schritt 1: Strukturieren der Anforderung

Weitere Informationen zur Anforderungsdefinition finden Sie unter [Aufrufen der Textanalyse-REST-API](text-analytics-how-to-call-api.md). Der Einfachheit halber sind hier noch einmal einige Punkte aufgeführt:

+ Erstellen Sie eine POST-Anforderung. Die API-Dokumentation für diese Anforderung finden Sie unter [Standpunktanalyse-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9).

+ Legen Sie den HTTP-Endpunkt für die Standpunktanalyse entweder mithilfe einer Textanalyseressource in Azure oder mithilfe eines instanziierten [Textanalysecontainers](text-analytics-how-to-install-containers.md) fest. Sie müssen `/text/analytics/v2.1/sentiment` in die URL einschließen. Beispiel: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/sentiment`.

+ Legen Sie einen Anforderungsheader fest, der den [Zugriffsschlüssel](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) für Textanalysevorgänge enthält.

+ Geben Sie im Anforderungstext die JSON-Dokumentsammlung an, die Sie für diese Analyse vorbereitet haben.

> [!Tip]
> Verwenden Sie [Postman](text-analytics-how-to-call-api.md), oder öffnen Sie die **API-Testkonsole** in der [Dokumentation](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9), um die Anforderung zu strukturieren und an den Dienst zu übermitteln.

## <a name="step-2-post-the-request"></a>Schritt 2: Übermitteln der Anforderung

Die Analyse erfolgt, wenn die Anforderung eingeht. Informationen zur Größe und Anzahl von Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie in der Übersicht im Abschnitt [Datengrenzwerte](../overview.md#data-limits).

Vergessen Sie nicht, dass der Dienst zustandslos ist. In Ihrem Konto werden keine Daten gespeichert. Die Ergebnisse werden direkt in der Antwort zurückgegeben.


## <a name="step-3-view-the-results"></a>Schritt 3: Zeigen Sie die Ergebnisse an

Die Standpunktanalyse klassifiziert den Text als vorwiegend positiv oder negativ. Hierzu wird dem Text eine Punktzahl zwischen 0 und 1 zugewiesen. Gegen 0,5 tendierende Werte sind neutral oder ungewiss. Die Punktzahl 0,5 steht für Neutralität. Wenn für eine Zeichenfolge keine Standpunktanalyse möglich ist oder eine Zeichenfolge keinen Standpunkt beinhaltet, ist die Punktzahl immer genau 0,5. Wenn Sie also beispielsweise eine spanische Zeichenfolge mit einem Sprachcode für Englisch übergeben, ist die Punktzahl 0,5.

Die Ausgabe wird umgehend zurückgegeben. Sie können die Ergebnisse an eine Anwendung streamen, die JSON akzeptiert, oder die Ausgabe in einer Datei im lokalen System speichern. Importieren Sie dann die Ausgabe in eine Anwendung, mit der Sie die Daten sortieren, durchsuchen und bearbeiten können.

Das folgende Beispiel zeigt die Antwort für die Dokumentsammlung in diesem Artikel:

```json
    {
        "documents": [
            {
                "score": 0.9999237060546875,
                "id": "1"
            },
            {
                "score": 0.0000540316104888916,
                "id": "2"
            },
            {
                "score": 0.99990355968475342,
                "id": "3"
            },
            {
                "score": 0.980544924736023,
                "id": "4"
            },
            {
                "score": 0.99996328353881836,
                "id": "5"
            }
        ],
        "errors": []
    }
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie sich mit Konzepten und mit dem Workflow für die Standpunktanalyse unter Verwendung der Textanalyse in Azure Cognitive Services vertraut gemacht. Zusammenfassung:

+ Die [Standpunktanalyse-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) ist für ausgewählte Sprachen verfügbar.
+ JSON-Dokumente im Anforderungstext umfassen eine ID, Text und einen Sprachcode.
+ Die POST-Anforderung wird an einen Endpunkt vom Typ `/sentiment` gesendet. Dabei werden ein personalisierter [Zugriffsschlüssel und ein Endpunkt](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) verwendet, der für Ihr Abonnement gültig ist.
+ Bei der Antwortausgabe handelt es sich um eine Stimmungspunktzahl für die jeweilige Dokument-ID. Sie kann an eine beliebige JSON-fähige App gestreamt werden. Zu den Beispiel-Apps zählen Excel und Power BI, um nur einige zu nennen.

## <a name="see-also"></a>Weitere Informationen

 [Übersicht über die Textanalyse](../overview.md) [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)</br>
 [Textanalysen (Produktseite)](//go.microsoft.com/fwlink/?LinkID=759712)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [How to extract key phrases in Text Analytics](text-analytics-how-to-keyword-extraction.md) (Extrahieren von Schlüsselbegriffen mithilfe der Textanalyse)
