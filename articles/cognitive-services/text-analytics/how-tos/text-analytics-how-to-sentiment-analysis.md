---
title: Standpunktanalyse unter Verwendung der Textanalyse von Azure Cognitive Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe der Textanalyse-REST-API Standpunkte ermitteln.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: d4269a99a8e535692e4897630a7edd9b27347d41
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304032"
---
# <a name="example-how-to-detect-sentiment-with-text-analytics"></a>Beispiel: Ermitteln von Standpunkten mit der Textanalyse

Die [Standpunktanalyse-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) wertet eine Texteingabe aus und gibt für jedes Dokument eine Stimmungspunktzahl zwischen 0 (negativ) und 1 (positiv) zurück.

Diese Funktion ermöglicht die Erkennung positiver und negativer Standpunkte in sozialen Medien, Kundenbewertungen und Diskussionsforen. Sie stellen die Inhalte bereit, der Dienst liefert die Modelle und Trainingsdaten.

Aktuell wird die Standpunktanalyse für Englisch, Deutsch, Spanisch und Französisch unterstützt. Andere Sprachen befinden sich in der Vorschauphase. Weitere Informationen finden Sie unter [Unterstützte Sprachen](../text-analytics-supported-languages.md).

> [!TIP]
> Die Textanalyse bietet darüber hinaus ein Linux-basiertes Docker-Containerimage für die Standpunktanalyse, damit Sie [den Textanalysecontainer nah bei Ihren Daten installieren und ausführen können](text-analytics-how-to-install-containers.md).

## <a name="concepts"></a>Konzepte

Die Textanalyse generiert mithilfe eines Machine Learning-Klassifizierungsalgorithmus eine Stimmungspunktzahl zwischen 0 und 1. Eine gegen 1 tendierende Punktzahl deutet auf einen positiven Standpunkt hin, eine gegen 0 tendierende Punktzahl auf einen negativen. Das Modell wurde vorab mit umfangreichen standpunktbezogenen Texten trainiert. Eigene Trainingsdaten können derzeit nicht bereitgestellt werden. Das Modell verwendet bei der Textanalyse eine Kombination aus verschiedenen Verfahren. Hierzu zählen etwa Textverarbeitung, Wortartanalyse, Wortanordnung und Wortassoziationen. Weitere Informationen zum Algorithmus finden Sie unter [Introducing Text Analytics in the Azure ML Marketplace](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/) (Vorstellung der Textanalyse im Azure Machine Learning-Marketplace).

Die Standpunktanalyse wird für das gesamte Dokument durchgeführt, nicht für eine bestimmte Entität im Text. In der Praxis verbessert sich tendenziell die Genauigkeit der Bewertung, wenn Dokumente einen oder zwei Sätze enthalten anstatt einen großen Textblock. Im Rahmen einer Objektivitätsbewertungsphase bestimmt das Modell, ob ein Dokument insgesamt objektiv ist oder einen Standpunkt enthält. Für ein überwiegend objektives Dokument wird keine Standpunkterkennungsphase gestartet: Es erhält die Punktzahl 0,50 und wird nicht weiter verarbeitet. Für Dokumente, die die Pipeline weiter durchlaufen, wird in der nächsten Phase abhängig vom im Dokument erkannten Standpunkt eine Punktzahl generiert, die über oder unter 0,50 liegt.

## <a name="preparation"></a>Vorbereitung

Die Standpunktanalyse liefert bessere Ergebnisse, wenn Sie ihr kleinere Textblöcke zuführen. Bei der Schlüsselbegriffserkennung verhält es sich genau umgekehrt: Sie funktioniert besser, wenn sie für große Textblöcke durchgeführt wird. Um für beide Vorgänge optimale Ergebnisse zu erzielen, empfiehlt es sich ggf., die Eingaben entsprechend umzustrukturieren.

Sie benötigen JSON-Dokumente im folgenden Format: ID, Text, Sprache.

Die Dokumentgröße darf 5.120 Zeichen pro Dokument nicht übersteigen, und pro Sammlung sind bis zu 1.000 Elemente (IDs) zulässig. Die Sammlung wird im Hauptteil der Anforderung übermittelt. Das folgende Beispiel zeigt Inhalte, die Sie ggf. für die Standpunktanalyse übermitteln können:

