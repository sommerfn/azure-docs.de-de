---
title: 'C#-Tutorial: Indizieren mehrerer Datenquellen'
titleSuffix: Azure Cognitive Search
description: Hier erfahren Sie, wie Sie Daten aus mehreren Datenquellen in einen einzelnen Azure Cognitive Search-Index importieren.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 69b18cdd4d0bb8e3d13bbacd5d21764004308786
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73795636"
---
# <a name="c-tutorial-combine-data-from-multiple-data-sources-in-one-azure-cognitive-search-index"></a>C#-Tutorial: Vereinen von Daten aus mehreren Datenquellen in einem einzelnen Azure Cognitive Search-Index

Azure Cognitive Search kann Daten aus mehreren Datenquellen importieren, analysieren und in einem einzelnen kombinierten Suchindex indizieren. Dies ist hilfreich, wenn strukturierte Daten mit weniger strukturierten Daten oder sogar Nur-Text-Daten aus anderen Quellen wie etwa Text-, HTML- oder JSON-Dokumenten aggregiert werden.

In diesem Tutorial erfahren Sie, wie Sie Hoteldaten aus einer Azure Cosmos DB-Datenquelle indizieren und mit Hotelzimmerdetails aus Azure Blob Storage-Dokumenten zusammenführen. Das Ergebnis ist ein kombinierter Hotelsuchindex mit komplexen Datentypen.

In diesem Tutorial werden C#, das .NET SDK für Azure Cognitive Search und das Azure-Portal verwendet, um folgende Aufgaben auszuführen:

> [!div class="checklist"]
> * Hochladen von Beispieldaten und Erstellen von Datenquellen
> * Ermitteln des Dokumentschlüssels
> * Definieren und Erstellen des Index
> * Indizieren von Hoteldaten aus Azure Cosmos DB
> * Zusammenführen von Hotelzimmerdaten aus Blob Storage

## <a name="prerequisites"></a>Voraussetzungen

In diesem Schnellstart werden die folgenden Dienste, Tools und Daten verwendet. 

