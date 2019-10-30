---
title: Modellieren von relationalen SQL-Daten für Import und Indizierung
titleSuffix: Azure Cognitive Search
description: Es wird beschrieben, wie Sie relationale Daten modellieren und diese in ein vereinfachtes Resultset denormalisieren, um in der kognitiven Azure-Suche die Indizierung und die Volltextsuche zu ermöglichen.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 3b973dd05d23d190c77986ca9bf6d39656739cd8
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72790096"
---
# <a name="how-to-model-relational-sql-data-for-import-and-indexing-in-azure-cognitive-search"></a>Modellieren von relationalen SQL-Daten für Import und Indizierung in der kognitiven Azure-Suche

Die kognitive Azure-Suche akzeptiert ein vereinfachtes Rowset als Eingabe für die [Indizierungspipeline](search-what-is-an-index.md). Wenn Ihre Quelldaten aus verknüpften Tabellen in einer relationalen SQL Server-Datenbank stammen, wird in diesem Artikel erläutert, wie das Resultset erstellt und eine Beziehung zwischen übergeordneten und untergeordneten Elementen in einem Index für die kognitive Azure-Suche modelliert wird.

Zur Veranschaulichung verwenden wir eine hypothetische Hoteldatenbank, die auf [Demodaten](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/hotels) basiert. Angenommen, die Datenbank besteht aus einer Tabelle „Hotels$“ mit 50 Hotels und einer Tabelle „Rooms$“ mit insgesamt 750 Zimmern unterschiedlicher Typen, Preise und Ausstattungen. Zwischen den Tabellen besteht eine 1:n-Beziehung. In unserem Ansatz stellt eine Sicht die Abfrage bereit, die 50 Zeilen zurückgibt, eine Zeile pro Hotel, wobei zugeordnete Zimmerdetails in die einzelnen Zeilen eingebettet sind.

   ![Tabellen und Sicht in der Hoteldatenbank](media/index-sql-relational-data/hotels-database-tables-view.png "Tabellen und Sicht in der Hoteldatenbank")


## <a name="the-problem-of-denormalized-data"></a>Das Problem denormalisierter Daten

Eine der Herausforderungen bei der Arbeit mit 1:n-Beziehungen besteht darin, dass Standardabfragen, die auf verknüpften Tabellen basieren, denormalisierte Daten zurückgeben, was in einem Szenario der kognitiven Azure-Suche nicht gut funktioniert. Sehen Sie sich das folgende Beispiel an, das Hotels und Räume verknüpft.

```sql
SELECT * FROM Hotels$
INNER JOIN Rooms$
ON Rooms$.HotelID = Hotels$.HotelID
```
Als Ergebnis dieser Abfrage werden alle Hotel-Felder zurückgegeben, gefolgt von allen Room-Feldern, wobei sich grundlegende Hotelinformationen für jeden Room-Wert wiederholen.

   ![Denormalisierte Daten, redundante Hoteldaten, wenn Room-Felder hinzugefügt werden](media/index-sql-relational-data/denormalize-data-query.png "Denormalisierte Daten, redundante Hoteldaten, wenn Room-Felder hinzugefügt werden")


Diese Abfrage ist zwar oberflächlich erfolgreich (alle Daten werden in einem vereinfachten Rowset bereitgestellt), kann aber nicht die richtige Dokumentstruktur für die erwartete Sucherfahrung bereitstellen. Während der Indizierung erstellt die kognitive Azure-Suche ein Suchdokument für jede erfasste Zeile. Wenn Ihre Suchdokumente wie die oben aufgeführten Ergebnisse aussehen, wären auch Duplikate vorhanden, sieben separate Dokumente allein schon für das Hotel Twin Dome. Eine Abfrage nach „Hotels in Florida“ würde sieben Ergebnisse nur für das Hotel Twin Dome zurückgeben, wodurch weitere relevante Hotels in den Suchergebnissen weiter nach unten rücken.

Um die erwartete Darstellung eines Dokuments pro Hotel zu erhalten, sollte ein Rowset mit der richtigen Granularität, jedoch mit umfassenden Informationen bereitgestellt werden. Glücklicherweise können Sie dies problemlos erreichen, indem Sie die in diesem Artikel beschriebenen Techniken anwenden.