```
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

Details zur Anforderungsdefinition finden Sie unter [Aufrufen der Textanalyse-REST-API](text-analytics-how-to-call-api.md). Der Einfachheit halber sind hier noch einmal einige Punkte aufgeführt:

+ Erstellen Sie eine Anforderung vom Typ **POST**. Lesen Sie die API-Dokumentation für diese Anforderung: [Standpunktanalyse-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9)

+ Legen Sie den HTTP-Endpunkt für die Standpunktanalyse entweder mithilfe einer Textanalyseressource in Azure oder mithilfe eines instanziierten [Textanalysecontainers](text-analytics-how-to-install-containers.md) fest. Er muss die Ressource `/sentiment` enthalten: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment`.

+ Legen Sie einen Anforderungsheader fest, der den Zugriffsschlüssel für Textanalysevorgänge enthält. Weitere Informationen finden Sie unter [How to find endpoints and access keys](text-analytics-how-to-access-key.md) (Ermitteln von Endpunkten und Zugriffsschlüsseln).

+ Geben Sie im Anforderungstext die JSON-Dokumentsammlung an, die Sie für diese Analyse vorbereitet haben.

> [!Tip]
> Verwenden Sie [Postman](text-analytics-how-to-call-api.md), oder öffnen Sie die **API-Testkonsole** in der [Dokumentation](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9), um die Anforderung zu strukturieren und mittels POST an den Dienst zu übermitteln.

## <a name="step-2-post-the-request"></a>Schritt 2: Übermitteln der Anforderung

Die Analyse erfolgt, wenn die Anforderung eingeht. Weitere Informationen zur Größe und Anzahl von Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie in der Übersicht im Abschnitt [Datengrenzwerte](../overview.md#data-limits).

Vergessen Sie nicht, dass der Dienst zustandslos ist. In Ihrem Konto werden keine Daten gespeichert. Die Ergebnisse werden direkt in der Antwort zurückgegeben.


## <a name="step-3-view-results"></a>Schritt 3: Anzeigen der Ergebnisse

Die Standpunktanalyse klassifiziert den Text als vorwiegend positiv oder negativ. Hierzu wird dem Text eine Punktzahl zwischen 0 und 1 zugewiesen. Gegen 0,5 tendierende Werte sind neutral oder ungewiss. Die Punktzahl 0,5 steht für Neutralität. Wenn für eine Zeichenfolge keine Standpunktanalyse möglich ist oder eine Zeichenfolge keinen Standpunkt beinhaltet, ist die Punktzahl immer genau 0,5. Wenn Sie also beispielsweise eine spanische Zeichenfolge mit einem Sprachcode für Englisch übergeben, ist die Punktzahl 0,5.

Die Ausgabe wird umgehend zurückgegeben. Sie können die Ergebnisse an eine Anwendung streamen, die JSON akzeptiert, oder die Ausgabe in einer Datei auf dem lokalen System speichern und sie anschließend in eine Anwendung importieren, in der Sie die Daten sortieren, durchsuchen und bearbeiten können.

Das folgende Beispiel zeigt die Antwort für die Dokumentsammlung in diesem Artikel:

```
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

In diesem Artikel haben Sie sich mit Konzepten und mit dem Workflow für die Standpunktanalyse unter Verwendung der Textanalyse in Cognitive Services vertraut gemacht. Zusammenfassung:

+ Die [Standpunktanalyse-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) ist für ausgewählte Sprachen verfügbar.
+ JSON-Dokumente im Anforderungstext umfassen eine ID, Text und einen Sprachcode.
+ Die POST-Anforderung wird an einen Endpunkt vom Typ `/sentiment` gesendet. Dabei werden ein personalisierter [Zugriffsschlüssel und ein Endpunkt](text-analytics-how-to-access-key.md) verwendet, der für Ihr Abonnement gültig ist.
+ Bei der Antwortausgabe handelt es sich um eine Stimmungspunktzahl für die jeweilige Dokument-ID. Sie kann an eine beliebige JSON-fähige App gestreamt werden (beispielsweise an Excel oder Power BI).

## <a name="see-also"></a>Weitere Informationen 

 [Übersicht über die Textanalyse](../overview.md)  
 [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)</br>
 [Textanalysen (Produktseite)](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [How to extract key phrases in Text Analytics](text-analytics-how-to-keyword-extraction.md) (Extrahieren von Schlüsselbegriffen mithilfe der Textanalyse)
