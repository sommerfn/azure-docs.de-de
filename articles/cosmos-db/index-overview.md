---
title: Indizierung in Azure Cosmos DB
description: Erhalten Sie Informationen zur Funktionsweise der Indizierung in Azure Cosmos DB.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: thweiss
ms.openlocfilehash: 4d961f8635a52a09011543b793ce8a87eaa4ea9e
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70914195"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>Indizierung in Azure Cosmos DB: Übersicht

Azure Cosmos DB ist eine schemaunabhängige Datenbank, die es Ihnen ermöglicht, Ihre Anwendung zu durchlaufen, ohne sich mit Schema- oder Indexverwaltung befassen zu müssen. Standardmäßig indiziert Azure Cosmos DB automatisch alle Eigenschaften für sämtliche Elemente in Ihrem [Container](databases-containers-items.md#azure-cosmos-containers), ohne dass ein Schema definiert oder Sekundärindizes konfiguriert werden müssen.

In diesem Artikel wird beschrieben, wie Azure Cosmos DB Daten indiziert und wie Indizes zur Verbesserung der Abfrageleistung verwendet werden. Sie sollten zunächst diesen Abschnitt lesen, bevor Sie sich mit dem Anpassen von [Indizierungsrichtlinien](index-policy.md) beschäftigen.

## <a name="from-items-to-trees"></a>Von Elementen zu Strukturen

Bei jedem Speichern eines Elements in einem Container wird dessen Inhalt als ein JSON-Dokument projiziert und anschließend in eine strukturierte Darstellung konvertiert. Das bedeutet, dass jede Eigenschaft dieses Elements als Knoten in einer Struktur dargestellt wird. Als übergeordnetes Element für alle Eigenschaften auf oberster Ebene des Elements wird ein Pseudostammknoten erstellt. Die Blattknoten enthalten die eigentlichen Skalarwerte eines Elements.

Betrachten Sie z. B. dieses Element:

```json
    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 },
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }
```

Es wird durch die folgende Struktur dargestellt:

![Das vorherige Element als Struktur dargestellt](./media/index-overview/item-as-tree.png)

Beachten Sie, wie Arrays in der Struktur codiert werden: Jeder Eintrag in einem Array erhält direkt einen Zwischenknoten, der mit dem Index dieses Eintrags innerhalb des Arrays bezeichnet ist (0, 1 usw.).

## <a name="from-trees-to-property-paths"></a>Von Strukturen zu Eigenschaftenpfaden

Azure Cosmos DB transformiert die Elemente in Strukturen, da so über die Pfade innerhalb dieser Strukturen auf Elemente verwiesen werden kann. Um den Pfad für eine Eigenschaft abzurufen, können Sie die Struktur vom Stammknoten aus bis zu dieser Eigenschaft durchlaufen. Verketten Sie dabei die Bezeichnungen aller durchlaufenen Knoten.

Dies sind die Pfade für jede Eigenschaft aus dem oben beschriebenen Beispielelement:

    /locations/0/country: "Germany"
    /locations/0/city: "Berlin"
    /locations/1/country: "France"
    /locations/1/city: "Paris"
    /headquarters/country: "Belgium"
    /headquarters/employees: 250
    /exports/0/city: "Moscow"
    /exports/1/city: "Athens"

Wenn ein Element geschrieben wird, indiziert Azure Cosmos DB den Pfad jeder Eigenschaft und den zugehörigen Wert.

## <a name="index-kinds"></a>Indextypen

Azure Cosmos DB unterstützt derzeit drei Arten von Indizes:

Der **Range**-Indextyp wird für Folgendes verwendet:

- Gleichheitsabfragen:

    ```sql
   SELECT * FROM container c WHERE c.property = 'value'
   ```

- Bereichsabfragen:

   ```sql
   SELECT * FROM container c WHERE c.property > 'value'
   ```
  (funktioniert für `>`, `<`, `>=`, `<=`, `!=`)

- `ORDER BY` fragt Folgendes ab:

   ```sql 
   SELECT * FROM container c ORDER BY c.property
   ```

- `JOIN` fragt Folgendes ab:

   ```sql
   SELECT child FROM container c JOIN child IN c.properties WHERE child = 'value'
   ```

Range-Indizes können für Skalarwerte (Zeichenfolge oder Zahl) verwendet werden.

Der **Spatial**-Indextyp wird für Folgendes verwendet:

- Abfragen zum räumlichen Abstand: 

   ```sql
   SELECT * FROM container c WHERE ST_DISTANCE(c.property, { "type": "Point", "coordinates": [0.0, 10.0] }) < 40
   ```

- Räumliche Abfragen: 

   ```sql
   SELECT * FROM container c WHERE ST_WITHIN(c.property, {"type": "Point", "coordinates": [0.0, 10.0] } })
   ```

Spatial-Indizes können für ordnungsgemäß formatierte [GeoJSON](geospatial.md)-Objekte verwendet werden. Derzeit werden Point, LineString, Polygon und MultiPolygon unterstützt.

Der **zusammengesetzte** Index wird für Folgendes verwendet:

- `ORDER BY` fragt mehrere Eigenschaften ab:

```sql
 SELECT * FROM container c ORDER BY c.property1, c.property2
```

- Abfragen mit einem Filter und `ORDER BY`. Diese Abfragen können einen zusammengesetzten Index verwenden, wenn die Filter-Eigenschaft der `ORDER BY`-Klausel hinzugefügt wird.

```sql
 SELECT * FROM container c WHERE c.property1 = 'value' ORDER BY c.property1, c.property2
```

- Abfragen mit einem Filter für zwei oder mehr Eigenschaften, bei denen mindestens eine Eigenschaft ein Gleichheitsfilter ist

```sql
 SELECT * FROM container c WHERE c.property1 = 'value' AND c.property2 > 'value'
```

## <a name="querying-with-indexes"></a>Abfragen mit Indizes

Die während der Indizierung der Daten extrahierten Pfade vereinfachen das Suchen im Index bei der Verarbeitung einer Abfrage. Durch einen Abgleich der `WHERE`-Klausel einer Abfrage mit der Liste der indizierten Pfade ist es möglich, sehr schnell die Elemente zu ermitteln, die dem Abfrageprädikat entsprechen.

Betrachten Sie beispielsweise die folgende Abfrage: `SELECT location FROM location IN company.locations WHERE location.country = 'France'`. Das Abfrageprädikat (nach Elementen filtern, die an beliebiger Stelle „France“ als Land enthalten) würde dem Pfad entsprechen, der rot hervorgehoben ist:

![Abgleichen mit einem bestimmten Pfad in einer Struktur](./media/index-overview/matching-path.png)

> [!NOTE]
> Eine `ORDER BY`-Klausel, die anhand einer einzelnen Eigenschaft geordnet wird, benötigt *immer* einen Range-Index. Falls der referenzierte Pfad diesen nicht aufweist, tritt ein Fehler auf. Ebenso benötigt eine `ORDER BY`-Abfrage, die nach mehreren Eigenschaften sortiert, *immer* einen zusammengesetzten Index.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Indizierung erhalten Sie in den folgenden Artikeln:

- [Indizierungsrichtlinie](index-policy.md)
- [Gewusst wie: Verwalten der Indizierungsrichtlinie](how-to-manage-indexing-policy.md)
