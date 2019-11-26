---
title: 'C#-Tutorial: Indizieren von Daten aus Azure SQL-Datenbanken'
titleSuffix: Azure Cognitive Search
description: Dieses C#-Codebeispiel veranschaulicht, wie Sie eine Verbindung mit Azure SQL-Datenbank herstellen, durchsuchbare Daten extrahieren und diese in einen Azure Cognitive Search-Index laden.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 4e8097eeb07420bee4ba30eb0fedbe5d4db2db9d
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74113330"
---
# <a name="c-tutorial-import-azure-sql-database-using-azure-cognitive-search-indexers"></a>C#-Tutorial: Importieren einer Azure SQL-Datenbank unter Verwendung von Azure Cognitive Search-Indexern

Sie erfahren, wie Sie einen Indexer konfigurieren, um durchsuchbare Daten aus einer Azure SQL-Beispieldatenbank zu extrahieren. Ein [Indexer](search-indexer-overview.md) ist eine Komponente von Azure Cognitive Search, die externe Datenquellen durchforstet und einen [Suchindex](search-what-is-an-index.md) auffüllt. Von allen Indexern wird der Indexer für Azure SQL-Datenbank am häufigsten verwendet. 

Es ist hilfreich, wenn Sie bereits mit dem Konfigurieren von Indexern vertraut sind, da dies das Schreiben und Verwalten von Code vereinfacht. Anstatt ein schemakonformes JSON-Dataset vorzubereiten und mithilfe von Push zu übermitteln, können Sie einen Indexer an eine Datenquelle anfügen und ihn Daten extrahieren und in einen Index einfügen lassen. Außerdem können Sie den Indexer optional auf der Grundlage eines Wiederholungszeitplans ausführen, um Änderungen in der zugrunde liegenden Quelle zu erfassen.

