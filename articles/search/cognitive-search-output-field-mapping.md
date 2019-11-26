---
title: Zuordnen von Eingaben zu Ausgabefeldern
titleSuffix: Azure Cognitive Search
description: Extrahieren Sie Quelldatenfelder, und reichern Sie diese mit Daten an, um sie Ausgabefeldern in einem Index der kognitiven Azure-Suche zuzuordnen.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d2d5e717154d16cc5579c1495aff9c1eebf54b17
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132382"
---
# <a name="how-to-map-ai-enriched-fields-to-a-searchable-index"></a>Zuordnen von mit KI angereicherten Feldern zu einem durchsuchbaren Index

In diesem Artikel erfahren Sie, wie Sie angereicherte Eingabefelder Ausgabefeldern in einem durchsuchbaren Index zuordnen. Nachdem Sie ein [Skillset definiert](cognitive-search-defining-skillset.md) haben, müssen Sie die Ausgabefelder aller Skills zuordnen, die Werte direkt für ein bestimmtes Feld in Ihrem Suchindex bereitstellen. Feldzuordnungen sind für das Verschieben von Inhalt aus angereicherten Dokumenten in den Index erforderlich.


## <a name="use-outputfieldmappings"></a>Verwenden von „outputFieldMappings“
Um Felder zuzuordnen, fügen Sie `outputFieldMappings` zu Ihrer Indexerdefinition hinzu, wie unten gezeigt:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```

Der Text der Anforderung ist wie folgt strukturiert:

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions"
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```
Setzen Sie für jede Ausgabefeldzuordnung den Namen des angereicherten Feldes (sourceFieldName) und den Namen des im Index referenzierten Feldes (targetFieldName) fest.

Der Pfad in einem sourceFieldName kann ein Element oder mehrere Elemente darstellen. Im obigen Beispiel stellt ```/document/content/sentiment``` einen einzelnen numerischen Wert dar, während ```/document/content/organizations/*/description``` für mehrere Organisationsbeschreibungen steht. In Fällen, in denen es mehrere Elemente gibt, werden diese zu einem Array „vereinfacht“, das jedes der Elemente enthält. Für das Beispiel ```/document/content/organizations/*/description``` würden die Daten im Feld *Beschreibungen* demnach wie ein flaches Array von Beschreibungen aussehen, bevor sie indiziert werden:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```
## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihre angereicherten Felder den durchsuchbaren Feldern zugeordnet haben, können Sie die Feldattribute für jedes der durchsuchbaren Felder [als Teil der Indexdefinition](search-what-is-an-index.md) festlegen.

Weitere Informationen zu Feldzuordnungen finden Sie unter [Feldzuordnungen in Indexern der kognitiven Azure-Suche](search-indexer-field-mappings.md).