- [Erstellen Sie einen Azure Cognitive Search-Dienst](search-create-service-portal.md), oder [suchen Sie nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in Ihrem aktuellen Abonnement. In diesem Tutorial können Sie einen kostenlosen Dienst verwenden.

- [Erstellen Sie ein Azure Cosmos DB-Konto](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal) zum Speichern der exemplarischen Hoteldaten.

- [Erstellen Sie ein Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) zum Speichern der exemplarischen JSON-Blobdaten.

- [Installieren Sie Visual Studio](https://visualstudio.microsoft.com/) für die Verwendung als IDE.

### <a name="install-the-project-from-github"></a>Installieren des Projekts von GitHub

1. Öffnen Sie das folgende Beispielrepository auf GitHub: [azure-search-dotnet-samples](https://github.com/Azure-Samples/azure-search-dotnet-samples).
1. Wählen Sie **Clone or download** (Klonen oder herunterladen) aus, und erstellen Sie eine private lokale Kopie des Repositorys.
1. Öffnen Sie Visual Studio, und installieren Sie das Microsoft Azure Cognitive Search-NuGet-Paket, sofern es noch nicht installiert ist. Wählen Sie im Menü **Extras** die Option **NuGet-Paket-Manager** und anschließend **NuGet-Pakete für Projektmappe verwalten...** aus. Suchen Sie auf der Registerkarte **Durchsuchen** nach **Microsoft.Azure.Search** (Version 9.0.1 oder höher), und führen Sie die Installation durch. Klicken Sie sich durch die zusätzlichen Dialogfelder, um die Installation abzuschließen.

    ![Hinzufügen von Azure-Bibliotheken mithilfe von NuGet](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

1. Navigieren Sie in Visual Studio zu Ihrem lokalen Repository, und öffnen Sie die Projektmappendatei **AzureSearchMultipleDataSources.sln**.

## <a name="get-a-key-and-url"></a>Abrufen eines Schlüssels und einer URL

Für die Interaktion mit dem Azure Cognitive Search-Dienst benötigen Sie die Dienst-URL und einen Zugriffsschlüssel. Ein Suchdienst wird mit beidem erstellt. Gehen Sie daher wie folgt vor, um die erforderlichen Informationen zu erhalten, falls Sie Azure Cognitive Search Ihrem Abonnement hinzugefügt haben:

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com/), und rufen Sie auf der Seite **Übersicht** Ihres Suchdiensts die URL ab. Ein Beispiel für einen Endpunkt ist `https://mydemo.search.windows.net`.

1. Rufen Sie unter **Einstellungen** > **Schlüssel** einen Administratorschlüssel ab, um Vollzugriff auf den Dienst zu erhalten. Es gibt zwei austauschbare Administratorschlüssel – diese wurden zum Zweck der Geschäftskontinuität bereitgestellt, falls Sie einen Rollover für einen Schlüssel durchführen müssen. Für Anforderungen zum Hinzufügen, Ändern und Löschen von Objekten können Sie den primären oder den sekundären Schlüssel verwenden.

![Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels](media/search-get-started-postman/get-url-key.png "Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels")

Für alle an Ihren Dienst gesendeten Anforderungen ist ein API-Schlüssel erforderlich. Ein gültiger Schlüssel sorgt bei jeder Anforderung für gegenseitiges Vertrauen zwischen der Anwendung, die die Anforderung sendet, und dem Dienst, der sie verarbeitet.

## <a name="prepare-sample-azure-cosmos-db-data"></a>Vorbereiten der Azure Cosmos DB-Beispieldaten

In diesem Beispiel werden zwei kleine Datensätze verwendet, die sieben fiktive Hotels beschreiben. Ein Datensatz beschreibt die Hotels an sich und wird in eine Azure Cosmos DB-Datenbank geladen. Der andere Datensatz enthält Hotelzimmerdetails und wird in Form von sieben separaten JSON-Dateien bereitgestellt, die in Azure Blob Storage hochgeladen werden.

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com), und navigieren Sie zur Übersichtsseite Ihres Azure Cosmos DB-Kontos.

1. Klicken Sie auf der Menüleiste auf „Container hinzufügen“. Wählen Sie „Neu erstellen“ aus, und geben Sie den Namen **hotel-rooms-db** an. Geben Sie **hotels** als Sammlungsname und **/HotelId** als Partitionsschlüssel ein. Klicken Sie auf **OK**, um die Datenbank und den Container zu erstellen.

   ![Hinzufügen eines Azure Cosmos DB-Containers](media/tutorial-multiple-data-sources/cosmos-add-container.png "Hinzufügen eines Azure Cosmos DB-Containers")

1. Wählen Sie im Cosmos DB-Daten-Explorer in der Datenbank **hotel-rooms-db** unter dem Container **hotels** das Element **items** aus. Klicken Sie auf der Befehlsleiste auf **Element hochladen**.

   ![Hochladen in die Azure Cosmos DB-Sammlung](media/tutorial-multiple-data-sources/cosmos-upload.png "Hochladen in die Cosmos DB-Sammlung")

1. Klicken Sie im Bereich „Hochladen“ auf die Ordnerschaltfläche, und navigieren Sie im Projektordner zur Datei **cosmosdb/HotelsDataSubset_CosmosDb.json**. Klicken Sie auf **OK**, um den Uploadvorgang zu starten.

   ![Auswählen der hochzuladenden Datei](media/tutorial-multiple-data-sources/cosmos-upload2.png "Auswählen der hochzuladenden Datei")

1. Aktualisieren Sie mithilfe der Schaltfläche „Aktualisieren“ die Elementansicht in der Hotelsammlung. Daraufhin sollten sieben neue Datenbankdokumente aufgeführt werden.

## <a name="prepare-sample-blob-data"></a>Vorbereiten der Beispielblobdaten

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com), navigieren Sie zu Ihrem Azure-Speicherkonto, klicken Sie auf **BLOBs** und dann auf **+ Container**.

1. [Erstellen Sie einen Blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) mit dem Namen **hotel-rooms** zum Speichern der exemplarischen JSON-Hotelzimmerdateien. Sie können die öffentliche Zugriffsebene auf beliebige gültige Werte festlegen.

   ![Erstellen eines Blobcontainers](media/tutorial-multiple-data-sources/blob-add-container.png "Erstellen eines Blobcontainers")

1. Nachdem der Container erstellt wurde, öffnen Sie ihn, und wählen Sie auf der Befehlsleiste die Option **Hochladen** aus.

   ![„Hochladen“ auf der Befehlsleiste](media/search-semi-structured-data/upload-command-bar.png "„Hochladen“ auf der Befehlsleiste")

1. Navigieren Sie zu dem Ordner, der die Beispieldateien enthält. Wählen Sie alle Dateien aus, und klicken Sie dann auf **Hochladen**.

   ![Hochladen von Dateien](media/tutorial-multiple-data-sources/blob-upload.png "Hochladen von Dateien")

Nach Abschluss des Uploadvorgangs sollten die Dateien in der Liste für den Datencontainer angezeigt werden.

## <a name="set-up-connections"></a>Einrichten von Verbindungen

Verbindungsinformationen für den Suchdienst und die Datenquellen werden in der Projektmappe in der Datei **appsettings.json** angegeben. 

1. Öffnen Sie in Visual Studio die Datei **AzureSearchMultipleDataSources.sln**.

1. Bearbeiten Sie im Projektmappen-Explorer die Datei **appsettings.json**.  

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "BlobStorageAccountName": "Put your Azure Storage account name here",
  "BlobStorageConnectionString": "Put your Azure Blob Storage connection string here",
  "CosmosDBConnectionString": "Put your Cosmos DB connection string here",
  "CosmosDBDatabaseName": "hotel-rooms-db"
}
```

Für die ersten beiden Einträge werden die URL und die Administratorschlüssel für Ihren Azure Cognitive Search-Dienst verwendet. Heißt der Endpunkt beispielsweise `https://mydemo.search.windows.net`, muss als Dienstname `mydemo` angegeben werden.