In diesem Tutorial verwenden Sie die [Azure Cognitive Search-.NET-Clientbibliotheken](https://aka.ms/search-sdk) und eine .NET Core-Konsolenanwendung, um folgende Aufgaben auszuführen:

> [!div class="checklist"]
> * Hinzufügen von Suchdienstinformationen zu Anwendungseinstellungen
> * Vorbereiten eines externen Datasets in Azure SQL-Datenbank 
> * Überprüfen der Index- und Indexerdefinition im Beispielcode
> * Ausführen des Indexercodes zum Importieren von Daten
> * Durchsuchen des Index
> * Anzeigen der Indexerkonfiguration im Portal

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Schnellstart werden die folgenden Dienste, Tools und Daten verwendet. 

[Erstellen Sie einen Azure Cognitive Search-Dienst](search-create-service-portal.md), oder [suchen Sie nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in Ihrem aktuellen Abonnement. In diesem Tutorial können Sie einen kostenlosen Dienst verwenden.

[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) speichert die von einem Indexer verwendete externe Datenquelle. In der Beispielprojektmappe steht eine SQL-Datendatei zum Erstellen der Tabelle zur Verfügung. In diesem Tutorial werden die Schritte zum Erstellen des Diensts und der Datenbank erläutert.

Eine beliebige Version von [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) kann zum Ausführen der Beispielprojektmappe verwendet werden. Der Beispielcode und die Anleitung wurden in der kostenlosen Community-Edition getestet.

[Azure-Samples/search-dotnet-getting-started](https://github.com/Azure-Samples/search-dotnet-getting-started) enthält die Beispielprojektmappe (im GitHub-Repository mit Azure-Beispielen). Laden Sie die Projektmappe herunter, und extrahieren Sie sie. Projektmappen sind standardmäßig schreibgeschützt. Klicken Sie mit der rechten Maustaste auf die Projektmappe, und löschen Sie das Nur-Lese-Attribut, damit Sie Dateien ändern können.

> [!Note]
> Bei Verwendung des kostenlosen Azure Cognitive Search-Diensts gilt eine Obergrenze von drei Indizes, drei Indexern und drei Datenquellen. In diesem Tutorial wird davon jeweils eine Instanz erstellt. Vergewissern Sie sich, dass Ihr Dienst über genügend freie Kapazität für die neuen Ressourcen verfügt.

## <a name="get-a-key-and-url"></a>Abrufen eines Schlüssels und einer URL

Für REST-Aufrufe sind die Dienst-URL und ein Zugriffsschlüssel für jede Anforderung erforderlich. Ein Suchdienst wird mit beidem erstellt. Gehen Sie daher wie folgt vor, um die erforderlichen Informationen zu erhalten, falls Sie Azure Cognitive Search Ihrem Abonnement hinzugefügt haben:

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com/), und rufen Sie auf der Seite **Übersicht** Ihres Suchdiensts die URL ab. Ein Beispiel für einen Endpunkt ist `https://mydemo.search.windows.net`.

1. Rufen Sie unter **Einstellungen** > **Schlüssel** einen Administratorschlüssel ab, um Vollzugriff auf den Dienst zu erhalten. Es gibt zwei austauschbare Administratorschlüssel – diese wurden zum Zweck der Geschäftskontinuität bereitgestellt, falls Sie einen Rollover für einen Schlüssel durchführen müssen. Für Anforderungen zum Hinzufügen, Ändern und Löschen von Objekten können Sie den primären oder den sekundären Schlüssel verwenden.

![Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels](media/search-get-started-postman/get-url-key.png "Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels")

Für alle an Ihren Dienst gesendeten Anforderungen ist ein API-Schlüssel erforderlich. Ein gültiger Schlüssel stellt anforderungsbasiert eine Vertrauensstellung her zwischen der Anwendung, die die Anforderung versendet, und dem Dienst, der sie verarbeitet.

## <a name="set-up-connections"></a>Einrichten von Verbindungen
Verbindungsinformationen für erforderliche Dienste werden in der Datei **appsettings.json** der Projektmappe angegeben. 

1. Öffnen Sie in Visual Studio die Datei **DotNetHowToIndexers.sln**.

1. Öffnen Sie im Projektmappen-Explorer die Datei **appsettings.json**, um die einzelnen Einstellungen auffüllen zu können.  

Die ersten beiden Einträge können Sie direkt angeben. Verwenden Sie dazu die URL und die Administratorschlüssel für Ihren Azure Cognitive Search-Dienst. Lautet der Endpunkt `https://mydemo.search.windows.net`, muss als Dienstname `mydemo` angegeben werden.

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "AzureSqlConnectionString": "Put your Azure SQL database connection string here",
}
```

Für den letzten Eintrag ist eine vorhandene Datenbank erforderlich. Sie erstellen sie im nächsten Schritt.

## <a name="prepare-sample-data"></a>Vorbereiten der Beispieldaten

In diesem Schritt erstellen Sie eine externe Datenquelle, die ein Indexer durchforsten kann. Das Dataset in Azure SQL-Datenbank kann über das Azure-Portal unter Verwendung der Datei *hotels.sql* aus dem Beispiel erstellt werden. Azure Cognitive Search nutzt vereinfachte Rowsets, wie sie beispielsweise auf der Grundlage einer Sicht oder Abfrage generiert werden. Die SQL-Datei in der Beispielprojektmappe erstellt eine einzelne Tabelle und füllt sie auf.

In der folgenden Übung wird davon ausgegangen, dass weder ein Server noch eine Datenbank vorhanden ist. In Schritt 2 werden Sie daher aufgefordert, beides zu erstellen. Optional: Falls Sie bereits über eine Ressource verfügen, können Sie ihr die Hoteltabelle hinzufügen (ab Schritt 4).

1. [Melden Sie sich am Azure-Portal an.](https://portal.azure.com/) 

2. Navigieren Sie zu einer **Azure SQL-Datenbank**, oder erstellen Sie eine, um eine Datenbank, einen Server und eine Ressourcengruppe zu erstellen. Dabei können Sie die Standardwerte und den günstigsten Tarif verwenden. Die Erstellung eines Servers hat unter anderem den Vorteil, dass Sie einen Administratorbenutzernamen und ein Kennwort angeben können. Diese werden in einem späteren Schritt zum Erstellen und Laden von Tabellen benötigt.

   ![Seite für die neue Datenbank](./media/search-indexer-tutorial/indexer-new-sqldb.png)

3. Klicken Sie auf **Erstellen**, um den neuen Server und die Datenbank bereitzustellen. Warten Sie, bis Server und Datenbank bereitgestellt wurden.

4. Öffnen Sie für Ihre neue Datenbank die Seite „SQL-Datenbank“, sofern sie nicht bereits geöffnet ist. Der Ressourcenname sollte *SQL-Datenbank* lauten (nicht *SQL Server*).

   ![SQL-Datenbankseite](./media/search-indexer-tutorial/hotels-db.png)

4. Klicken Sie im Navigationsbereich auf **Abfrage-Editor (Vorschau)** .

5. Klicken Sie auf **Anmelden**, und geben Sie den Benutzernamen und das Kennwort des Serveradministrators ein.

6. Klicken Sie auf **Abfrage öffnen**, und navigieren Sie zum Speicherort von *hotels.sql*. 

7. Wählen Sie die Datei aus, und klicken Sie auf **Öffnen**. Das Skript sollte in etwa wie im folgenden Screenshot aussehen:

   ![SQL-Skript](./media/search-indexer-tutorial/sql-script.png)

8. Klicken Sie auf **Ausführen**, um die Abfrage auszuführen. Im Ergebnisbereich sollte eine Erfolgsmeldung für drei Zeilen angezeigt werden.

9. Wenn Sie ein Rowset aus dieser Tabelle zurückgeben möchten, können Sie als Überprüfungsschritt die folgende Abfrage ausführen:

    ```sql
    SELECT HotelId, HotelName, Tags FROM Hotels
    ```
    Die Prototypabfrage (`SELECT * FROM Hotels`) funktioniert im Abfrage-Editor nicht. Die Beispieldaten enthalten geografische Koordinaten im Feld „Standort“. Diese werden derzeit vom Editor nicht verarbeitet. Durch Ausführen der folgenden Anweisung erhalten Sie eine Liste mit weiteren abfragbaren Spalten: `SELECT * FROM sys.columns WHERE object_id = OBJECT_ID('dbo.Hotels')`

10. Sie verfügen nun über ein externes Dataset. Kopieren Sie als Nächstes die ADO.NET-Verbindungszeichenfolge für die Datenbank. Navigieren Sie auf der Seite „SQL-Datenbank“ Ihrer Datenbank zu **Einstellungen** > **Verbindungszeichenfolgen**, und kopieren Sie die ADO.NET-Verbindungszeichenfolge.
 
    Eine ADO.NET-Verbindungszeichenfolge sieht wie im folgenden Beispiel aus und muss mit einem gültigen Datenbanknamen, Benutzernamen und Kennwort versehen werden:

    ```sql
    Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
    ```
11. Fügen Sie in Visual Studio in der Datei **appsettings.json** als dritten Eintrag die Verbindungszeichenfolge in „AzureSqlConnectionString“ ein.

    ```json
    {
      "SearchServiceName": "<placeholder-Azure-Search-service-name>",
      "SearchServiceAdminApiKey": "<placeholder-admin-key-for-Azure-Search>",
      "AzureSqlConnectionString": "Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security  Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
    }
    ```

## <a name="understand-the-code"></a>Grundlegendes zum Code

Sobald die Daten und Konfigurationseinstellungen vorhanden sind, kann das Beispielprogramm **DotNetHowToIndexers.sln** kompiliert und ausgeführt werden. Nehmen Sie sich zuvor kurz Zeit, um sich die Index- und Indexerdefinition für dieses Beispiel etwas genauer anzusehen. Der relevante Code befindet sich in zwei Dateien:

  + **hotel.cs**: Enthält das Schema, das den Index definiert.
  + **Program.cs**: Enthält die Funktionen zum Erstellen und Verwalten von Strukturen in Ihrem Dienst.

### <a name="in-hotelcs"></a>hotel.cs

Das Indexschema definiert die Sammlung „fields“ einschließlich Attributen zur Angabe zulässiger Vorgänge (beispielsweise, ob in einem Feld eine Volltextsuche möglich ist oder ob es gefiltert oder sortiert werden kann), wie in der folgenden Felddefinition für „HotelName“ zu sehen. 

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
```

Ein Schema kann auch andere Elemente enthalten. Hierzu zählen beispielsweise Bewertungsprofile zur Erhöhung einer Suchbewertung, benutzerdefinierte Analysen und andere Konstrukte. Für unsere Zwecke reicht jedoch eine einfache Schemadefinition, die lediglich Felder aus den Beispieldatasets enthält.

In diesem Tutorial ruft der Indexer Daten aus einer einzelnen Datenquelle ab. In der Praxis können Sie mehrere Indexer an den gleichen Index anfügen und so einen konsolidierten durchsuchbaren Index erstellen, der auf mehreren Datenquellen basiert. Abhängig von Ihren Flexiblitätsanforderungen können Sie das gleiche Index-Indexer-Paar nutzen und nur die Datenquellen variieren oder einen einzelnen Index mit verschiedenen Indexer-/Datenquellenkombinationen verwenden.

### <a name="in-programcs"></a>Program.cs

Das Hauptprogramm enthält Logik zum Erstellen eines Clients, eines Index, einer Datenquelle und eines Indexers. Der Code sucht und löscht vorhandene Ressourcen mit dem gleichen Namen (unter der Annahme, dass Sie dieses Programm ggf. mehrmals ausführen).

Das Datenquellenobjekt wird mit Einstellungen konfiguriert, die speziell für Ressourcen von Azure SQL-Datenbank gelten, z. B. [inkrementelle Indizierung](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) zur Nutzung der integrierten [Features für die Erkennung von Änderungen](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) von Azure SQL. Die Demodatenbank „hotels“ in Azure SQL enthält eine Spalte für Vorläufiges Löschen mit dem Namen **IsDeleted**. Wenn diese Spalte in der Datenbank auf „true“ festgelegt ist, entfernt der Indexer das entsprechende Dokument aus dem Azure Cognitive Search-Index.

  ```csharp
  Console.WriteLine("Creating data source...");

  DataSource dataSource = DataSource.AzureSql(
      name: "azure-sql",
      sqlConnectionString: configuration["AzureSQLConnectionString"],
      tableOrViewName: "hotels",
      deletionDetectionPolicy: new SoftDeleteColumnDeletionDetectionPolicy(
          softDeleteColumnName: "IsDeleted",
          softDeleteMarkerValue: "true"));
  dataSource.DataChangeDetectionPolicy = new SqlIntegratedChangeTrackingPolicy();

  searchService.DataSources.CreateOrUpdateAsync(dataSource).Wait();
  ```

Ein Indexerobjekt ist plattformagnostisch. Dies bedeutet, dass die Konfiguration, die Zeitplanung und das Aufrufen unabhängig von der Quelle gleich sind. Dieses Indexerbeispiel enthält einen Zeitplan und eine Zurücksetzungsoption, mit der der Verlauf des Indexers gelöscht wird. Außerdem wird eine Methode zum sofortigen Erstellen und Ausführen des Indexers aufgerufen.

  ```csharp
  Console.WriteLine("Creating Azure SQL indexer...");
  Indexer indexer = new Indexer(
      name: "azure-sql-indexer",
      dataSourceName: dataSource.Name,
      targetIndexName: index.Name,
      schedule: new IndexingSchedule(TimeSpan.FromDays(1)));
  // Indexers contain metadata about how much they have already indexed
  // If we already ran the sample, the indexer will remember that it already
  // indexed the sample data and not run again
  // To avoid this, reset the indexer if it exists
  exists = await searchService.Indexers.ExistsAsync(indexer.Name);
  if (exists)
  {
      await searchService.Indexers.ResetAsync(indexer.Name);
  }

  await searchService.Indexers.CreateOrUpdateAsync(indexer);

  // We created the indexer with a schedule, but we also
  // want to run it immediately
  Console.WriteLine("Running Azure SQL indexer...");

  try
  {
      await searchService.Indexers.RunAsync(indexer.Name);
  }
  catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
  {
      Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
  }
  ```



## <a name="run-the-indexer"></a>Ausführen des Indexers

In diesem Schritt wird das Programm kompiliert und ausgeführt. 

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **DotNetHowToIndexers**, und klicken Sie auf **Erstellen**.
2. Klicken Sie erneut mit der rechten Maustaste auf **DotNetHowToIndexers**, und klicken Sie anschließend auf **Debuggen** > **Neue Instanz starten**.

Das Programm wird im Debugmodus ausgeführt. Ein Konsolenfenster gibt Aufschluss über den Status der einzelnen Vorgänge.

  ![SQL-Skript](./media/search-indexer-tutorial/console-output.png)

Der Code wird lokal in Visual Studio ausgeführt und stellt eine Verbindung mit Ihrem Suchdienst in Azure her, der wiederum unter Verwendung der Verbindungszeichenfolge eine Verbindung mit Azure SQL-Datenbank herstellt und das Dataset abruft. Durch die Anzahl von Vorgängen gibt es mehrere potenzielle Fehlerquellen. Prüfen Sie im Falle eines Fehlers allerdings zunächst Folgendes:

+ Die Angabe der Suchdienst-Verbindungsinformationen beschränkt sich in diesem Tutorial auf den Dienstnamen. Wenn Sie die vollständige URL eingegeben haben, werden die Vorgänge bei der Indexerstellung mit einem Verbindungsfehler beendet.

+ Datenbank-Verbindungsinformationen in **appsettings.json**: Hierbei muss es sich um die ADO.NET-Verbindungszeichenfolge aus dem Portal handeln, die mit einem gültigen Benutzernamen und Kennwort für die Datenbank versehen wurde. Das Benutzerkonto muss über Datenabrufberechtigungen verfügen.

+ Ressourceneinschränkungen: Bedenken Sie, dass für den Free-Tarif eine Obergrenze von drei Indizes, Indexern und Datenquellen gilt. Ist die Obergrenze für einen Dienst erreicht, können keine neuen Objekte mehr erstellt werden.

## <a name="search-the-index"></a>Durchsuchen des Index 

Klicken Sie im Azure-Portal im oberen Bereich der Übersichtsseite des Suchdiensts auf **Suchexplorer**, um einige Abfragen für den neuen Index zu übermitteln.

1. Klicken Sie im oberen Bereich auf **Index ändern**, um den Index *hotels* auszuwählen.

2. Klicken Sie auf die Schaltfläche **Suchen**, um eine leere Suchabfrage auszuführen. 

   Die drei Einträge in Ihrem Index werden als JSON-Dokumente zurückgegeben. Der Suchexplorer gibt Dokumente im JSON-Format zurück, sodass Sie sich die gesamte Struktur ansehen können.

3. Geben Sie als Nächstes eine Suchzeichenfolge ein: `search=river&$count=true`. 

   Diese Abfrage führt eine Volltextsuche nach dem Begriff `river` durch, und das Ergebnis enthält die Anzahl passender Dokumente. Die Rückgabe der Anzahl passender Dokumente ist hilfreich in Testszenarien mit einem umfangreichen Index mit tausenden oder Millionen von Dokumenten. In diesem Fall entspricht der Abfrage lediglich ein einzelnes Dokument.

4. Geben Sie abschließend eine Suchzeichenfolge ein, die die JSON-Ausgabe auf relevante Felder beschränkt: `search=river&$count=true&$select=hotelId, baseRate, description`. 

   Die Abfrageantwort wird auf die ausgewählten Felder beschränkt, um eine präzisere Ausgabe zu erhalten.

## <a name="view-indexer-configuration"></a>Anzeigen der Indexerkonfiguration

Alle Indexer werden im Portal angezeigt. Das gilt auch für den, den Sie gerade programmgesteuert erstellt haben. Sie können eine Indexerdefinition öffnen und die dazugehörige Datenquelle anzeigen oder einen Aktualisierungszeitplan zur Erfassung neuer und geänderter Zeilen konfigurieren.

1. [Melden Sie sich am Azure-Portal](https://portal.azure.com/) an, und klicken Sie in Ihrem Suchdienst auf der Seite **Übersicht** auf die Links für **Indizes**, **Indexer** und **Datenquellen**.
3. Wählen Sie einzelne Objekte aus, um die Konfigurationseinstellungen anzuzeigen oder zu ändern.

   ![Kachel „Indexer“ und „Datenquellen“](./media/search-indexer-tutorial/tiles-portal.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Die schnellste Möglichkeit zur Bereinigung des Systems nach einem Tutorial besteht im Löschen der Ressourcengruppe, die den Azure Cognitive Search-Dienst enthält. Sie können dann die Ressourcengruppe löschen, um alle darin enthaltenen Daten endgültig zu löschen. Der Name der Ressourcengruppe befindet sich im Portal auf der Seite „Übersicht“ des Azure Cognitive Search-Diensts.

## <a name="next-steps"></a>Nächste Schritte

Sie können KI-Anreicherungsalgorithmen an eine Indexerpipeline anfügen. Als Nächstes fahren Sie mit dem folgenden Tutorial fort.

> [!div class="nextstepaction"]
> [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](search-howto-indexing-azure-blob-storage.md)