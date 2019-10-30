---
title: 'Beispiel: Modellieren der AdventureWorks Inventory-Datenbank'
titleSuffix: Azure Cognitive Search
description: Es wird beschrieben, wie Sie relationale Daten modellieren und diese in ein vereinfachtes Dataset transformieren, um in der kognitiven Azure-Suche die Indizierung und die Volltextsuche zu ermöglichen.
author: HeidiSteen
manager: nitinme
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: heidist
ms.openlocfilehash: edb6162724938962df8a7340afea6e930a0b1049
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792992"
---
# <a name="example-model-the-adventureworks-inventory-database-for-azure-cognitive-search"></a>Beispiel: Modellieren der AdventureWorks Inventory-Datenbank für die kognitive Azure-Suche

Die kognitive Azure-Suche akzeptiert ein vereinfachtes Rowset als Eingabe für die [Indizierungspipeline](search-what-is-an-index.md) (Datenerfassung). Wenn die Quelldaten aus einer relationalen SQL Server-Datenbank stammen, veranschaulicht dieser Artikel einen Ansatz zum Erstellen eines vereinfachten Rowsets vor der Indizierung. Als Beispiel wird dabei die AdventureWorks-Beispieldatenbank verwendet.

## <a name="about-adventureworks"></a>Informationen zu AdventureWorks

Wenn Sie eine SQL Server-Instanz nutzen, sind Sie ggf. bereits mit der [AdventureWorks-Beispieldatenbank](https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017) vertraut. Unter den Tabellen dieser Datenbank sind fünf Tabellen mit Produktinformationen.

+ **ProductModel**: Name
+ **Product**: Name, Farbe, Kosten, Größe, Gewicht, Bild, Kategorie (jede Zeile ist mit einem bestimmten ProductModel verknüpft)
+ **ProductDescription**: Beschreibung
+ **ProductModelProductDescription**: Gebietsschema (in jeder Zeile wird ein ProductModel mit einer bestimmten ProductDescription für eine bestimmte Sprache verknüpft)
+ **ProductCategory**: Name, übergeordnete Kategorie

In diesem Beispiel besteht das Ziel darin, alle diese Daten in einem vereinfachten Rowset zusammenzufassen, das von einem Suchindex erfasst werden kann. 

## <a name="considering-our-options"></a>Mögliche Optionen

Ein etwas naiver Ansatz würde darin bestehen, alle Zeilen der Tabelle „Product“ (verknüpft, falls zutreffend) zu indizieren, da diese Tabelle über die präzisesten Informationen verfügt. Bei diesem Ansatz wird der Suchindex aber für als doppelt wahrgenommene Einträge eines Resultsets geöffnet. Das Modell „Road-650“ ist beispielsweise in zwei Farben und sechs Größen erhältlich. Eine Abfrage nach „road bikes“ ergibt dann vor allem zwölf Instanzen desselben Modells, das nur anhand der Größe und Farbe variiert. Die anderen sechs Modelle von Straßenfahrrädern erscheinen in den Suchergebnissen dann erst weit hinten, und zwar auf Seite 2.

  ![Produktliste](./media/search-example-adventureworks/products-list.png "Produktliste")
 
Beachten Sie, dass das Modell „Road-650“ über zwölf Optionen verfügt. 1:n-Entitätszeilen werden im Suchindex am besten als Felder mit mehreren Werten oder Felder mit vorab aggregierten Werten dargestellt.

Das Lösen dieses Problems ist schwieriger als das Verschieben des Zielindex in die Tabelle „ProductModel“. Hierbei würden die wichtigen Daten in der Tabelle „Product“ ignoriert werden, die in den Suchergebnissen aber trotzdem enthalten sein sollten.

## <a name="use-a-collection-data-type"></a>Verwenden eines „Collection“-Datentyps

Der „richtige Ansatz“ ist die Nutzung eines Suchschemafeatures, das im Datenbankmodell nicht über eine direkte Entsprechung verfügt: **Collection(Edm.String)** . Dieses Konstrukt wird im Indexschema für die kognitive Azure-Suche definiert. Ein Collection-Datentyp wird verwendet, wenn Sie anstelle einer sehr langen (einzelnen) Zeichenfolge eine Liste mit individuellen Zeichenfolgen darstellen müssen. Wenn Sie Tags oder Schlüsselwörter nutzen, würden Sie für dieses Feld einen Collection-Datentyp verwenden.

Indem Sie Felder mit mehreren Werten vom Typ **Collection(Edm.String)** für „color“, „size“ und „image“ definieren, werden die Hilfsinformationen für die Facettierung und Filterung beibehalten, ohne dass im Index doppelte Einträge auftreten. Wenden Sie entsprechend Aggregatfunktionen auf die numerischen Felder von „Product“ an, und indizieren Sie **minListPrice** – anstatt **listPrice** für jedes einzelne Produkt.

Bei einem Index mit diesen Strukturen würde eine Suche nach „mountain bikes“ die Anzeige von einzelnen Fahrradmodellen ergeben, während wichtige Metadaten wie Farbe, Größe und niedrigster Preis erhalten bleiben. Der folgende Screenshot enthält eine entsprechende Darstellung.

  ![Beispiel für die Suche nach Mountainbikes](./media/search-example-adventureworks/mountain-bikes-visual.png "Beispiel für die Suche nach Mountainbikes")

