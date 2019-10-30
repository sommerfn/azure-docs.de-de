---
title: Arbeiten mit Qualifikationsgruppen
titleSuffix: Azure Cognitive Search
description: In Qualifikationsgruppen erstellen Sie eine KI-Anreicherungspipeline in der kognitiven Azure-Suche. Informieren Sie sich über wichtige Konzepte und Details zur Zusammenstellung von Qualifikationsgruppen.
manager: nitinme
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 8a783581394de05fff9f0060e124e8dc59c96b60
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72790167"
---
# <a name="working-with-skillsets-in-azure-cognitive-search"></a>Arbeiten mit Qualifikationsgruppen in der kognitiven Azure-Suche

Dieser Artikel richtet sich an Entwickler, die ein besseres Verständnis der Funktionsweise der Anreicherungspipeline benötigen. Dabei wird davon ausgegangen, dass Sie über ein konzeptionelles Verständnis des KI-Anreicherungsprozesses verfügen. Wenn Ihnen dieses Konzept noch nicht vertraut ist, beginnen Sie mit folgenden Themen:
+ [KI-Anreicherung in der kognitiven Azure-Suche](cognitive-search-concept-intro.md)
+ [Wissensspeicher (Vorschau)](knowledge-store-concept-intro.md)

## <a name="specify-the-skillset"></a>Festlegen der Qualifikationsgruppe
Eine Qualifikationsgruppe ist eine wiederverwendbare Ressource in der kognitiven Azure-Suche, die eine Sammlung von kognitiven Fähigkeiten (Qualifikationen) festlegt, die zum Analysieren, Transformieren und Anreichern von Text- oder Bildinhalten während der Indizierung verwendet werden. Mithilfe einer Qualifikationsgruppe können Sie Text- und Bildanreicherungen in der Datenerfassungsphase hinzufügen sowie neue Informationen und Strukturen aus Rohinhalten extrahieren und erstellen.

Eine Qualifikationsgruppe verfügt über drei Eigenschaften:

+   ```skills```, eine ungeordnete Sammlung von Qualifikationen, für die die Plattform die Ausführungssequenz basierend auf den für jede Qualifikation erforderlichen Eingaben bestimmt
+   ```cognitiveServices```, der zum Berechnen der aufgerufenen kognitiven Fähigkeiten erforderliche Cognitive Services-Schlüssel
+   ```knowledgeStore```, das Speicherkonto, in das Ihre angereicherten Dokumente projiziert werden



