---
title: Indizieren von CSV-Blobs mit einem Blobindexer der kognitiven Azure-Suche
titleSuffix: Azure Cognitive Search
description: Durchforsten von CSV-Blobs in Azure Blob Storage für die Volltextsuche mithilfe eines Indexers der kognitiven Azure-Suche. Indexer automatisieren die Datenerfassung für ausgewählte Datenquellen wie Azure Blob Storage.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 18d0eb704deba80bf83b5cae0a598f47181700f7
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72793779"
---
# <a name="how-to-index-csv-blobs-using-a-blob-indexer-in-azure-cognitive-search"></a>Indizieren von CSV-Blobs mit einem Blobindexer in der kognitiven Azure-Suche 

> [!Note]
> Der delimitedText-Analysemodus befindet sich in der Vorschau und ist nicht für die Produktion ausgelegt. Dieses Feature wird durch die [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Das .NET SDK wird derzeit nicht unterstützt.
>

Standardmäßig analysiert der [Blobindexer der kognitiven Azure-Suche](search-howto-indexing-azure-blob-storage.md) durch Trennzeichen getrennte Blobs als einzelnen Textblock. Bei Blobs mit CSV-Daten sollen die einzelnen Zeilen im Blob jedoch häufig als separates Dokument behandelt werden. Angenommen, Sie möchten den folgenden durch Trennzeichen getrennten Text in zwei Dokumente analysieren, die jeweils die Felder „Id“, „DatePublished“ und „Tags“ enthalten: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

In diesem Artikel erfahren Sie, wie Sie CSV-Blobs mit einem Blobindexer der kognitiven Azure-Suche durch Festlegen des Analysemodus `delimitedText` analysieren. 

> [!NOTE]
> Befolgen Sie die Empfehlungen zur Indexerkonfiguration in [1:n-Indizierung](search-howto-index-one-to-many-blobs.md), um mehrere Suchdokumente aus einem Azure-Blob auszugegeben.

## <a name="setting-up-csv-indexing"></a>Einrichten der CSV-Indizierung
Erstellen oder aktualisieren Sie zum Indizieren von CSV-Blobs eine Indexerdefinition mit dem `delimitedText`-Analysemodus in einer [Indexer erstellen](https://docs.microsoft.com/rest/api/searchservice/create-indexer)-Anforderung:

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

`firstLineContainsHeaders` gibt an, dass die erste (nicht leere) Zeile der einzelnen Blobs Header enthält.
Wenn Blobs am Anfang keine Headerzeile enthalten, sollten die Header in der Indexerkonfiguration angegeben werden: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Sie können das Trennzeichen mithilfe der Konfigurationseinstellung `delimitedTextDelimiter` anpassen. Beispiel:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Derzeit wird nur die UTF-8-Codierung unterstützt. Wenn Sie Unterstützung für andere Codierungen benötigen, stimmen Sie unter [UserVoice](https://feedback.azure.com/forums/263029-azure-search) dafür ab.

> [!IMPORTANT]
> Bei Verwendung des Analysemodus für durch Trennzeichen getrennten Text wird in der kognitiven Azure-Suche davon ausgegangen, dass alle Blobs in der Datenquelle CSV-Blobs sind. Wenn Sie eine Mischung aus CSV- und Nicht-CSV-Blobs in der gleichen Datenquelle unterstützen müssen, stimmen Sie unter [UserVoice](https://feedback.azure.com/forums/263029-azure-search) dafür ab.
> 
> 

## <a name="request-examples"></a>Beispiele für Anforderungen
Dies alles wird an vollständigen Nutzlastbeispielen demonstriert. 

Datenquelle: 

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexer:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-cognitive-search-better"></a>Helfen Sie uns bei der Verbesserung der kognitiven Azure-Suche
Wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben, stimmen Sie unter [UserVoice](https://feedback.azure.com/forums/263029-azure-search/) dafür ab.