## <a name="define-a-query-that-returns-embedded-json"></a>Definieren einer Abfrage, die eingebetteten JSON-Code zurückgibt

Um die erwartete Suchumgebung bereitzustellen, sollte Ihr Dataset aus einer Zeile für jedes Suchdokument in der kognitiven Azure-Suche bestehen. In unserem Beispiel möchten wir eine Zeile für jedes Hotel bereitstellen, aber die Benutzer sollen auch in der Lage sein, nach anderen zimmerbezogenen Feldern zu suchen, die für sie wichtig sind, wie z.B. Preis pro Nacht, Größe und Anzahl der Betten oder Strandblick – allen Informationen, die Teil der Zimmerdetails sind.

Die Lösung besteht darin, die Zimmerdetails als geschachtelten JSON-Code zu erfassen und die JSON-Struktur dann in ein Feld in einer Sicht einzufügen, wie im zweiten Schritt gezeigt. 

1. Angenommen, Sie verfügen über zwei verknüpfte Tabellen, „Hotels$“ und „Rooms$“, die Details über 50 Hotels und 750 Zimmer enthalten und über das Feld „HotelID“ verknüpft sind. Diese Tabellen enthalten einzeln 50 Hotels bzw. 750 zugehörige Zimmer.

    ```sql
    CREATE TABLE [dbo].[Hotels$](
      [HotelID] [nchar](10) NOT NULL,
      [HotelName] [nvarchar](255) NULL,
      [Description] [nvarchar](max) NULL,
      [Description_fr] [nvarchar](max) NULL,
      [Category] [nvarchar](255) NULL,
      [Tags] [nvarchar](255) NULL,
      [ParkingIncluded] [float] NULL,
      [SmokingAllowed] [float] NULL,
      [LastRenovationDate] [smalldatetime] NULL,
      [Rating] [float] NULL,
      [StreetAddress] [nvarchar](255) NULL,
      [City] [nvarchar](255) NULL,
      [State] [nvarchar](255) NULL,
      [ZipCode] [nvarchar](255) NULL,
      [GeoCoordinates] [nvarchar](255) NULL
    ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
    GO

    CREATE TABLE [dbo].[Rooms$](
      [HotelID] [nchar](10) NULL,
      [Description] [nvarchar](255) NULL,
      [Description_fr] [nvarchar](255) NULL,
      [Type] [nvarchar](255) NULL,
      [BaseRate] [float] NULL,
      [BedOptions] [nvarchar](255) NULL,
      [SleepsCount] [float] NULL,
      [SmokingAllowed] [float] NULL,
      [Tags] [nvarchar](255) NULL
    ) ON [PRIMARY]
    GO
    ```

2. Erstellen Sie eine Sicht die alle Felder in der übergeordneten Tabelle (`SELECT * from dbo.Hotels$`) enthält, und fügen Sie ein neues *Rooms*-Feld hinzu, das die Ausgabe einer geschachtelten Abfrage enthält. Eine **FOR JSON AUTO**-Klausel für `SELECT * from dbo.Rooms$` strukturiert die Ausgabe als JSON. 

     ```sql
   CREATE VIEW [dbo].[HotelRooms]
   AS
   SELECT *, (SELECT *
              FROM dbo.Rooms$
              WHERE dbo.Rooms$.HotelID = dbo.Hotels$.HotelID FOR JSON AUTO) AS Rooms
   FROM dbo.Hotels$
   GO
   ```

   Der folgende Screenshot zeigt die resultierende Sicht mit dem nvarchar-Feld *Rooms* unten. Das Feld *Rooms* ist nur in der Sicht „HotelRooms“ vorhanden.

   ![HotelRooms-Sicht](media/index-sql-relational-data/hotelsrooms-view.png "HotelRooms-Sicht")

1. Führen Sie `SELECT * FROM dbo.HotelRooms` aus, um das Rowset abzurufen. Diese Abfrage gibt 50 Zeilen, eine pro Hotel, mit zugeordneten Zimmerinformationen als JSON-Sammlung zurück. 

   ![Rowset aus der HotelRooms-Sicht](media/index-sql-relational-data/hotelrooms-rowset.png "Rowset aus der HotelRooms-Sicht")

Dieses Rowset kann nun in die kognitive Azure-Suche importiert werden.