## <a name="use-script-for-data-manipulation"></a>Verwenden eines Skripts für die Datenbearbeitung

Leider lässt sich diese Art der Modellierung allein durch SQL-Anweisungen nicht ohne Weiteres erreichen. Verwenden Sie stattdessen ein einfaches NodeJS-Skript, um die Daten zu laden und dann den für die Suche geeigneten JSON-Entitäten zuzuordnen.

Die endgültige Zuordnung von Datenbank und Suche sieht wie folgt aus:

+ model (Edm.String: searchable, filterable, retrievable) aus „ProductModel.Name“
+ description_en (Edm.String: searchable) aus „ProductDescription“ für das Modell mit culture=’en’
+ color (Collection(Edm.String): searchable, filterable, facetable, retrievable): eindeutige Werte aus „Product.Color“ für das Modell
+ size (Collection(Edm.String): searchable, filterable, facetable, retrievable): eindeutige Werte aus „Product.Size“ für das Modell
+ image (Collection(Edm.String): retrievable): eindeutige Werte aus „Product.ThumbnailPhoto“ für das Modell
+ minStandardCost (Edm.Double: filterable, facetable, sortable, retrievable): Aggregatminimum aller „Product.StandardCost“-Einträge für das Modell
+ minListPrice (Edm.Double: filterable, facetable, sortable, retrievable): Aggregatminimum aller „Product.ListPrice“-Einträge für das Modell
+ minWeight (Edm.Double: filterable, facetable, sortable, retrievable): Aggregatminimum aller „Product.Weight“-Einträge für das Modell
+ products (Collection(Edm.String): searchable, filterable, retrievable): eindeutige Werte aus „Product.Name“ für das Modell

Nachdem die Tabelle „ProductModel“ mit „Product“ und „ProductDescription“ verknüpft wurde, können Sie [lodash](https://lodash.com/) (oder Linq in C#) verwenden, um das Resultset schnell zu transformieren:

```javascript
var records = queryYourDatabase();
var models = _(records)
  .groupBy('ModelName')
  .values()
  .map(function(d) {
    return {
      model: _.first(d).ModelName,
      description: _.first(d).Description,
      colors: _(d).pluck('Color').uniq().compact().value(),
      products: _(d).pluck('ProductName').uniq().compact().value(),
      sizes: _(d).pluck('Size').uniq().compact().value(),
      images: _(d).pluck('ThumbnailPhotoFilename').uniq().compact().value(),
      minStandardCost: _(d).pluck('StandardCost').min(),
      maxStandardCost: _(d).pluck('StandardCost').max(),
      minListPrice: _(d).pluck('ListPrice').min(),
      maxListPrice: _(d).pluck('ListPrice').max(),
      minWeight: _(d).pluck('Weight').min(),
      maxWeight: _(d).pluck('Weight').max(),
    };
  })
  .value();
```

Der sich ergebende JSON-Code sieht wie folgt aus:

```json
[
  {
    "model": "HL Road Frame",
    "colors": [
      "Black",
      "Red"
    ],
    "products": [
      "HL Road Frame - Black, 58",
      "HL Road Frame - Red, 58",
      "HL Road Frame - Red, 62",
      "HL Road Frame - Red, 44",
      "HL Road Frame - Red, 48",
      "HL Road Frame - Red, 52",
      "HL Road Frame - Red, 56",
      "HL Road Frame - Black, 62",
      "HL Road Frame - Black, 44",
      "HL Road Frame - Black, 48",
      "HL Road Frame - Black, 52"
    ],
    "sizes": [
      "58",
      "62",
      "44",
      "48",
      "52",
      "56"
    ],
    "images": [
      "no_image_available_small.gif"
    ],
    "minStandardCost": 868.6342,
    "maxStandardCost": 1059.31,
    "minListPrice": 1431.5,
    "maxListPrice": 1431.5,
    "minWeight": 961.61,
    "maxWeight": 1043.26
  }
]
```

Abschließend ist hier die SQL-Abfrage zum Zurückgeben des anfänglichen Recordsets angegeben. Ich habe das npm-Modul [mssql](https://www.npmjs.com/package/mssql) verwendet, um die Daten in meine NodeJS-App zu laden.

```T-SQL
SELECT
  m.Name as ModelName,
  d.Description,
  p.Name as ProductName,
  p.*
FROM 
  SalesLT.ProductModel m
INNER JOIN 
  SalesLT.ProductModelProductDescription md
  ON m.ProductModelId = md.ProductModelId
INNER JOIN 
  SalesLT.ProductDescription d
  ON md.ProductDescriptionId = d.ProductDescriptionId
LEFT JOIN 
  SalesLT.product p
  ON m.ProductModelId = p.ProductModelId
WHERE
  md.Culture='en'
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Beispiel: Facet-Taxonomien mit mehreren Ebenen in der kognitiven Azure-Suche](search-example-adventureworks-multilevel-faceting.md)