Die nächsten Einträge dienen zum Angeben der Kontonamen und Verbindungszeichenfolgeninformationen für die Azure Blob Storage- und die Azure Cosmos DB-Datenquelle.

### <a name="identify-the-document-key"></a>Ermitteln des Dokumentschlüssels

In Azure Cognitive Search werden die einzelnen Dokumente im Index durch das Schlüsselfeld eindeutig identifiziert. Jeder Suchindex muss über genau ein Schlüsselfeld vom Typ `Edm.String`verfügen. Dieses Schlüsselfeld muss für jedes Dokument in einer Datenquelle vorhanden sein, die dem Index hinzugefügt wird. (Es ist gleichzeitig das einzige erforderliche Feld.)

Beim Indizieren von Daten aus mehreren Datenquellen muss jeder Datenquellen-Schlüsselwert dem gleichen Schlüsselfeld im kombinierten Index zugeordnet werden. Häufig ist ein gewisses Maß an Vorausplanung erforderlich, um einen sinnvollen Dokumentschlüssel für Ihren Index zu bestimmen und sicherzustellen, dass er in jeder Datenquelle vorhanden ist.

Azure Cognitive Search-Indexer können Feldzuordnungen verwenden, um im Zuge der Indizierung den Namen und sogar das Format von Datenfeldern zu ändern, damit Quelldaten dem korrekten Indexfeld zugeführt werden können.

In unseren Azure Cosmos DB-Beispieldaten heißt die Hotel-ID beispielsweise **HotelId**. In den JSON-Blobdateien für die Hotelzimmer heißt die Hotel-ID dagegen **Id**. Das Programm behandelt diese Diskrepanz, indem es das Feld **Id** aus den Blobs dem Schlüsselfeld **HotelId** im Index zuordnet.

> [!NOTE]
> Automatisch generierte Dokumentschlüssel (wie sie etwa standardmäßig von einigen Indexern erstellt werden) eignen sich häufig nicht besonders für kombinierte Indizes. Grundsätzlich empfiehlt es sich, einen aussagekräftigen, eindeutigen Schlüsselwert zu verwenden, der bereits in Ihren Datenquellen vorhanden ist oder diesen problemlos hinzugefügt werden kann.

## <a name="understand-the-code"></a>Grundlegendes zum Code

Sobald die Daten und Konfigurationseinstellungen vorhanden sind, kann das Beispielprogramm in **AzureSearchMultipleDataSources.sln** erstellt und ausgeführt werden.

