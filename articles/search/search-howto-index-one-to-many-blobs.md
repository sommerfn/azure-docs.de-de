---
title: Indizieren eines Blobs in mehrere Suchindexdokumente über den Azure-Blobindexer für die Volltextsuche
titleSuffix: Azure Cognitive Search
description: Durchforsten von Azure-Blobs nach Textinhalten mithilfe des Blobindexers der kognitiven Azure-Suche. Jedes Blob kann ein oder mehrere Suchindexdokumente ergeben.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 127354e55a81e379825b41759f2b6150ba554a12
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73818541"
---
# <a name="indexing-blobs-to-produce-multiple-search-documents"></a>Indizieren von Blobs zum Generieren mehrerer Suchdokumente
Standardmäßig behandelt ein Blobindexer die Inhalte eines Blobs als ein Suchdokument. Einige **parsingMode**-Werte unterstützen Szenarios, in denen sich aus einem einzigen Blob mehrere Suchdokumente ergeben können. Es gibt folgende unterschiedliche Typen von **parsingMode**, die es einem Indexer ermöglichen, mehr als ein Suchdokument aus einem Blob zu extrahieren:
+ `delimitedText`
+ `jsonArray`
+ `jsonLines`

## <a name="one-to-many-document-key"></a>1:n-Dokumentschlüssel
Jedes Dokument, das in einem Index der kognitiven Azure-Suche angezeigt wird, wird eindeutig durch einen Dokumentschlüssel identifiziert. 

Wenn kein Analysemodus angegeben wird und keine explizite Zuordnung für das Schlüsselfeld im Index vorhanden ist, wird in der kognitiven Azure-Suche automatisch die `metadata_storage_path`-Eigenschaft als Schlüssel [zugeordnet](search-indexer-field-mappings.md). Durch diese Zuordnung wird sichergestellt, dass jedes Blob als einzelnes Suchdokument angezeigt wird.

Wenn einer der oben aufgeführten Analysemodi verwendet wird, wird ein Blob mehreren Suchdokumenten zugeordnet, sodass ein Dokumentschlüssel, der nur auf Blobmetadaten basiert, ungeeignet ist. In der kognitiven Azure-Suche kann jedoch ein 1:n-Dokumentschlüssel für jede einzelne Entität generiert werden, die aus einem Blob extrahiert wird, um diese Einschränkung zu umgehen. Diese Eigenschaft mit dem Namen `AzureSearch_DocumentKey` wird jeder einzelnen Entität hinzugefügt, die aus dem Blob extrahiert wird. Der Wert dieser Eigenschaft ist für jede einzelne Entität _blobübergreifend_ garantiert eindeutig, und die Entitäten werden als separate Suchdokumente angezeigt.

Wenn keine expliziten Feldzuordnungen für das Schlüsselindexfeld angegeben werden, wird mit der Feldzuordnungsfunktion `base64Encode` standardmäßig `AzureSearch_DocumentKey` zugeordnet.

## <a name="example"></a>Beispiel
Angenommen, Ihre Indexdefinition weist folgende Felder auf:
+ `id`
+ `temperature`
+ `pressure`
+ `timestamp`

Und Ihr Blobcontainer enthält Blobs mit folgender Struktur:

_Blob1.json_

    { "temperature": 100, "pressure": 100, "timestamp": "2019-02-13T00:00:00Z" }
    { "temperature" : 33, "pressure" : 30, "timestamp": "2019-02-14T00:00:00Z" }

_Blob2.json_

    { "temperature": 1, "pressure": 1, "timestamp": "2018-01-12T00:00:00Z" }
    { "temperature" : 120, "pressure" : 3, "timestamp": "2013-05-11T00:00:00Z" }

Wenn Sie einen Indexer erstellen und **parsingMode** auf `jsonLines` festlegen, ohne explizite Feldzuordnungen für das Schlüsselfeld anzugeben, wird die folgende Zuordnung implizit angewendet
    
    {
        "sourceFieldName" : "AzureSearch_DocumentKey",
        "targetFieldName": "id",
        "mappingFunction": { "name" : "base64Encode" }
    }

Mit diesem Setup enthält der Index der kognitiven Azure-Suche folgende Informationen (Base64-codierte ID aus Gründen der Übersichtlichkeit gekürzt).

| id | Temperatur | pressure | timestamp |
|----|-------------|----------|-----------|
| aHR0 ... YjEuanNvbjsx | 100 | 100 | 2019-02-13T00:00:00Z |
| aHR0 ... YjEuanNvbjsy | 33 | 30 | 2019-02-14T00:00:00Z |
| aHR0 ... YjIuanNvbjsx | 1 | 1 | 2018-01-12T00:00:00Z |
| aHR0 ... YjIuanNvbjsy | 120 | 3 | 2013-05-11T00:00:00Z |

## <a name="custom-field-mapping-for-index-key-field"></a>Benutzerdefinierte Feldzuordnung für das Indexschlüsselfeld

Angenommen, Ihr Blobcontainer enthält bei derselben Indexdefinition wie im vorherigen Beispiel Blobs mit folgender Struktur:

_Blob1.json_

    recordid, temperature, pressure, timestamp
    1, 100, 100,"2019-02-13T00:00:00Z" 
    2, 33, 30,"2019-02-14T00:00:00Z" 

_Blob2.json_

    recordid, temperature, pressure, timestamp
    1, 1, 1,"2018-01-12T00:00:00Z" 
    2, 120, 3,"2013-05-11T00:00:00Z" 

Wenn Sie einen Indexer mit **parsingMode** `delimitedText` erstellen, kommt es Ihnen möglicherweise natürlich vor, wie folgt eine Feldzuordnungsfunktion für die Schlüsselfelder einzurichten:

    {
        "sourceFieldName" : "recordid",
        "targetFieldName": "id"
    }

Aus dieser Zuordnung ergeben sich jedoch _nicht_ 4 Dokumente, die im Index angezeigt werden, da das Feld `recordid` nicht _blobübergreifend_ eindeutig ist. Deshalb wird empfohlen, dass Sie die implizite Feldzuordnung verwenden, die von der Eigenschaft `AzureSearch_DocumentKey` auf das Schlüsselindexfeld für 1:n-Analysemodi angewendet wird.

Wenn Sie eine explizite Feldzuordnung einrichten möchten, sollten Sie sicherstellen, dass _sourceField_ für jede einzelne Entität **blobübergreifend** eindeutig ist.

> [!NOTE]
> Der von `AzureSearch_DocumentKey` verwendete Ansatz zur Sicherstellung der Eindeutigkeit jeder extrahierten Entität kann sich ändern, und Sie sollten sich deshalb für die Anforderungen Ihrer Anwendung nicht darauf verlassen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie noch nicht mit der grundlegenden Struktur und dem Workflow der Blobindizierung vertraut sind, sollten Sie zunächst [Indizieren von Azure Blob Storage mit Azure Cognitive Search](search-howto-index-json-blobs.md) lesen. Weitere Informationen zum Analysemodus für die verschiedenen Blobinhaltstypen finden Sie in den folgenden Artikeln.

> [!div class="nextstepaction"]
> [Indizieren von CSV-Blobs](search-howto-index-csv-blobs.md)
> [Indizieren von JSON-Blobs](search-howto-index-json-blobs.md)
