---
title: Verwenden der Entitätserkennung mit der Textanalyse-API
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mithilfe der Textanalyse-REST-API Entitäten erkennen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 10/21/2019
ms.author: aahi
ms.openlocfilehash: e51e5945df8b08ec81db0c85416b31b3ec788ffd
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488647"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Verwenden der Erkennung benannter Entitäten in der Textanalyse

Der [API für die Erkennung benannter Entitäten](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) wird unstrukturierter Text übergeben. Für jedes JSON-Dokument werden eine Liste mit eindeutig unterscheidbaren Entitäten und Links zu weiteren Informationen im Web (Wikipedia und Bing) zurückgegeben.

## <a name="entity-linking-and-named-entity-recognition"></a>Entitätsverknüpfung und Erkennung benannter Entitäten

Der Textanalyseendpunkt `entities` unterstützt sowohl die Erkennung benannter Entitäten (Named Entity Recognition, NER) als auch die Entitätsverknüpfung.

### <a name="entity-linking"></a>Entitätsverknüpfung
Die Entitätsverknüpfung ist die Möglichkeit, die Identität einer im Text gefundenen Entität zu identifizieren und eindeutig zu machen (beispielsweise die Ermittlung, ob „Mars“ als Planet oder als römischer Kriegsgott verwendet wird). Für diesen Prozess ist das Vorhandensein einer Knowledge Base erforderlich, mit der erkannte Entitäten verknüpft sind. Wikipedia wird als Knowledge Base für den `entities`-Endpunkt der Textanalyse verwendet.

### <a name="named-entity-recognition-ner"></a>Erkennung benannter Entitäten (NER)
Die Erkennung benannter Entitäten (Named Entity Recognition, NER) ist die Möglichkeit, unterschiedliche Entitäten im Text zu identifizieren und sie in vordefinierte Klassen oder Typen zu kategorisieren. 

## <a name="named-entity-recognition-v3-public-preview"></a>Named Entity Recognition v3 – öffentliche Vorschau