> [!NOTE]
> Bei dieser Vorgehensweise wird davon ausgegangen, dass eingebetteter JSON-Code die [maximalen Spaltengrößenbeschränkungen von SQL Server](https://docs.microsoft.com/sql/sql-server/maximum-capacity-specifications-for-sql-server) nicht überschreitet. Wenn Ihre Daten nicht geeignet sind, können Sie eine programmgesteuerte Vorgehensweise ausprobieren, wie im [Beispiel: Modellieren der AdventureWorks Inventory-Datenbank für die kognitive Azure-Suche](search-example-adventureworks-modeling.md) veranschaulicht.

 ## <a name="use-a-complex-collection-for-the-many-side-of-a-one-to-many-relationship"></a>Verwenden einer komplexen Sammlung für die „n“-Seite einer 1: n-Beziehung

Erstellen Sie auf der Seite für die kognitive Azure-Suche ein Indexschema, das die 1:n-Beziehung mithilfe von geschachteltem JSON modelliert. Das Resultset, das Sie im vorherigen Abschnitt erstellt haben, entspricht im Allgemeinen dem unten angegebenen Indexschema (aus Platzgründen haben wir einige Felder gekürzt).

Das folgende Beispiel ähnelt dem Beispiel in [Modellieren komplexer Datentypen](search-howto-complex-data-types.md#creating-complex-fields). Die *Rooms*-Struktur, die im Mittelpunkt dieses Artikels steht, befindet sich in der Feldsammlung eines Indexes namens *Hotels*. Dieses Beispiel zeigt auch einen komplexen Typ für *Address*, der sich von *Rooms* darin unterscheidet, dass er aus einem festgelegten Satz von Elementen besteht, im Gegensatz zu der in einer Sammlung zulässigen beliebigen Anzahl von Elementen.

```json
{
  "name": "hotels",
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
    { "name": "Description_fr", "type": "Edm.String", "searchable": true, "analyzer": "fr.lucene" },
    { "name": "Category", "type": "Edm.String", "searchable": true, "filterable": false },
    { "name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "facetable": true },
    { "name": "Address", "type": "Edm.ComplexType",
      "fields": [
        { "name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
        { "name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
        { "name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true }
      ]
    },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)",
      "fields": [
        { "name": "Description", "type": "Edm.String", "searchable": true, "analyzer": "en.lucene" },
        { "name": "Description_fr", "type": "Edm.String", "searchable": true, "analyzer": "fr.lucene" },
        { "name": "Type", "type": "Edm.String", "searchable": true },
        { "name": "BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true },
        { "name": "BedOptions", "type": "Edm.String", "searchable": true, "filterable": true, "facetable": true },
        { "name": "SleepsCount", "type": "Edm.Int32", "filterable": true, "facetable": true },
        { "name": "SmokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true },
        { "name": "Tags", "type": "Edm.Collection", "searchable": true }
      ]
    }
  ]
}
```

Mit dem vorherigen Resultset und dem oben aufgeführten Indexschema verfügen Sie über alle erforderlichen Komponenten für einen erfolgreichen Indizierungsvorgang. Das vereinfachte Dataset erfüllt die Indizierungsanforderungen, behält jedoch Detailinformationen bei. Im Index für die kognitive Azure-Suche werden Suchergebnisse problemlos in die hotelbasierten Entitäten eingeteilt, wobei der Kontext einzelner Räume und ihre Attribute beibehalten werden.

## <a name="next-steps"></a>Nächste Schritte

Sie können den [Datenimport-Assistenten](search-import-data-portal.md) mit Ihrem eigenen Dataset zum Erstellen und Laden des Index verwenden. Der Assistent erkennt die eingebettete JSON-Sammlung, z.B. die in *Rooms* enthaltene Sammlung, und leitet ein Indexschema ab, das eine Sammlung komplexer Typen enthält. 

  ![Vom Datenimport-Assistenten abgeleiteter Index](media/index-sql-relational-data/search-index-rooms-complex-collection.png "Vom Datenimport-Assistenten abgeleiteter Index")

Verwenden Sie den folgenden Schnellstart, um die grundlegenden Schritte des Datenimport-Assistenten kennen zu lernen.

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen eines Suchindex über das Azure-Portal](search-get-started-portal.md)