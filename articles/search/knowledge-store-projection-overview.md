---
title: Arbeiten mit Projektionen in einem Wissensspeicher (Vorschau)
titleSuffix: Azure Cognitive Search
description: Speichern und formen Sie Ihre angereicherten Daten aus der Indizierungspipeline mit KI-Anreicherung in einem Wissensspeicher zur Verwendung in von der Volltextsuche abweichenden Szenarien. „Wissensspeicher“ ist zurzeit als öffentliche Vorschauversion verfügbar.
manager: nitinme
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: bb6af4be232810c1f5d135e459238e2e4f2cd5d8
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73720044"
---
# <a name="working-with-projections-in-a-knowledge-store-in-azure-cognitive-search"></a>Arbeiten mit Projektionen in einem Wissensspeicher in der kognitiven Azure-Suche

> [!IMPORTANT] 
> „Wissensspeicher“ ist zurzeit als öffentliche Vorschauversion verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Previewfunktionen werden von der [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Die Portalunterstützung ist momentan eingeschränkt, und das .NET SDK wird nicht unterstützt.

Die kognitive Azure-Suche ermöglicht eine Inhaltsanreicherung über integrierte kognitive Qualifikationen und benutzerdefinierte Qualifikationen als Teil der Indizierung. Anreicherungen strukturieren Ihre Dokumente und machen die Suche effektiver. In vielen Fällen sind die angereicherten Dokumente für von der Suche abweichenden Szenarien nützlich, z. B. für das Knowledge Mining.

Projektionen als Komponente des [Wissensspeichers](knowledge-store-concept-intro.md) sind Ansichten von angereicherten Dokumenten, die für das Knowledge Mining in physischen Speichern gespeichert werden können. Mit einer Projektion können Sie Ihre Daten in einer Form „projizieren“, die Ihren Anforderungen entspricht, und Beziehungen beibehalten, sodass die Daten in Tools wie Power BI ohne zusätzlichen Aufwand gelesen werden können.

Projektionen können tabellarisch sein, wobei die Daten in Zeilen und Spalten in Azure Table Storage gespeichert werden oder als JSON-Objekte, die in Azure Blob Storage gespeichert werden. Sie können mehrere Projektionen Ihrer Daten definieren, wenn diese angereichert werden. Mehrere Projektionen sind nützlich, wenn Sie die gleichen Daten für einzelne Anwendungsfälle unterschiedlich formen möchten.

Im Wissensspeicher werden drei Arten von Projektionen unterstützt:

+ **Tabellen**: Für Daten, die am besten als Zeilen und Spalten dargestellt werden, können Sie mit Tabellenprojektionen eine schematisierte Form oder Projektion in Table Storage definieren.

+ **Objekte:** Wenn Sie eine JSON-Darstellung Ihrer Daten und Anreicherungen benötigen, werden Objektprojektionen als Blobs gespeichert.

+ **Dateien**: Wenn Sie die aus den Dokumenten extrahierten Bilder speichern müssen, können Sie mithilfe von Dateiprojektionen die normalisierten Bilder speichern.

Im Kontext definierte Projektionen finden Sie unter [Erste Schritte mit Wissensspeichern](knowledge-store-howto.md).

## <a name="projection-groups"></a>Projektionsgruppen

In einigen Fällen müssen Sie die angereicherten Daten in verschiedenen Formen projizieren, um unterschiedliche Ziele zu erreichen. Im Wissensspeicher können Sie mehrere Gruppen von Projektionen definieren. Projektionsgruppen weisen die folgenden Hauptmerkmale für gegenseitige Ausschließlichkeit und Verbundenheit auf.

### <a name="mutual-exclusivity"></a>Gegenseitige Ausschließlichkeit

Alle Inhalte, die in einer einzelnen Gruppe projiziert werden, sind unabhängig von in anderen Projektionsgruppen projizierten Daten.
Diese Unabhängigkeit bedeutet, dass die gleichen Daten unterschiedlich geformt, aber in jeder Projektionsgruppe wiederholt vorhanden sein können.

### <a name="relatedness"></a>Verbundenheit

Projektionsgruppen ermöglichen es Ihnen jetzt, Ihre Dokumente übergreifend für alle Projektionstypen zu projizieren und gleichzeitig die Beziehungen zwischen den Projektionstypen zu erhalten. Alle Inhalte, die in einer einzelnen Projektionsgruppe projiziert werden, behalten die Beziehungen innerhalb der Daten für alle Projektionstypen bei. Innerhalb von Tabellen basieren Beziehungen auf einem generierten Schlüssel, und jeder untergeordnete Knoten behält einen Verweis auf den übergeordneten Knoten bei. Zwischen Typen (Tabellen, Objekte und Dateien) werden Beziehungen beibehalten, wenn ein einzelner Knoten typübergreifend projiziert wird. Stellen Sie sich beispielsweise ein Szenario vor, in dem Sie über ein Dokument mit Bildern und Text verfügen. Sie können den Text in Tabellen oder Objekten und die Bilder in Dateien projizieren, wobei die Tabellen oder Objekte über eine Eigenschaft verfügen, die die Datei-URL enthält.

## <a name="input-shaping"></a>Eingabestrukturierung

Die richtige Form oder Struktur Ihrer Daten ist ausschlaggebend für eine effektive Verwendung, egal ob in Tabellen oder Objekten. Die Möglichkeit, Ihre Daten basierend darauf, wie Sie den Zugriff und die Verwendung planen, zu formen oder zu strukturieren, ist ein entscheidendes Merkmal, das als Qualifikation **Shaper** in der Qualifikationsgruppe verfügbar gemacht wird.  

Projektionen lassen sich einfacher definieren, wenn die Anreicherungsstruktur ein Objekt enthält, das dem Schema der Projektion entspricht. Mit der aktualisierten [Qualifikation „Shaper“](cognitive-search-skill-shaper.md) können Sie ein Objekt aus verschiedenen Knoten der Anreicherungsstruktur erstellen und übergeordnet unter einem neuen Knoten festlegen. Mit der Qualifikation **Shaper** können Sie komplexe Typen mit geschachtelten Objekten definieren.

Wenn Sie eine neue Form definiert haben, die alle für die Projektion benötigten Elemente enthält, können Sie diese Form nun als Quelle für die Projektionen oder als Eingabe für eine andere Qualifikation verwenden.

## <a name="projection-slicing"></a>Projektionsslicing

Wenn Sie eine Projektionsgruppe definieren, kann ein einzelner Knoten in der Anreicherungsstruktur per Slicing in mehrere verknüpfte Tabellen oder Objekte aufgeteilt werden. Wenn eine Projektion mit einem Quellpfad hinzugefügt wird, der einer vorhandenen Projektion untergeordnet ist, führt dies dazu, dass der untergeordnete Knoten aus dem übergeordneten Knoten herausgeschnitten und in die neue, noch verknüpfte Tabelle bzw. das Objekt projiziert wird. Mit dieser Methode können Sie einen einzelnen Knoten in einer Qualifikation „Shaper“ als Quelle für all Ihre Projektionen definieren.

## <a name="table-projections"></a>Tabellenprojektionen

Da dies den Import vereinfacht, werden Tabellenprojektionen für die Datenuntersuchung mit Power BI empfohlen. Zudem ermöglichen Tabellenprojektionen die Änderung der Kardinalität zwischen Tabellenbeziehungen. 

Sie können ein einzelnes Dokument im Index in mehreren Tabellen projizieren und die Beziehungen beibehalten. Beim Projizieren in mehreren Tabellen wird die komplette Form in jeder Tabelle projiziert, es sei denn, ein untergeordneter Knoten ist die Quelle einer anderen Tabelle innerhalb derselben Gruppe.

### <a name="defining-a-table-projection"></a>Definieren einer Tabellenprojektion

Beim Definieren einer Tabellenprojektion im Element `knowledgeStore` der Qualifikationsgruppe ordnen Sie zunächst der Tabellenquelle einen Knoten in der Anreicherungsstruktur zu. Dieser Knoten ist normalerweise die Ausgabe einer **Shaper**-Qualifikation, die Sie der Liste der Qualifikationen hinzugefügt haben, um eine bestimmte Form zu generieren, die in Tabellen projiziert werden soll. Der Knoten, der projiziert werden soll, kann so segmentiert werden, dass er in mehreren Tabellen projiziert wird. Die Tabellendefinition ist eine Liste der Tabellen, die projiziert werden sollen.

Für jede Tabelle müssen drei Eigenschaften angegeben werden:

+ tableName: Der Name der Tabelle in Azure Storage.

+ generatedKeyName: Der Spaltenname für den Schlüssel, mit dem die Zeile eindeutig identifiziert wird.

+ source: Der Knoten in der Anreicherungsstruktur, aus der Sie die Anreicherungen erstellen. Dieser Knoten ist normalerweise die Ausgabe eines Shapers, es kann jedoch auch die Ausgabe aller anderen Qualifikationen sein.

Es folgt ein Beispiel für Tabellenprojektionen.

```json
{
    "name": "your-skillset",
    "skills": [
      …your skills
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [
            { "tableName": "MainTable", "generatedKeyName": "SomeId", "source": "/document/EnrichedShape" },
            { "tableName": "KeyPhrases", "generatedKeyName": "KeyPhraseId", "source": "/document/EnrichedShape/*/KeyPhrases/*" },
            { "tableName": "Entities", "generatedKeyName": "EntityId", "source": "/document/EnrichedShape/*/Entities/*" }
          ]
        },
        {
          "objects": [ ]
        },
        {
            "files": [ ]
        }
      ]
    }
}
```

Wie in diesem Beispiel veranschaulicht, werden die Schlüsselbegriffe und Entitäten in verschiedenen Tabellen modelliert und behalten einen Verweis auf das übergeordnete Element (MainTable) für jede Zeile bei.

Die folgende Abbildung ist eine Referenz für die Caselaw-Übung unter [Erste Schritte mit Wissensspeichern](knowledge-store-howto.md). In einem Szenario, bei dem ein Fall mehrere Auffassungen umfasst und jede Auffassung durch Identifizieren von darin enthaltenen Entitäten angereichert wird, können Sie die Projektionen wie hier gezeigt modellieren.

![Entitäten und Beziehungen in Tabellen](media/knowledge-store-projection-overview/TableRelationships.png "Modellieren von Beziehungen in Tabellenprojektionen")

## <a name="object-projections"></a>Objektprojektionen

Objektprojektionen sind JSON-Darstellungen der Anreicherungsstruktur, die aus beliebigen Knoten erstellt werden können. In vielen Fällen kann über die Qualifikation **Shaper**, mit der eine Tabellenprojektion erstellt wird, auch eine Objektprojektion generiert werden. 

```json
{
    "name": "your-skillset",
    "skills": [
      …your skills
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [ ]
        },
        {
          "objects": [
            {
              "storageContainer": "Reviews", 
              "format": "json", 
              "source": "/document/Review", 
              "key": "/document/Review/Id" 
            }
          ]
        },
        {
            "files": [ ]
        }
      ]
    }
}
```

Zum Generieren einer Objektprojektion sind einige objektspezifische Attribute erforderlich:

+ storageContainer: Der Container, in dem die Objekte gespeichert werden.
+ source: Der Pfad zum Knoten der Anreicherungsstruktur, der als Stamm der Projektion dient.
+ key: Ein Pfad, der einen eindeutigen Schlüssel für das zu speichernde Objekt darstellt. Er wird verwendet, um den Namen des Blobs im Container zu erstellen.

## <a name="file-projection"></a>Dateiprojektion

Dateiprojektionen ähneln Objektprojektionen und werden nur für die Sammlung `normalized_images` durchgeführt. Ähnlich wie Objektprojektionen werden Dateiprojektionen im Blobcontainer mit dem Ordnerpräfix des Base64-codierten Werts der Dokument-ID gespeichert. Dateiprojektionen können nicht denselben Container wie Objektprojektionen verwenden und müssen in einen anderen Container projiziert werden.

```json
{
    "name": "your-skillset",
    "skills": [
      …your skills
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [ ]
        },
        {
          "objects": [ ]
        },
        {
            "files": [
                 {
                  "storageContainer": "ReviewImages",
                  "source": "/document/normalized_images/*"
                }
            ]
        }
      ]
    }
}
```

## <a name="projection-lifecycle"></a>Lebenszyklus von Projektionen

Der Lebenszyklus der Projektionen ist an die Quelldaten in der Datenquelle gebunden. Wenn die Daten aktualisiert und neu indiziert werden, werden die Projektionen mit den Ergebnissen der Anreicherungen aktualisiert. So wird sichergestellt, dass die Projektionen konsistent mit den Daten in der Datenquelle sind. Die Projektionen erben die Löschrichtlinie, die Sie für den Index konfiguriert haben. Projektionen werden nicht gelöscht, wenn der Indexer oder der Suchdienst selbst gelöscht wird.

## <a name="using-projections"></a>Verwenden von Projektionen

Nach dem Ausführen des Indexers können Sie die projizierten Daten in den Containern oder Tabellen lesen, die Sie durch Projektionen angegeben haben.

Für die Analyse erfolgt die Untersuchung in Power BI einfach durch das Festlegen von Azure Table Storage als Datenquelle. Sie können auf einfache Weise eine Reihe von Visualisierungen für Ihre Daten erstellen, indem Sie die darin enthaltenen Beziehungen verwenden.

Wenn Sie die angereicherten Daten in einer Data Science-Pipeline verwenden möchten, können Sie alternativ [die Daten aus Blobs in einen Pandas-Datenrahmen laden](../machine-learning/team-data-science-process/explore-data-blob.md).

Wenn Sie Ihre Daten schließlich aus dem Wissensspeicher exportieren möchten, enthält Azure Data Factory Connectors zum Exportieren der Daten in die gewünschte Datenbank. 

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie als nächsten Schritt entsprechend den Anweisungen Ihren ersten Wissensspeicher unter Verwendung von Beispieldaten.

> [!div class="nextstepaction"]
> [Erstellen eines Wissensspeichers](knowledge-store-howto.md).