Diese einfache C#/.NET-Konsolen-App führt folgende Aufgaben aus:
* Sie erstellt auf der Grundlage der Struktur der C#-Klasse „Hotel“ (die auch auf die Klassen „Address“ und „Room“ verweist) einen neuen Azure Cognitive Search-Index.
* Sie erstellt eine Azure Cosmos DB-Datenquelle und einen Indexer, der Azure Cosmos DB-Daten Indexfeldern zuordnet.
* Sie führt den Azure Cosmos DB-Indexer aus, Hoteldaten zu laden.
* Sie erstellt eine Azure Blob Storage-Datenquelle und einen Indexer, der JSON-Blobdaten Indexfeldern zuordnet.
* Sie führt den Azure Blob Storage-Indexer aus, um Zimmerdaten zu laden.

 Nehmen Sie sich vor dem Ausführen des Programms etwas Zeit, um sich den Code sowie die Index- und Indexerdefinitionen für dieses Beispiel etwas genauer anzusehen. Der relevante Code befindet sich in zwei Dateien:

  + **Hotel.cs** enthält das Schema, das den Index definiert.
  + **Program.cs** enthält Funktionen zum Erstellen des Azure Cognitive Search-Index, der Datenquellen und der Indexer sowie zum Laden der kombinierten Ergebnisse in den Index.

### <a name="define-the-index"></a>Definieren des Index

In diesem Beispielprogramm wird das .NET SDK verwendet, um einen Azure Cognitive Search-Index zu definieren und zu erstellen. Das SDK nutzt die Klasse [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder), um eine Indexstruktur auf der Grundlage einer C#-Datenmodellklasse zu generieren.

Das Datenmodell wird durch die Klasse „Hotel“ definiert, die auch Verweise auf die Klassen „Address“ und „Room“ enthält. „FieldBuilder“ führt ein Drilldown durch die verschiedenen Klassendefinitionen aus, um eine komplexe Datenstruktur für den Index zu generieren. Mithilfe von Metadatentags werden die Attribute der einzelnen Felder definiert, um beispielsweise anzugeben, ob das Feld durchsuchbar oder sortierbar ist.

Die folgenden Codeausschnitte aus der Datei **Hotel.cs** zeigen, wie ein einzelnes Feld und ein Verweis auf eine andere Datenmodellklasse angegeben werden können.

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
public Room[] Rooms { get; set; }
. . .
```

In der Datei **Program.cs** wird der Index mit einem Namen und einer Feldsammlung definiert, die durch die Methode `FieldBuilder.BuildForType<Hotel>()` generiert wird, und anschließend erstellt:

```csharp
private static async Task CreateIndex(string indexName, SearchServiceClient searchService)
{
    // Create a new search index structure that matches the properties of the Hotel class.
    // The Address and Room classes are referenced from the Hotel class. The FieldBuilder
    // will enumerate these to create a complex data structure for the index.
    var definition = new Index()
    {
        Name = indexName,
        Fields = FieldBuilder.BuildForType<Hotel>()
    };
    await searchService.Indexes.CreateAsync(definition);
}
```

### <a name="create-azure-cosmos-db-data-source-and-indexer"></a>Erstellen der Azure Cosmos DB-Datenquelle und des zugehörigen Indexers

Das Hauptprogramm enthält auch Logik zum Erstellen der Azure Cosmos DB-Datenquelle für die Hoteldaten.

Dazu wird zunächst der Name der Azure Cosmos DB-Datenbank mit der Verbindungszeichenfolge verkettet. Anschließend wird das Datenquellenobjekt definiert – einschließlich spezifischer Einstellungen für Azure Cosmos DB-Quellen (wie etwa die Eigenschaft „useChangeDetection“).

  ```csharp