Die [nächste Version der Named Entity Recognition](https://cognitiveusw2ppe.portal.azure-api.net/docs/services/TextAnalytics-v3-0-Preview-1/operations/56f30ceeeda5650db055a3c7/console) steht jetzt als öffentliche Vorschauversion zur Verfügung. Sie enthält Updates sowohl für die Entitätsverknüpfung als auch für die Erkennung benannter Entitäten. 

:::row:::
    :::column span="":::
        **Feature**
    :::column-end:::
    ::: column span="":::
        **Beschreibung** 
    :::column-end:::
:::row-end:::
<!-- expanded types and subtypes row-->
:::row:::
    :::column span="":::
        Erweiterte Entitätstypen und Untertypen
    :::column-end:::
    :::column span="":::
     Erweiterte Klassifizierung und Erkennung für mehrere benannte Entitätstypen.
    :::column-end:::
:::row-end:::
<!-- separate endpoints row-->
:::row:::
    :::column span="":::
        Separate Anforderungsendpunkte 
    :::column-end:::
    :::column span="":::
        Separate Endpunkte zum Senden von Entitätsverknüpfungs- und NER-Anforderungen.
    :::column-end:::
:::row-end:::
<!-- model-version row -->
:::row:::
    :::column span="":::
        `model-version`-Parameter
    :::column-end:::
    :::column span="":::
        Ein optionaler Parameter zum Auswählen einer Version des Textanalysemodells. Zurzeit steht nur das Standardmodell zur Verwendung zur Verfügung.
    :::column-end:::
:::row-end:::

### <a name="entity-types"></a>Entitätstypen

Named Entity Recognition v3 bietet erweiterte Erkennung über mehrere Typen hinweg. Aktuell kann NER v3 die folgenden Kategorien von Entitäten erkennen. Eine ausführliche Liste der unterstützten Entitäten und Sprachen finden Sie im Artikel [Benannte Entitätstypen](../named-entity-types.md).

* Allgemein
* Personenbezogene Informationen 

### <a name="request-endpoints"></a>Anforderungsendpunkte

Named Entity Recognition v3 verwendet separate Endpunkte für NER-Anforderungen und Entitätsverknüpfungsanforderungen. Verwenden Sie basierend auf Ihrer Anforderung ein URL-Format unten:

NER
* Allgemeine Entitäten: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/general`

* Entitäten für personenbezogene Informationen: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/pii`

Entitätsverknüpfung
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/linking`

### <a name="model-versioning"></a>Versionsverwaltung der Modelle

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

## <a name="supported-types-for-named-entity-recognition-v2"></a>Unterstützte Typen für Named Entity Recognition v2

> [!NOTE]
> Die folgenden Entitäten werden von Named Entity Recognition (NER) Version 2 unterstützt. [NER V3](#named-entity-recognition-v3-public-preview) befindet sich in der öffentlichen Vorschauphase und erweitert die Anzahl und Tiefe der in Text erkannten Entitäten erheblich.   

| type  | SubType | Beispiel |
|:-----------   |:------------- |:---------|
| Person        | N/V\*         | „Jeff“, „Bill Gates“     |
| Location      | N/V\*         | „Redmond, Washington“, „Paris“  |
| Organisation  | N/V\*         | „Microsoft“   |
| Menge      | Number        | „6“, „sechs“     |
| Menge      | Prozentsatz    | „50 %“, „fünfzig Prozent“|
| Menge      | Ordinal       | „2.“, „zweite“     |
| Menge      | Alter           | „90 Tage alt“, „30 Jahre alt“    |
| Menge      | Currency      | „€10,99“     |
| Menge      | Dimension     | „10 Kilometer“, „40 cm“     |
| Menge      | Temperatur   | „32 Grad“    |
| Datetime      | N/V\*         | „18:30 4. Februar 2012“      |
| Datetime      | Date          | „2. Mai 2017“ und „02/05/2017“   |
| Datetime      | Time          | „8:00“, „8 Uhr“  |
| Datetime      | DateRange     | „2. Mai bis 5. Mai“    |
| Datetime      | TimeRange     | „18: 00 Uhr bis 19 Uhr“     |
| Datetime      | Duration      | „1 Minute und 45 Sekunden“   |
| Datetime      | Set           | „jeden Dienstag“     |
| URL           | N/V\*         | "https:\//www.bing.com"    |
| Email         | N/V\*         | "support@contoso.com" |

\* Je nach Eingabe und extrahierten Entitäten können bestimmte Entitäten den `SubType` auslassen.  Alle aufgelisteten unterstützten Entitätstypen sind nur für Englisch, Chinesisch (vereinfacht), Französisch, Deutsch und Spanisch verfügbar.

### <a name="language-support"></a>Sprachunterstützung

Zum Verwenden der Entitätsverknüpfung in verschiedenen Sprachen ist die Nutzung einer entsprechenden Knowledge Base in jeder Sprache erforderlich. Für die Entitätsverknüpfung in der Textanalyse bedeutet dies, dass jede Sprache, die vom `entities`-Endpunkt unterstützt wird, über einen Link zum entsprechenden Wikipedia-Corpus dieser Sprache verfügt. Da die Größe der Corpora zwischen Sprachen variiert, ist zu erwarten, dass auch der Abruf der Funktionalität für die Entitätsverknüpfung variiert. Weitere Information finden Sie im Artikel zu den [unterstützten Sprachen](../language-support.md#sentiment-analysis-key-phrase-extraction-and-named-entity-recognition).

## <a name="preparation"></a>Vorbereitung

Sie benötigen JSON-Dokumente im folgenden Format: ID, Text, Sprache.

Informationen zu den derzeit unterstützten Sprachen finden Sie in [dieser Liste](../text-analytics-supported-languages.md).

Die Dokumentgröße darf 5.120 Zeichen pro Dokument nicht übersteigen, und pro Sammlung sind bis zu 1.000 Elemente (IDs) zulässig. Die Sammlung wird im Hauptteil der Anforderung übermittelt. Das folgende Beispiel enthält eine Darstellung von Inhalten, die Sie an die Entitätsverknüpfung übermitteln können.

```json
    {
        "documents": [
            {
                "id": "1",
                "language": "en",
                "text": "Jeff bought three dozen eggs because there was a 50% discount."
            },
            {
                "id": "2",
                "language": "en",
                "text": "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>Schritt 1: Strukturieren der Anforderung

Details zur Anforderungsdefinition finden Sie unter [Aufrufen der Textanalyse-REST-API](text-analytics-how-to-call-api.md). Der Einfachheit halber sind hier noch einmal einige Punkte aufgeführt:

+ Erstellen Sie eine Anforderung vom Typ **POST**. Lesen Sie die API-Dokumentation für diese Anforderung: [Entitäten-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

+ Legen Sie den HTTP-Endpunkt für die Schlüsselbegriffsextraktion entweder mithilfe einer Textanalyseressource in Azure oder mithilfe eines instanziierten [Textanalysecontainers](text-analytics-how-to-install-containers.md) fest. Sie müssen `/text/analytics/v2.1/entities` einschließen. Beispiel: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`.

+ Legen Sie einen Anforderungsheader fest, der den [Zugriffsschlüssel](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) für Textanalysevorgänge enthält.

+ Geben Sie im Anforderungstext die JSON-Dokumentsammlung an, die Sie für diese Analyse vorbereitet haben.

> [!Tip]
> Verwenden Sie [Postman](text-analytics-how-to-call-api.md), oder öffnen Sie die **API-Testkonsole** in der [Dokumentation](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634), um eine Anforderung zu strukturieren und mittels POST an den Dienst zu übermitteln.

## <a name="step-2-post-the-request"></a>Schritt 2: Übermitteln der Anforderung

Die Analyse erfolgt, wenn die Anforderung eingeht. Weitere Informationen zur Größe und Anzahl von Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie in der Übersicht im Abschnitt [Datengrenzwerte](../overview.md#data-limits).

Vergessen Sie nicht, dass der Dienst zustandslos ist. In Ihrem Konto werden keine Daten gespeichert. Die Ergebnisse werden direkt in der Antwort zurückgegeben.

## <a name="step-3-view-results"></a>Schritt 3: Anzeigen der Ergebnisse

Alle POST-Anforderungen geben eine Antwort im JSON-Format mit den IDs und erkannten Eigenschaften zurück.

Die Ausgabe wird umgehend zurückgegeben. Sie können die Ergebnisse an eine Anwendung streamen, die JSON akzeptiert, oder die Ausgabe in einer Datei auf dem lokalen System speichern und sie anschließend in eine Anwendung importieren, in der Sie die Daten sortieren, durchsuchen und bearbeiten können.

Als Nächstes wird ein Beispiel für die Ausgabe der Entitätsverknüpfung angegeben:

```json
    {
        "Documents": [
            {
                "Id": "1",
                "Entities": [
                    {
                        "Name": "Jeff",
                        "Matches": [
                            {
                                "Text": "Jeff",
                                "Offset": 0,
                                "Length": 4
                            }
                        ],
                        "Type": "Person"
                    },
                    {
                        "Name": "three dozen",
                        "Matches": [
                            {
                                "Text": "three dozen",
                                "Offset": 12,
                                "Length": 11
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "50",
                        "Matches": [
                            {
                                "Text": "50",
                                "Offset": 49,
                                "Length": 2
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "50%",
                        "Matches": [
                            {
                                "Text": "50%",
                                "Offset": 49,
                                "Length": 3
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Percentage"
                    }
                ]
            },
            {
                "Id": "2",
                "Entities": [
                    {
                        "Name": "Great Depression",
                        "Matches": [
                            {
                                "Text": "The Great Depression",
                                "Offset": 0,
                                "Length": 20
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "Great Depression",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/Great_Depression",
                        "BingId": "d9364681-98ad-1a66-f869-a3f1c8ae8ef8"
                    },
                    {
                        "Name": "1929",
                        "Matches": [
                            {
                                "Text": "1929",
                                "Offset": 30,
                                "Length": 4
                            }
                        ],
                        "Type": "DateTime",
                        "SubType": "DateRange"
                    },
                    {
                        "Name": "By 1933",
                        "Matches": [
                            {
                                "Text": "By 1933",
                                "Offset": 36,
                                "Length": 7
                            }
                        ],
                        "Type": "DateTime",
                        "SubType": "DateRange"
                    },
                    {
                        "Name": "Gross domestic product",
                        "Matches": [
                            {
                                "Text": "GDP",
                                "Offset": 49,
                                "Length": 3
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "Gross domestic product",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/Gross_domestic_product",
                        "BingId": "c859ed84-c0dd-e18f-394a-530cae5468a2"
                    },
                    {
                        "Name": "United States",
                        "Matches": [
                            {
                                "Text": "America",
                                "Offset": 56,
                                "Length": 7
                            }
                        ],
                        "WikipediaLanguage": "en",
                        "WikipediaId": "United States",
                        "WikipediaUrl": "https://en.wikipedia.org/wiki/United_States",
                        "BingId": "5232ed96-85b1-2edb-12c6-63e6c597a1de",
                        "Type": "Location"
                    },
                    {
                        "Name": "25",
                        "Matches": [
                            {
                                "Text": "25",
                                "Offset": 72,
                                "Length": 2
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Number"
                    },
                    {
                        "Name": "25%",
                        "Matches": [
                            {
                                "Text": "25%",
                                "Offset": 72,
                                "Length": 3
                            }
                        ],
                        "Type": "Quantity",
                        "SubType": "Percentage"
                    }
                ]
            }
        ],
        "Errors": []
    }
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie sich mit Konzepten und mit dem Workflow für die Entitätsverknüpfung unter Verwendung der Textanalyse in Cognitive Services vertraut gemacht. Zusammenfassung:

+ Die [Entitäten-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634) ist für ausgewählte Sprachen verfügbar.
+ JSON-Dokumente im Anforderungstext umfassen eine ID, Text und einen Sprachcode.
+ Die POST-Anforderung wird an einen Endpunkt vom Typ `/entities` gesendet. Dabei werden ein personalisierter [Zugriffsschlüssel und ein Endpunkt](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) verwendet, der für Ihr Abonnement gültig ist.
+ Die Antwortausgabe, die aus verknüpften Entitäten besteht (z.B. Zuverlässigkeitsbewertungen, Offsets und Weblinks für jede Dokument-ID), kann in allen Anwendungen verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Textanalyse-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

* [Übersicht über die Textanalyse](../overview.md)
* [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)</br>
* [Textanalysen (Produktseite)](//go.microsoft.com/fwlink/?LinkID=759712)