Qualifikationsgruppen werden in JSON erstellt. Mithilfe der [Ausdruckssprache](https://docs.microsoft.com/azure/search/cognitive-search-skill-conditional) können Sie komplexe Qualifikationsgruppen mit Schleifen und [Verzweigungen](https://docs.microsoft.com/en-us/azure/search/cognitive-search-skill-conditional) erstellen. Die Ausdruckssprache verwendet die [JSON-Zeiger](https://tools.ietf.org/html/rfc6901)-Pfadnotation mit einigen Änderungen zum Identifizieren von Knoten in der Anreicherungsstruktur. Ein ```"/"``` durchläuft eine niedrigere Ebene in der Struktur und ```"*"``` fungiert als „for-each“-Operator im Kontext. Diese Konzepte lassen sich am besten mit einem Beispiel verdeutlichen. Um einige Konzepte und Funktionen zu veranschaulichen, schauen wir uns die Qualifikationsgruppen für das [Beispiel „Hotelbewertungen“](knowledge-store-connect-powerbi.md) an. Um die Qualifikationsgruppe anzuzeigen, nachdem Sie den Workflow zum Importieren von Daten ausgeführt haben, müssen Sie unter Verwendung eines REST-API-Clients die [Qualifikationsgruppe abrufen](https://docs.microsoft.com/en-us/rest/api/searchservice/get-skillset).

### <a name="enrichment-tree"></a>Anreicherungsstruktur

Um zu sehen, wie eine Qualifikationsgruppe Ihr Dokument zunehmend anreichert, beginnen wir damit, wie das Dokument vor den Anreicherungen aussieht. Die Ausgabe der Dokumententschlüsselung ist abhängig von der Datenquelle und dem ausgewählten spezifischen Analysemodus. Dies ist auch der Zustand des Dokuments, aus dem die [Feldzuordnungen](search-indexer-field-mappings.md) beim Hinzufügen von Daten zum Suchindex Inhalte abrufen können.
![Wissensspeicher im Pipelinediagramm](./media/knowledge-store-concept-intro/annotationstore_sans_internalcache.png "KWissensspeicher im Pipelinediagramm)

Sobald sich ein Dokument in der Anreicherungspipeline befindet, wird es als Inhaltsstruktur mit zugeordneten Anreicherungen dargestellt. Diese Struktur wird als Ausgabe der Dokumententschlüsselung instanziiert. Das Anreicherungsstruktur-Format ermöglicht der Anreicherungspipeline das Anfügen von Metadaten auch an primitive Datentypen. Es handelt sich nicht um ein gültiges JSON-Objekt, kann jedoch in ein gültiges JSON-Format projiziert werden. In der folgenden Tabelle wird der Zustand eines Dokuments gezeigt, das in die Anreicherungspipeline wechselt:

|Datenquelle\Analysemodus|Standard|JSON, JSON-Zeilen & CSV|
|---|---|---|
|Blob Storage|/document/content<br>/document/normalized_images/*<br>…|/document/{key1}<br>/document/{key2}<br>…|
|SQL|/document/{column1}<br>/document/{column2}<br>…|– |
|Cosmos DB|/document/{key1}<br>/document/{key2}<br>…|–|

 Beim Ausführen von Qualifikationen werden neue Knoten zur Anreicherungsstruktur hinzugefügt. Diese neuen Knoten können dann als Eingaben für Downstreamqualifikationen verwendet werden, und somit in den Wissensspeicher projiziert oder Indexfeldern zugeordnet werden. Anreicherungen sind nicht änderbar: Die Knoten können nach der Erstellung nicht bearbeitet werden. Mit steigender Komplexität Ihrer Qualifikationen wird auch Ihre Anreicherungsstruktur komplexer, aber nicht alle Knoten in der Anreicherungsstruktur müssen in den Index oder Wissensspeicher aufgenommen werden. Sie können selektiv nur eine Teilmenge der Anreicherungen im Index oder Wissensspeicher beibehalten.

Sie können selektiv nur eine Teilmenge der Anreicherungen im Index oder Wissensspeicher beibehalten.
In den übrigen Teilen dieses Dokuments gehen wir vom [Beispiel „Hotelbewertungen“](https://docs.microsoft.com/en-us/azure/search/knowledge-store-connect-powerbi) aus, die gleichen Konzepte gelten aber auch für die Anreicherung von Dokumenten aus allen anderen Datenquellen.

### <a name="context"></a>Kontext
Jede Qualifikation erfordert einen Kontext. Ein Kontext bestimmt Folgendes:
+   Wie oft die Qualifikation ausgeführt wird, basierend auf den ausgewählten Knoten. Wird bei Kontextwerten einer Typsammlung ein ```/*``` an das Ende hinzugefügt, führt dies dazu, dass die Qualifikation für jede Instanz in der Sammlung einmal aufgerufen wird. 
+   Wo in der Anreicherungsstruktur die Qualifikationsausgaben hinzugefügt werden. Ausgaben werden der Struktur immer als untergeordnete Elemente des Kontextknotens hinzugefügt. 
+   Form der Eingaben. Bei Sammlungen mit mehreren Ebenen wirkt sich das Festlegen des Kontexts auf die übergeordnete Sammlung auf die Form der Eingabe der Qualifikation aus. Sie verfügen beispielsweise über eine Anreicherungsstruktur mit einer Liste von Ländern, von denen jedes mit einer Liste von Bundesländern angereichert ist, die wiederum eine Liste von Postleitzahlen enthalten.

|Kontext|Eingabe|Form der Eingabe|Aufruf einer Qualifikation|
|---|---|---|---|
|```/document/countries/*``` |```/document/countries/*/states/*/zipcodes/*``` |Eine Liste aller Postleitzahlen im Land |Einmal pro Land |
|```/document/countries/*/states/*``` |```/document/countries/*/states/*/zipcodes/*``` |Eine Liste aller Postleitzahlen im Bundesland | Einmal pro Kombination aus Land und Bundesland|

### <a name="sourcecontext"></a>SourceContext

`sourceContext` wird nur in [Shaper-Qualifikationen](cognitive-search-skill-shaper.md) und [Projektionen](knowledge-store-projection-overview.md) verwendet. Es wird verwendet, um geschachtelte Objekte mit mehreren Ebenen zu erstellen. Mit `sourceContext` können Sie ein hierarchisches, anonymes Typobjekt erstellen, für das mehrere Qualifikationen erforderlich wären, wenn Sie nur den Kontext verwenden würden. Die Verwendung von `sourceContext` wird im nächsten Abschnitt gezeigt.

### <a name="projections"></a>Projektionen

Als Projektion wird der Prozess bezeichnet, bei dem die Knoten aus der Anreicherungsstruktur ausgewählt werden, die im Wissensspeicher gespeichert werden sollen. Projektionen sind benutzerdefinierte Formen des Dokuments (Inhalt und Anreicherungen), die entweder als Tabellen- oder Objektprojektionen ausgegeben werden können. Weitere Informationen zum Arbeiten mit Projektionen finden Sie unter [Arbeiten mit Projektionen](knowledge-store-projection-overview.md).

![Feldzuordnungsoptionen](./media/cognitive-search-working-with-skillsets/field-mapping-options.png "Feldzuordnungsoptionen für die Anreicherungspipeline")

Im Diagramm oben wird die Auswahl beschrieben, mit der Sie arbeiten, je nachdem, wo Sie sich in der Anreicherungspipeline befinden.

## <a name="generate-enriched-data"></a>Generieren von angereicherten Daten 

Betrachten wir nun die Qualifikationsgruppe „Hotelbewertungen“ näher. Mithilfe des [Tutorials](knowledge-store-connect-powerbi.md) können Sie die Qualifikationsgruppe erstellen oder [anzeigen](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/samples/skillset.json). Wir betrachten Folgendes:

* Die Entwicklung der Anreicherungsstruktur mit Ausführung der einzelnen Qualifikationen 
* Die Funktionsweise des Kontexts und der Eingaben, um zu bestimmen, wie oft eine Qualifikation ausgeführt wird 
* Die Form der Eingabe in Abhängigkeit vom Kontext. 

Da wir für den Indexer den Analysemodus „Text mit Trennzeichen“ verwenden, stellt ein Dokument innerhalb des Anreicherungsprozesses eine einzelne Zeile in der CSV-Datei dar.

### <a name="skill-1-split-skill"></a>Qualifikation 1: Qualifikation „Aufteilung“ 

![Anreicherungsstruktur nach der Dokumententschlüsselung](media/cognitive-search-working-with-skillsets/enrichment-tree-doc-cracking.png "Anreicherungsstruktur nach der Dokumententschlüsselung")

Mit dem Qualifikationskontext ```"/document/reviews_text"``` wird diese Qualifikation für `reviews_text` genau einmal ausgeführt. Die Qualifikationsausgabe ist eine Liste, in der `reviews_text` in 5000-Zeichen-Segmente aufgeteilt ist. Die Ausgabe der Qualifikation „Aufteilung“ wird `pages` benannt und der Anreicherungsstruktur hinzugefügt. Mit dem Feature `targetName` können Sie eine Qualifikationsausgabe umbenennen, bevor sie der Anreicherungsstruktur hinzugefügt wird.

Die Anreicherungsstruktur verfügt jetzt über einen neuen Knoten, der unter den Kontext der Qualifikation platziert wird. Dieser Knoten ist für alle Qualifikationen, Projektionen oder Ausgabefeldzuordnungen verfügbar.


Der Stammknoten für alle Anreicherungen ist `"/document"`. Wenn Sie Blob-Indexer verwenden, verfügt der `"/document"`-Knoten über die untergeordneten Knoten `"/document/content"` und `"/document/normalized_images"`. Wenn Sie, wie in diesem Beispiel, CSV-Daten verwenden, werden die Spaltennamen den Knoten unter `"/document"` zugeordnet. Um auf eine der Anreicherungen zuzugreifen, die einem Knoten durch eine Qualifikation hinzugefügt wurden, ist der vollständige Pfad für die Anreicherung erforderlich. Wenn Sie z.B. den Text aus dem Knoten ```pages``` als Eingabe für eine andere Qualifikation verwenden möchten, müssen Sie ihn als ```"/document/reviews_text/pages/*"``` angeben.
 
 ![Anreicherungsstruktur nach Qualifikation 1](media/cognitive-search-working-with-skillsets/enrichment-tree-skill1.png "Anreicherungsstruktur nach Ausführung von Qualifikation 1")

### <a name="skill-2-language-detection"></a>Qualifikation 2: Sprachenerkennung
 Die Qualifikation „Spracherkennung“ ist zwar die dritte in der Qualifikationsgruppe definierte Qualifikation (Qualifikation 3), sie wird aber als nächste Qualifikation ausgeführt. Da keine Eingaben erforderlich sind, wird sie nicht blockiert und parallel mit der vorherigen Qualifikation ausgeführt. Genau wie die Qualifikation „Aufteilung“ wird die Qualifikation „Spracherkennung“ auch einmal für jedes Dokument aufgerufen. Die Anreicherungsstruktur verfügt jetzt über einen neuen Knoten für die Sprache.
 ![Anreicherungsstruktur nach Qualifikation 2](media/cognitive-search-working-with-skillsets/enrichment-tree-skill2.png "EnAnreicherungsstruktur nach Ausführung von Qualifikation 2
 
 ### <a name="skill-3-key-phrases-skill"></a>Qualifikation 3: Qualifikation „Schlüsselbegriffe“ 

Im Kontext ```/document/reviews_text/pages/*``` wird die Qualifikation „Schlüsselbegriffe“ einmal für jedes Element in der `pages`-Sammlung aufgerufen. Die Ausgabe der Qualifikation ist ein Knoten unter dem zugeordneten page-Element. 

 Wenn Sie sich jetzt die restlichen Qualifikationen in der Qualifikationsgruppe anschauen, sollten Sie sich vorstellen können, wie die Anreicherungsstruktur mit der Ausführung jeder weiteren Qualifikation weiter wächst. Einige Qualifikationen, wie z.B. die Qualifikation „Zusammenführen“ und die Qualifikation „Shaper“, erstellen auch neue Knoten, verwenden jedoch nur Daten aus vorhandenen Knoten und erstellen keine eigenen neuen Anreicherungen.

![Anreicherungsstruktur nach allen Qualifikationen](media/cognitive-search-working-with-skillsets/enrichment-tree-final.png "Anreicherungsstruktur nach allen Qualifikationen")

Die Farben der Connectors in der Struktur oben zeigen an, dass die Anreicherungen durch unterschiedliche Qualifikationen erstellt wurden, und die Knoten müssen einzeln adressiert werden und sind nicht Teil des Objekts, das bei der Auswahl des übergeordneten Knotens zurückgegeben wird.

## <a name="save-enrichments-in-a-knowledge-store"></a>Speichern von Anreicherungen in einem Wissensspeicher 

Qualifikationsgruppen definieren außerdem einen Wissensspeicher, in dem Ihre angereicherten Dokumente als Tabellen oder Objekte projiziert werden können. Um die angereicherten Daten im Wissensspeicher zu speichern, definieren Sie eine Reihe von Projektionen Ihres angereicherten Dokuments. Weitere Informationen zum Wissensspeicher finden Sie in der [Übersicht über Wissensspeicher](knowledge-store-concept-intro.md).

### <a name="slicing-projections"></a>Aufteilen von Projektionen

Wenn Sie eine Tabellenprojektionsgruppe definieren, kann ein einzelner Knoten in der Anreicherungsstruktur in mehrere verwandte Tabellen aufgeteilt werden. Wenn Sie eine Tabelle mit einem Quellpfad hinzufügen, der ein untergeordnetes Element einer vorhandenen Tabellenprojektion ist, wird der resultierende untergeordnete Knoten kein untergeordnetes Element der vorhandenen Tabellenprojektion sein, sondern stattdessen in die neue, verwandte Tabelle projiziert. Mit dieser Aufteilungstechnik können Sie einen einzelnen Knoten in einer Shaper-Qualifikation als Quelle für alle Ihre Tabellenprojektionen definieren. 

### <a name="shaping-projections"></a>Strukturieren von Projektionen

Projektionen können auf zwei Arten definiert werden. Sie können eine Shaper-Qualifikation verwenden, um einen neuen Knoten zu erstellen, der als Stammknoten für alle von Ihnen projizierten Anreicherungen dient. Dann würden Sie in ihren Projektionen nur auf die Ausgabe der Shaper-Qualifikation verweisen. Sie können eine Projektion auch innerhalb der Projektionsdefinition selbst inline strukturieren.

Der Shaper-Ansatz ist ausführlicher als die Inlinestrukturierung, stellt jedoch sicher, dass alle Mutationen der Anreicherungsstruktur in den Qualifikationen enthalten sind und dass die Ausgabe ein Objekt ist, das wiederverwendet werden kann. Mit der Inlinestrukturierung können Sie die benötigte Form erstellen, aber es handelt sich um ein anonymes Objekt, das nur für die Projektion verfügbar ist, für die es definiert wurde. Die Ansätze können gleichzeitig oder getrennt einzeln verwendet werden. Die für Sie im Portalworkflow erstellte Qualifikationsgruppe enthält beide. Sie verwendet eine Shaper-Qualifikation für die Tabellenprojektionen, aber auch Inlinestrukturierung zum Projizieren der Tabelle mit den Schlüsselbegriffen.

Um das Beispiel zu erweitern, könnten Sie die Inlinestrukturierung entfernen und mithilfe einer Shaper-Qualifikation einen neuen Knoten für die Schlüsselbegriffe erstellen. Zum Erstellen einer in drei Tabellen (`hotelReviewsDocument`, `hotelReviewsPages` und `hotelReviewsKeyPhrases`) projizierten Form werden die beiden Optionen in den folgenden Abschnitten beschrieben.


#### <a name="shaper-skill-and-projection"></a>Shaper-Qualifikation und Projektion 

> [!Note]
> Einige der Spalten aus der Dokumententabelle wurden aus Platzgründen aus diesem Beispiel entfernt.
>
```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "name": "#5",
    "description": null,
    "context": "/document",
    "inputs": [        
        {
            "name": "reviews_text",
            "source": "/document/reviews_text",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "reviews_title",
            "source": "/document/reviews_title",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "AzureSearch_DocumentKey",
            "source": "/document/AzureSearch_DocumentKey",
            "sourceContext": null,
            "inputs": []
        },  
        {
            "name": "pages",
            "source": null,
            "sourceContext": "/document/reviews_text/pages/*",
            "inputs": [
                {
                    "name": "SentimentScore",
                    "source": "/document/reviews_text/pages/*/Sentiment",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "LanguageCode",
                    "source": "/document/Language",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "Page",
                    "source": "/document/reviews_text/pages/*",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "keyphrase",
                    "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                    "inputs": [
                        {
                            "source": "/document/reviews_text/pages/*/Keyphrases/*",
                            "name": "Keyphrases"
                        }
                    ]
                }
            ]
        }
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "tableprojection"
        }
    ]
}
```

Mit dem `tableprojection`-Knoten, der im Abschnitt `outputs` oben definiert wurde, können wir jetzt das Aufteilungsfeature verwenden, um Teile des `tableprojection`-Knotens in verschiedene Tabellen zu projizieren:

> [!Note]
> Dies ist nur ein Codeausschnitt der Projektion innerhalb der Wissensspeicherkonfiguration.
>
```json
"projections": [
    {
        "tables": [
            {
                "tableName": "hotelReviewsDocument",
                "generatedKeyName": "Documentid",
                "source": "/document/tableprojection"
            },
            {
                "tableName": "hotelReviewsPages",
                "generatedKeyName": "Pagesid",
                "source": "/document/tableprojection/pages/*"
            },
            {
                "tableName": "hotelReviewsKeyPhrases",
                "generatedKeyName": "KeyPhrasesid",
                "source": "/document/tableprojection/pages/*/keyphrase/*"
            }
        ]
    }
]
```

#### <a name="inline-shaping-projections"></a>Inlinestrukturierung von Projektionen

Der Inlinestrukturierungsansatz erfordert keine Shaper-Qualifikationen, da alle für die Projektionen benötigten Formen genau dann erstellt werden, wenn Sie benötigt werden. Um die gleichen Daten wie im vorherigen Beispiel zu projizieren, würde die Inlineprojektionsoption wie folgt aus:

```json
"projections": [
    {
        "tables": [
            {
                "tableName": "hotelReviewsInlineDocument",
                "generatedKeyName": "Documentid",
                "sourceContext": "/document",     
                "inputs": [
                    {
                        "name": "reviews_text",
                        "source": "/document/reviews_text"
                    },
                    {
                        "name": "reviews_title",
                        "source": "/document/reviews_title"
                    },
                    {
                        "name": "AzureSearch_DocumentKey",
                        "source": "/document/AzureSearch_DocumentKey"
                    }                             
                ]
            },
            {
                "tableName": "hotelReviewsInlinePages",
                "generatedKeyName": "Pagesid",
                "sourceContext": "/document/reviews_text/pages/*",
                "inputs": [
                        {
                    "name": "SentimentScore",
                    "source": "/document/reviews_text/pages/*/Sentiment"
                    },
                    {
                        "name": "LanguageCode",
                        "source": "/document/Language"
                    },
                    {
                        "name": "Page",
                        "source": "/document/reviews_text/pages/*"
                    }
                ]
            },
            {
                "tableName": "hotelReviewsInlineKeyPhrases",
                "generatedKeyName": "KeyPhraseId",
                "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                "inputs": [
                    {
                        "name": "Keyphrases",
                        "source": "/document/reviews_text/pages/*/Keyphrases/*"
                    }
                ]
            }
        ]
    }
]
```
  
Bei beiden Ansätzen ist zu beobachten, wie Werte von `"Keyphrases"` mithilfe von `"sourceContext"` projiziert werden. Der `"Keyphrases"`-Knoten, der eine Sammlung von Zeichenfolgen enthält, ist selbst ein untergeordnetes Element des Seitentexts. Da Projektionen jedoch ein JSON-Objekt erfordern und es sich bei der Seite um einen Grundtyp (Zeichenfolge) handelt, wird `"sourceContext"` verwendet, um den Schlüsselbegriff in ein Objekt mit einer benannten Eigenschaft zu umschließen. Diese Technik ermöglicht, dass sogar Grundtypen unabhängig projiziert werden können.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Schritt erstellen Sie ihre erste Qualifikationsgruppe mit kognitiven Fähigkeiten.

> [!div class="nextstepaction"]
> [Erstellen Ihrer ersten Qualifikationsgruppe](cognitive-search-defining-skillset.md).