private static async Task CreateAndRunCosmosDbIndexer(string indexName, SearchServiceClient searchService)
{
    // Append the database name to the connection string
    string cosmosConnectString = 
        configuration["CosmosDBConnectionString"]
        + ";Database=" 
        + configuration["CosmosDBDatabaseName"];

    DataSource cosmosDbDataSource = DataSource.CosmosDb(
        name: configuration["CosmosDBDatabaseName"], 
        cosmosDbConnectionString: cosmosConnectString,
        collectionName: "hotels",
        useChangeDetection: true);

    // The Azure Cosmos DB data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(cosmosDbDataSource);
  ```

Nach Erstellung der Datenquelle richtet das Programm einen Azure Cosmos DB-Indexer namens **hotel-rooms-cosmos-indexer** ein.

```csharp
    Indexer cosmosDbIndexer = new Indexer(
        name: "hotel-rooms-cosmos-indexer",
        dataSourceName: cosmosDbDataSource.Name,
        targetIndexName: indexName,
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));
    
    // Indexers keep metadata about how much they have already indexed.
    // If we already ran this sample, the indexer will remember that it already
    // indexed the sample data and not run again.
    // To avoid this, reset the indexer if it exists.
    bool exists = await searchService.Indexers.ExistsAsync(cosmosDbIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(cosmosDbIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(cosmosDbIndexer);
```
Bereits vorhandene Indexer mit dem gleichen Namen werden vor der Erstellung des neuen Indexers gelöscht (für den Fall, dass Sie dieses Beispiel mehrmals ausführen möchten).

In diesem Beispiel wird ein Zeitplan für den Indexer definiert, sodass er einmal täglich ausgeführt wird. Sie können die Zeitplaneigenschaft aus diesem Aufruf entfernen, wenn der Indexer in Zukunft nicht mehr automatisch ausgeführt werden soll.

### <a name="index-azure-cosmos-db-data"></a>Indizieren der Azure Cosmos DB-Daten

Nach Erstellung der Datenquelle und des zugehörigen Indexers ist der Code zum Ausführen des Indexers nicht sonderlich lang:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Dieses Beispiel enthält einen einfachen try-catch-Block, um Fehler zu melden, die ggf. bei der Ausführung auftreten.

Nach Ausführung des Azure Cosmos DB-Indexers enthält der Suchindex einen vollständigen Satz exemplarischer Hoteldokumente. Die Zimmerfelder für die einzelnen Hotels sind jedoch ein leeres Array, da die Azure Cosmos DB-Datenquelle keine Zimmerdetails enthält. Als Nächstes führt das Programm einen Pullvorgang für Blob Storage aus, um die Zimmerdaten zu laden und zusammenzuführen.

### <a name="create-blob-storage-data-source-and-indexer"></a>Erstellen der Blob Storage-Datenquelle und des zugehörigen Indexers

Zum Abrufen der Zimmerdetails richtet das Programm zunächst eine Blob Storage-Datenquelle ein, um auf eine Gruppe individueller JSON-Blobdateien zu verweisen.

```csharp
private static async Task CreateAndRunBlobIndexer(string indexName, SearchServiceClient searchService)
{
    DataSource blobDataSource = DataSource.AzureBlobStorage(
        name: configuration["BlobStorageAccountName"],
        storageConnectionString: configuration["BlobStorageConnectionString"],
        containerName: "hotel-rooms");

    // The blob data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(blobDataSource);
```

Nach Erstellung der Datenquelle richtet das Programm einen Blobindexer namens **hotel-rooms-blob-indexer** ein.

```csharp
    // Add a field mapping to match the Id field in the documents to 
    // the HotelId key field in the index
    List<FieldMapping> map = new List<FieldMapping> {
        new FieldMapping("Id", "HotelId")
    };

    Indexer blobIndexer = new Indexer(
        name: "hotel-rooms-blob-indexer",
        dataSourceName: blobDataSource.Name,
        targetIndexName: indexName,
        fieldMappings: map,
        parameters: new IndexingParameters().ParseJson(),
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));

    // Reset the indexer if it already exists
    bool exists = await searchService.Indexers.ExistsAsync(blobIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(blobIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(blobIndexer);
```

Die JSON-Blobs enthalten ein Schlüsselfeld namens **Id** (anstelle von **HotelId**). Der Code weist den Indexer mithilfe der Klasse `FieldMapping` an, den Wert des Felds **Id** an den Dokumentschlüssel **HotelId** im Index weiterzuleiten.

Blob Storage-Indexer können Parameter nutzen, die den zu verwendenden Analysemodus angeben. Der Analysemodus unterscheidet sich abhängig davon, ob Blobs ein einzelnes Dokument oder mehrere Dokumente innerhalb des gleichen Blobs darstellen. In diesem Beispiel stellt jedes Blob ein einzelnes Indexdokument dar, weshalb im Code der Parameter `IndexingParameters.ParseJson()` verwendet wird.

Weitere Informationen zu Indexer-Analyseparametern für JSON-Blobs finden Sie unter [Indizieren von JSON-Blobs mit dem Azure Search-Blobindexer](search-howto-index-json-blobs.md). Weitere Informationen zum Angeben dieser Parameter unter Verwendung des .NET SDK finden Sie unter [IndexingParametersExtensions Class](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingparametersextensions) (Klasse „IndexingParametersExtensions“).

Bereits vorhandene Indexer mit dem gleichen Namen werden vor der Erstellung des neuen Indexers gelöscht (für den Fall, dass Sie dieses Beispiel mehrmals ausführen möchten).

In diesem Beispiel wird ein Zeitplan für den Indexer definiert, sodass er einmal täglich ausgeführt wird. Sie können die Zeitplaneigenschaft aus diesem Aufruf entfernen, wenn der Indexer in Zukunft nicht mehr automatisch ausgeführt werden soll.

### <a name="index-blob-data"></a>Indizieren der Blobdaten

Nach Erstellung der Blob Storage-Datenquelle und des zugehörigen Indexers ist der Code zum Ausführen des Indexers nicht sonderlich komplex:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Da der Index bereits mit Hoteldaten aus der Azure Cosmos DB-Datenbank aufgefüllt wurde, aktualisiert der Blobindexer die im Index vorhandenen Dokumente und fügt die Zimmerdetails hinzu.

> [!NOTE]
> Wenn beide Datenquellen die gleichen schlüsselfremden Felder enthalten und die Daten in diesen Feldern nicht übereinstimmen, enthält der Index die Werte des zuletzt ausgeführten Indexers. In unserem Beispiel enthalten beide Datenquellen ein Feld namens **HotelName**. Sollten sich die Daten in diesem Feld bei Dokumenten mit dem gleichen Schlüsselwert aus irgendeinem Grund unterscheiden, werden im Index die Daten vom Typ **HotelName** aus der zuletzt indizierten Datenquelle gespeichert.

## <a name="search-your-json-files"></a>Durchsuchen von JSON-Dateien

Nach Ausführung des Programms können Sie den aufgefüllten Suchindex über den [**Suchexplorer**](search-explorer.md) des Portals erkunden.

Öffnen Sie im Azure-Portal die Seite **Übersicht** für den Suchdienst, und suchen Sie in der Liste **Indexes** nach dem Index **hotel-rooms-sample**.

  ![Liste mit den Azure Cognitive Search-Indizes](media/tutorial-multiple-data-sources/index-list.png "Liste mit den Azure Cognitive Search-Indizes")

Klicken Sie in der Liste auf den Index „hotel-rooms-sample“. Daraufhin wird eine Suchexploreroberfläche für den Index angezeigt. Geben Sie eine Abfrage für einen Begriff wie „Luxury“ ein. Daraufhin sollte in den Ergebnissen mindestens ein Dokument angezeigt werden, und dieses Dokument sollte eine Liste mit Zimmerobjekten aus dem zugehörigen Zimmerarray enthalten.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Die schnellste Möglichkeit zur Bereinigung des Systems nach einem Tutorial besteht im Löschen der Ressourcengruppe, die den Azure Cognitive Search-Dienst enthält. Sie können dann die Ressourcengruppe löschen, um alle darin enthaltenen Daten endgültig zu löschen. Der Name der Ressourcengruppe befindet sich im Portal auf der Seite „Übersicht“ des Azure Cognitive Search-Diensts.

## <a name="next-steps"></a>Nächste Schritte

Es gibt verschiedene Ansätze und mehrere Optionen für die Indizierung von JSON-Blobs. Falls Ihre Quelldaten JSON-Inhalte enthalten, können Sie sich diese Optionen näher ansehen, um die optimale Lösung für Ihr Szenario zu ermitteln.

> [!div class="nextstepaction"]
> [Indizieren von JSON-Blobs mit dem Azure Cognitive Search-Blobindexer](search-howto-index-json-blobs.md)

Es empfiehlt sich bisweilen, strukturierte Indexdaten aus einer Datenquelle mit kognitiv angereicherten Daten aus unstrukturierten Blobs oder Volltextinhalten zu erweitern. Im folgenden Tutorial erfahren Sie, wie Sie eine Kombination aus Cognitive Services, Azure Cognitive Search und dem .NET SDK verwenden:

> [!div class="nextstepaction"]
> [Aufrufen von Cognitive Services-APIs in einer Azure Cognitive Search-Indizierungspipeline](cognitive-search-tutorial-blob-dotnet.md)
