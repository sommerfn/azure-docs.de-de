---
title: Schlüsselbegriffsextraktion mit der Textanalyse-REST-API
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mithilfe der Textanalyse-REST-API von Azure Cognitive Services Schlüsselbegriffe extrahieren.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 07/29/2019
ms.author: raymondl
ms.openlocfilehash: ec5ff756d7e732430675676868bc754627a2a4a1
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72429027"
---
# <a name="example-how-to-extract-key-phrases-using-text-analytics"></a>Beispiel: Erkennen von Schlüsselbegriffen mithilfe der Textanalyse

Die [Schlüsselbegriffserkennungs-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6) bewertet unstrukturierten Text und gibt für jedes JSON-Dokument eine Liste mit Schlüsselbegriffen zurück.

Diese Funktion ist nützlich, wenn Sie die wichtigsten Punkte in einer Sammlung von Dokumenten schnell identifizieren müssen. Wenn der eingegebene Text beispielsweise „Das Essen war köstlich, und es gab hervorragendes Personal“ lautet, gibt der Dienst die Kernpunkte „Essen“ und „hervorragendes Personal“ zurück.

Weitere Informationen finden Sie unter [Unterstützte Sprachen](../text-analytics-supported-languages.md).

> [!TIP]
> Die Textanalyse bietet darüber hinaus ein Linux-basiertes Docker-Containerimage für die Schlüsselbegriffserkennung, damit Sie [den Textanalysecontainer nah bei Ihren Daten installieren und ausführen können](text-analytics-how-to-install-containers.md).

## <a name="preparation"></a>Vorbereitung

Die Schlüsselbegriffserkennung funktioniert am besten, wenn Sie ihr größere Texte zur Verarbeitung übergeben. Dies steht im Gegensatz zur Standpunktanalyse, die mit kleineren Texten besser funktioniert. Um für beide Vorgänge optimale Ergebnisse zu erzielen, empfiehlt es sich ggf., die Eingaben entsprechend umzustrukturieren.

Sie benötigen JSON-Dokumente im folgenden Format: ID, Text, Sprache.

Die Dokumentgröße darf 5.120 Zeichen pro Dokument nicht übersteigen, und pro Sammlung sind bis zu 1.000 Elemente (IDs) zulässig. Die Sammlung wird im Hauptteil der Anforderung übermittelt. Das folgende Beispiel ist eine Abbildung von Inhalten, die Sie zur Schlüsselbegriffserkennung übermitteln könnten.

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

Weitere Informationen zur Anforderungsdefinition finden Sie unter [Aufrufen der Textanalyse-API](text-analytics-how-to-call-api.md). Der Einfachheit halber sind hier noch einmal einige Punkte aufgeführt:

+ Erstellen Sie eine Anforderung vom Typ **POST**. Lesen Sie die API-Dokumentation für diese Anforderung: [Schlüsselbegriffs-API:](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6)

+ Legen Sie den HTTP-Endpunkt für die Schlüsselbegriffsextraktion entweder mithilfe einer Textanalyseressource in Azure oder mithilfe eines instanziierten [Textanalysecontainers](text-analytics-how-to-install-containers.md) fest. Sie müssen `/text/analytics/v2.1/keyPhrases` in die URL einschließen. Beispiel: `https://<your-custom-subdomain>.api.cognitiveservices.azure.com/text/analytics/v2.1/keyPhrases`.

+ Legen Sie einen Anforderungsheader fest, der den [Zugriffsschlüssel](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) für Textanalysevorgänge enthält.

+ Geben Sie im Anforderungstext die JSON-Dokumentsammlung an, die Sie für diese Analyse vorbereitet haben.

> [!Tip]
> Verwenden Sie [Postman](text-analytics-how-to-call-api.md), oder öffnen Sie die **API-Testkonsole** in der [Dokumentation](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6), um eine Anforderung zu strukturieren und mittels POST an den Dienst zu übermitteln.

## <a name="step-2-post-the-request"></a>Schritt 2: Übermitteln der Anforderung

Die Analyse erfolgt, wenn die Anforderung eingeht. Informationen zur Größe und Anzahl von Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie in der Übersicht im Abschnitt [Datengrenzwerte](../overview.md#data-limits).

Vergessen Sie nicht, dass der Dienst zustandslos ist. In Ihrem Konto werden keine Daten gespeichert. Die Ergebnisse werden direkt in der Antwort zurückgegeben.

## <a name="step-3-view-results"></a>Schritt 3: Anzeigen der Ergebnisse

Alle POST-Anforderungen geben eine Antwort im JSON-Format mit den IDs und erkannten Eigenschaften zurück. Die Reihenfolge der zurückgegebenen Schlüsselbegriffe wird vom Modell intern bestimmt.

Die Ausgabe wird umgehend zurückgegeben. Sie können die Ergebnisse an eine Anwendung streamen, die JSON akzeptiert, oder die Ausgabe in einer Datei auf dem lokalen System speichern und sie anschließend in eine Anwendung importieren, in der Sie die Daten sortieren, durchsuchen und bearbeiten können.

Beispiel für die Ausgabe der Schlüsselbegriffserkennung:

```json
    {
        "documents": [
            {
                "keyPhrases": [
                    "year",
                    "trail",
                    "trip",
                    "views"
                ],
                "id": "1"
            },
            {
                "keyPhrases": [
                    "marked trails",
                    "Worst hike",
                    "goners"
                ],
                "id": "2"
            },
            {
                "keyPhrases": [
                    "trail",
                    "small children",
                    "family"
                ],
                "id": "3"
            },
            {
                "keyPhrases": [
                    "spectacular views",
                    "trail",
                    "area"
                ],
                "id": "4"
            },
            {
                "keyPhrases": [
                    "places",
                    "beautiful views",
                    "favorite trail"
                ],
                "id": "5"
            }
        ],
        "errors": []
    }
```

Wie bereits erwähnt, sucht das Analysetool nach unbedeutenden Wörter, verwirft sie und behält einzelne Begriffe oder Ausdrücke bei, die offenbar Subjekt oder Objekt eines Satzes sind.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie sich mit Konzepten und dem Workflow für die Extraktion von Schlüsselbegriffen unter Verwendung der Textanalyse in Cognitive Services vertraut gemacht. Zusammenfassung:

+ Die [Schlüsselbegriffserkennungs-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6) ist für ausgewählte Sprachen verfügbar.
+ JSON-Dokumente im Anforderungstext umfassen eine ID, Text und einen Sprachcode.
+ Die POST-Anforderung wird an einen Endpunkt vom Typ `/keyphrases` gesendet. Dabei werden ein personalisierter [Zugriffsschlüssel und ein Endpunkt](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) verwendet, der für Ihr Abonnement gültig ist.
+ Bei der Antwortausgabe handelt es sich um Schlüsselwörter und -begriffe für die jeweilige Dokument-ID. Sie kann an eine beliebige JSON-fähige App gestreamt werden – beispielsweise an Microsoft Office Excel oder Power BI.

## <a name="see-also"></a>Weitere Informationen

 [Übersicht über die Textanalyse](../overview.md) [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)</br>
 [Textanalysen (Produktseite)](//go.microsoft.com/fwlink/?LinkID=759712)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Textanalyse-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c6)
