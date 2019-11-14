---
title: Indizieren einer Azure Cosmos DB-Datenquelle
titleSuffix: Azure Cognitive Search
description: Durchforsten einer Azure Cosmos DB-Datenquelle und Erfassen von Daten in einem durchsuchbaren Volltextindex in der kognitiven Azure-Suche. Indexer automatisieren die Datenerfassung für ausgewählte Datenquellen wie Azure Cosmos DB.
author: mgottein
manager: nitinme
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 41da5b59c7d9429a068ecd483aa96edb1141b727
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73719948"
---
# <a name="how-to-index-cosmos-db-data-using-an-indexer-in-azure-cognitive-search"></a>Indizieren von Cosmos DB-Daten mithilfe eines Indexers in der kognitiven Azure-Suche 

> [!IMPORTANT] 
> Die SQL-API ist allgemein verfügbar.
> Die Unterstützung von MongoDB-API, Gremlin-API und Cassandra-API befindet sich aktuell in der Public Preview-Phase. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Füllen Sie [dieses Formular](https://aka.ms/azure-cognitive-search/indexer-preview) aus, wenn Sie Zugriff auf die Vorschauversionen erhalten möchten. Previewfunktionen werden von der [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Die Portalunterstützung ist momentan eingeschränkt, und das .NET SDK wird nicht unterstützt.

In diesem Artikel erfahren Sie, wie Sie einen Azure Cosmos DB-[Indexer](search-indexer-overview.md) zum Extrahieren von Inhalten konfigurieren und dafür sorgen, dass er in der kognitiven Azure-Suche durchsucht werden kann. Mit diesem Workflow erstellen Sie einen Index der kognitiven Azure-Suche und laden ihn mit vorhandenem aus Azure Cosmos DB extrahiertem Text. 

Da die Begrifflichkeiten etwas verwirrend sind, soll hier darauf hingewiesen werden, dass die [Indizierung in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/index-overview) und die [Indizierung in der kognitiven Azure-Suche](search-what-is-an-index.md) zwei verschiedene Vorgänge sind, die jedem Dienst eigen sind. Wenn Sie mit der Indizierung in der kognitiven Azure-Suche beginnen, muss Ihre Azure Cosmos DB-Datenbank bereits vorhanden sein und Daten enthalten.

Mit dem Cosmos DB-Indexer in Azure Cognitive Search können Sie [Azure Cosmos DB-Elemente](https://docs.microsoft.com/azure/cosmos-db/databases-containers-items#azure-cosmos-items) durchforsten, auf die über verschiedene Protokolle zugegriffen wird.

+ Für die allgemein verfügbare [SQL-API](https://docs.microsoft.com/azure/cosmos-db/sql-api-query-reference) können Sie das [Portal](#cosmos-indexer-portal), die [REST-API](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) oder das [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet) verwenden.

+ Für die [MongoDB-API (Vorschauversion)](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction) und die [Gremlin-API (Vorschauversion)](https://docs.microsoft.com/azure/cosmos-db/graph-introduction) können Sie entweder das [Portal](#cosmos-indexer-portal) oder die [REST-API-Version 2019-05-06-Preview](search-api-preview.md) in einem Aufruf vom Typ [Create Indexer (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer) verwenden, um den Indexer zu erstellen.

+ Für die [Cassandra-API (Vorschauversion)](https://docs.microsoft.com/azure/cosmos-db/cassandra-introduction) kann nur die [REST-API-Version 2019-05-06-Preview](search-api-preview.md) in einem Aufruf vom Typ [Create Indexer (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer) verwendet werden.


> [!Note]
> Bei User Voice können Sie für die [Tabellen-API](https://feedback.azure.com/forums/263029-azure-search/suggestions/32759746-azure-search-should-be-able-to-index-cosmos-db-tab) abstimmen, wenn Sie möchten, dass sie in Azure Cognitive Search unterstützt wird.
>

<a name="cosmos-indexer-portal"></a>

## <a name="use-the-portal"></a>Verwenden des Portals

> [!Note]
> Das Portal unterstützt derzeit die SQL-API und die MongoDB-API (Vorschauversion).

Die einfachste Methode zum Indizieren von Azure Cosmos DB-Elementen ist die Verwendung eines Assistenten im [Azure-Portal](https://portal.azure.com/). Der [**Datenimport**](search-import-data-portal.md)-Assistent in der kognitiven Azure-Suche kann durch die Entnahme von Stichproben und das Lesen von Metadaten im Container einen Standardindex erstellen, Quellfelder Zielindexfeldern zuordnen und den Index in einem einzigen Vorgang laden. Je nach Größe und Komplexität der Quelldaten können Sie auch innerhalb von Minuten einen funktionsfähigen Volltextsuchindex erstellen.

Sie sollten für die kognitive Azure-Suche und Azure Cosmos DB dasselbe Azure-Abonnement verwenden, vorzugsweise in derselben Region.

### <a name="1---prepare-source-data"></a>1\. Vorbereiten von Quelldaten

Sie benötigen ein Cosmos DB-Konto, eine Azure Cosmos DB-Datenbank, die der SQL-API, der MongoDB-API (Vorschauversion) oder der Gremlin-API (Vorschauversion) zugeordnet ist, sowie Inhalte in der Datenbank.

Stellen Sie sicher, dass Ihre Cosmos DB-Datenbank Daten enthält. Der [Datenimport-Assistent](search-import-data-portal.md) liest Metadaten und entnimmt Stichproben, um ein Indexschema abzuleiten. Darüber hinaus lädt er auch Daten aus Cosmos DB. Wenn die Daten nicht vorhanden sind, wird der Assistent mit der folgenden Fehlermeldung beendet: „Fehler bei der Ermittlung des Indexschemas der Datenquelle: Could not build a prototype index because datasource 'emptycollection' returned no data“ (Prototyp-Index konnte nicht erstellt werden, da von der Datenquelle „emptycollection“ keine Daten zurückgegeben wurden).

### <a name="2---start-import-data-wizard"></a>2\. Starten des Datenimport-Assistenten

Sie können den Assistenten auf der Seite des Azure Cognitive Search-Diensts über die Befehlsleiste starten. (Weitere Informationen finden Sie [hier](search-import-data-portal.md).) Falls Sie eine Verbindung mit der Cosmos DB-SQL-API herstellen, können Sie alternativ im Abschnitt **Einstellungen** (im linken Navigationsbereich Ihres Cosmos DB-Kontos) auf **Azure Cognitive Search hinzufügen** klicken.

   ![Befehl „Daten importieren“ im Portal](./media/search-import-data-portal/import-data-cmd2.png "Starten des Datenimport-Assistenten")

### <a name="3---set-the-data-source"></a>3\. Einrichten der Datenquelle

Auf der Seite **Datenquelle** muss die Quelle **Cosmos DB** lauten und folgende Angaben enthalten:

+ Der **Name** ist der Name des Datenquellenobjekts. Nachdem Sie es erstellt haben, können Sie es für andere Workloads verwenden.

+ Das **Cosmos DB-Konto** muss die erste oder zweite Verbindungszeichenfolge aus Cosmos DB mit einem `AccountEndpoint` und einem `AccountKey` sein. Fügen Sie für MongoDB-Sammlungen am Ende der Verbindungszeichenfolge die Angabe **ApiKind=MongoDb** hinzu, und trennen Sie sie mit einem Semikolon von der Verbindungszeichenfolge. Halten Sie sich bei der Gremlin-API und der Cassandra-API an die Anweisungen für die [REST-API](#cosmosdb-indexer-rest).

+ Die **Datenbank** ist eine im Konto vorhandene Datenbank. 

+ Eine **Sammlung** ist ein Container mit Dokumenten. Dokumente müssen vorhanden sein, damit ein Import durchgeführt werden kann. 

+ Die **Abfrage** kann leer sein, wenn alle Dokumente gewünscht werden. Andernfalls können Sie eine Abfrage eingeben, mit der eine Teilmenge von Dokumenten ausgewählt wird. **Abfrage** ist nur für die SQL-API verfügbar.

   ![Cosmos DB-Datenquellendefinition](media/search-howto-index-cosmosdb/cosmosdb-datasource.png "Cosmos DB-Datenquellendefinition")

### <a name="4---skip-the-add-cognitive-search-page-in-the-wizard"></a>4\. Überspringen der Seite „Kognitive Suche hinzufügen“ im Assistenten

Das Hinzufügen kognitiver Qualifikationen ist für den Import eines Dokuments nicht erforderlich. Wenn Sie der Indizierungspipeline keine [KI-Anreicherung hinzufügen](cognitive-search-concept-intro.md) möchten, können Sie diesen Schritt überspringen.

Um den Schritt zu überspringen, wechseln Sie zunächst zur nächsten Seite.

   ![Schaltfläche „Nächste Seite“ zum Hinzufügen von Skills](media/search-get-started-portal/next-button-add-cog-search.png)

Von dieser Seite aus können Sie zur Indexanpassung springen.

   ![Überspringen des Schritts zu kognitiven Qualifikationen](media/search-get-started-portal/skip-cog-skill-step.png)

### <a name="5---set-index-attributes"></a>5\. Festlegen von Indexattributen

Auf der **Indexseite** sollte eine Liste von Feldern mit einem Datentyp sowie mehrere Kontrollkästchen zum Festlegen von Indexattributen aufgeführt sein. Der Assistent kann basierend auf Metadaten und durch Sampling der Quelldaten eine Felderliste erstellen. 

Sie können durch Klicken auf das Kontrollkästchen am Kopf einer Attributspalte mehrere Attribute gleichzeitig auswählen. Wählen Sie **Abrufbar** und **Durchsuchbar** für jedes Feld aus, das an eine Client-App zurückgegeben werden sollte und der Volltextsuche unterliegt. Sie werden feststellen, dass Ganzzahlen nicht mit der Volltext- oder Fuzzysuche durchsuchbar sind (Zahlen werden wörtlich ausgewertet und eignen sich häufig in Filtern).

Weitere Informationen finden Sie in den Beschreibungen der [Indexattribute](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) und [Sprachanalysetools](https://docs.microsoft.com/rest/api/searchservice/language-support). 

Nehmen Sie sich einen Moment Zeit, um Ihre Auswahl zu überprüfen. Wenn Sie den Assistenten ausführen, werden physische Datenstrukturen erstellt, und Sie können diese Felder nicht bearbeiten, ohne alle Objekte zu löschen und neu zu erstellen.

   ![Cosmos DB-Indexdefinition](media/search-howto-index-cosmosdb/cosmosdb-index-schema.png "Cosmos DB-Indexdefinition")

### <a name="6---create-indexer"></a>6: Erstellen des Indexers

Wenn alle Angaben gemacht wurden, erstellt der Assistent drei unterschiedliche Objekte in Ihrem Suchdienst. Ein Datenquellenobjekt und ein Indexobjekt werden als benannte Ressourcen in Ihrem Dienst der kognitiven Azure-Suche gespeichert. Mit dem letzten Schritt wird ein Indexerobjekt erstellt. Wenn der Indexer benannt wird, kann er als eigenständige Ressource existieren. Diese können Sie unabhängig vom Index und Datenquellenobjekt, die im selben Durchlauf des Assistenten erstellt wurden, planen und verwalten.

Falls Sie noch nicht mit Indexern vertraut sind: Ein *Indexer* ist eine Ressource in der kognitiven Azure-Suche, die eine externe Datenquelle nach durchsuchbarem Inhalt durchforstet. Die Ausgabe des **Datenimport**-Assistenten ist ein Indexer, der die Cosmos DB-Datenquelle durchforstet, durchsuchbaren Inhalt extrahiert und diesen in einen Index in der kognitiven Azure-Suche importiert.

Der folgende Screenshot zeigt die Konfiguration des Standardindexers. Sie können zu **Einmal** wechseln, wenn Sie den Indexer einmal ausführen möchten. Klicken Sie auf **Senden**, um den Assistenten auszuführen und alle Objekte zu erstellen. Die Indizierung wird sofort durchgeführt.

   ![Cosmos DB-Indexerdefinition](media/search-howto-index-cosmosdb/cosmosdb-indexer.png "Cosmos DB-Indexerdefinition")

Sie können den Datenimport auf den Portalseiten überwachen. Fortschrittsbenachrichtigungen geben den Indizierungsstatus an und wie viele Dokumente hochgeladen werden. 

Wenn die Indizierung abgeschlossen ist, können Sie den [Such-Explorer](search-explorer.md) zum Abfragen Ihres Indexes verwenden.

> [!NOTE]
> Wenn nicht die Daten angezeigt werden, die Sie erwarten, müssen Sie vielleicht weitere Attribute für weitere Felder festlegen. Löschen Sie den Index und Indexer, die Sie gerade erstellt haben, und führen Sie die Schritte des Assistenten erneut aus, wobei Sie Ihre Auswahl für Indexattribute in Schritt 5 ändern. 

<a name="cosmosdb-indexer-rest"></a>

## <a name="use-rest-apis"></a>Verwenden von REST-APIs

Sie können die REST-API für die Indizierung von Azure Cosmos DB-Daten in einem dreiteiligen Workflow verwenden, der für alle Indexer in der kognitiven Azure-Suche gleich ist: Erstellen einer Datenquelle, eines Index und eines Indexers. Die Datenextraktion aus Cosmos DB erfolgt, wenn Sie die Anforderung für die Indexererstellung übermitteln. Nachdem diese Anforderung abgeschlossen ist, verfügen Sie über einen Index, der abgefragt werden kann. 

> [!NOTE]
> Wenn Sie Daten per Cosmos DB-Gremlin-API oder Cosmos DB-Cassandra-API indizieren möchten, müssen Sie zunächst [dieses Formular](https://aka.ms/azure-cognitive-search/indexer-preview) ausfüllen, um Zugriff auf die geschlossene Vorschau anzufordern. Nachdem Ihre Anforderung bearbeitet wurde, erhalten Sie Anweisungen für die Erstellung der Datenquelle unter Verwendung der [REST-API-Version 2019-05-06-Preview](search-api-preview.md).

Wie bereits weiter oben in diesem Artikel erwähnt, sind [Azure Cosmos DB-Indizierung](https://docs.microsoft.com/azure/cosmos-db/index-overview) und [Azure Cognitive Search-Indizierung](search-what-is-an-index.md) zwei unterschiedliche Vorgänge. Bei der Cosmos DB-Indizierung werden standardmäßig alle Dokumente automatisch indiziert (außer bei Verwendung der Cassandra-API). Wenn Sie die automatische Indizierung deaktivieren, kann auf die Dokumente nur über ihre Self-Links oder über Abfragen mit der entsprechenden Dokument-ID zugegriffen werden. Bei der Azure Cognitive Search-Indizierung muss die automatische Cosmos DB-Indizierung für die Sammlung aktiviert sein, die von Azure Cognitive Search indiziert wird. Wenn Sie sich für die Vorschauversion des Indexers der Cosmos DB-Cassandra-API registrieren, erhalten Sie eine Einrichtungsanleitung für die Cosmos DB-Indizierung.

> [!WARNING]
> Azure Cosmos DB ist die nächste Generation von DocumentDB. Zuvor konnten Sie mit der API-Version **2017-11-11** die `documentdb`-Syntax verwenden. Das bedeutete, dass Sie den Datenquellentyp als `cosmosdb` oder `documentdb` angeben konnten. Ab API-Version **2019-05-06** wird mit den APIs der kognitiven Azure-Suche sowie im Portal nur die `cosmosdb`-Syntax wie in diesem Artikel beschrieben unterstützt. Das bedeutet, dass der Datenquellentyp `cosmosdb` sein muss, wenn Sie eine Verbindung mit einem Cosmos DB-Endpunkt herstellen möchten.

### <a name="1---assemble-inputs-for-the-request"></a>1 – Zusammenstellen der Eingaben für die Anforderung

Für jede Anforderung müssen Sie den Dienstnamen und den Administratorschlüssel für die kognitive Azure-Suche (im POST-Header) sowie den Namen des Speicherkontos und den Schlüssel für Blob Storage angeben. Sie können [Postman](search-get-started-postman.md) zum Senden von HTTP-Anforderungen an die kognitive Azure-Suche verwenden.

Kopieren Sie die folgenden vier Werten in den Editor, sodass Sie sie in eine Anforderung einfügen können:

+ Dienstname der kognitiven Azure-Suche
+ Administratorschlüssel der kognitiven Azure-Suche
+ Cosmos DB-Verbindungszeichenfolge

Sie finden diese Werte im Portal:

1. Kopieren Sie in den Portalseiten für die kognitive Azure-Suche die Suchdienst-URL von der Seite „Übersicht“.

2. Klicken Sie im linken Navigationsbereich auf **Schlüssel**, und kopieren Sie dann entweder den primären oder sekundären Schlüssel (sie sind identisch).

3. Wechseln Sie zu den Portalseiten für Ihr Cosmos-Speicherkonto. Klicken Sie im linken Navigationsbereich unter **Einstellungen** auf die Option **Schlüssel**. Diese Seite enthält einen URI, zwei Sätze Verbindungszeichenfolgen und zwei Sätze Schlüssel. Kopieren Sie eine der Verbindungszeichenfolgen in den Editor.

### <a name="2---create-a-data-source"></a>2 - Erstellen einer Datenquelle

Eine **Datenquelle** gibt die zu indizierenden Daten, Anmeldeinformationen und Richtlinien für das Bestimmen von Änderungen in den Daten an (z.B. geänderte oder gelöschte Dokumente in Ihrer Sammlung). Die Datenquelle wird als unabhängige Ressource definiert, sodass sie von mehreren Indexern verwendet werden kann.

Formulieren Sie zum Erstellen einer Datenquelle eine POST-Anforderung:

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mycosmosdbdatasource",
        "type": "cosmosdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myCosmosDbEndpoint.documents.azure.com;AccountKey=myCosmosDbAuthKey;Database=myCosmosDbDatabaseId"
        },
        "container": { "name": "myCollection", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

Der Anforderungstext umfasst die Datenquellendefinition, welche die folgenden Felder enthalten sollte:

| Feld   | BESCHREIBUNG |
|---------|-------------|
| **name** | Erforderlich. Wählen Sie für Ihr Datenquellenobjekt einen beliebigen Namen aus. |
|**type**| Erforderlich. Muss `cosmosdb`lauten. |
|**credentials** | Erforderlich. Muss eine Cosmos DB-Verbindungszeichenfolge sein.<br/>Bei SQL-Sammlungen weisen Verbindungszeichenfolgen folgendes Format auf: `AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`<br/><br/>Bei MongoDB-Sammlungen fügen Sie der Verbindungszeichenfolge **ApiKind=MongoDb** hinzu:<br/>`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>;ApiKind=MongoDb`<br/><br/>Registrieren Sie sich bei Verwendung von Gremlin-Graphen und Cassandra-Tabellen für die [geschlossene Indexervorschau](https://aka.ms/azure-cognitive-search/indexer-preview), um Zugriff auf die Vorschauversion sowie Informationen zur Formatierung der Anmeldeinformationen zu erhalten.<br/><br/>Vermeiden Sie Portnummern in der Endpunkt-URL. Wenn Sie die Portnummer einfügen, kann die Azure Cosmos DB-Datenbank in der kognitiven Azure-Suche nicht indiziert werden.|
| **container** | Enthält die folgenden Elemente: <br/>**name:** Erforderlich. Geben Sie die ID der zu indizierenden Datenbanksammlung an.<br/>**Abfrage**: Optional. Sie können eine Abfrage angeben, um ein beliebiges JSON-Dokument in einem Flatfile-Schema zu vereinfachen, das in der kognitiven Azure-Suche indiziert werden kann.<br/>Für die MongoDB-API, die Gremlin-API und die Cassandra-API werden keine Abfragen unterstützt. |
| **dataChangeDetectionPolicy** | Empfohlen. Weitere Informationen finden Sie im Abschnitt [Indizieren von geänderten Dokumenten](#DataChangeDetectionPolicy).|
|**dataDeletionDetectionPolicy** | Optional. Weitere Informationen finden Sie im Abschnitt [Indizieren von gelöschten Dokumenten](#DataDeletionDetectionPolicy).|

### <a name="using-queries-to-shape-indexed-data"></a>Verwenden von Abfragen zum Formen indizierter Daten
Sie können eine SQL-Abfrage angeben, um geschachtelte Eigenschaften oder Arrays zu vereinfachen, JSON-Eigenschaften zu projizieren und die zu indizierenden Daten zu filtern. 

> [!WARNING]
> Für die **MongoDB-API**, die **Gremlin-API** und die **Cassandra-API** werden keine benutzerdefinierten Abfragen unterstützt: Der Parameter `container.query` muss auf NULL festgelegt oder weggelassen werden. Wenn Sie eine benutzerdefinierte Abfrage verwenden möchten, informieren Sie uns auf [User Voice](https://feedback.azure.com/forums/263029-azure-search).

Beispieldokument:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "cosmosdb", "search"]
    }

Abfrage zur Filterung:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark ORDER BY c._ts

Vereinfachen der Abfrage:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Abfrage zur Projektion:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


Abfrage zum Vereinfachen eines Arrays:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts


### <a name="3---create-a-target-search-index"></a>3 - Erstellen eines Zielsuchindex 

[Erstellen Sie einen Zielindex der kognitiven Azure-Suche ](/rest/api/searchservice/create-index), wenn Sie bislang noch über keinen verfügen. Anhand des folgenden Beispiels wird ein Index mit einer ID und einem Beschreibungsfeld erstellt:

    POST https://[service name].search.windows.net/indexes?api-version=2019-05-06
    Content-Type: application/json
    api-key: [Search service admin key]

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

Stellen Sie sicher, dass das Schema des Ziel-Indexes mit dem Schema der JSON-Quelldokumente oder mit der Ausgabe Ihrer benutzerdefinierten Abfrageprojektion kompatibel ist.

> [!NOTE]
> Bei partitionierten Sammlungen ist der Standarddokumentschlüssel die Eigenschaft `_rid` von Azure Cosmos DB. Diese wird von Azure Cognitive Search automatisch in `rid` umbenannt, da Feldnamen nicht mit einem Unterstrich beginnen dürfen. Darüber hinaus enthalten die `_rid`-Werte von Azure Cosmos DB Zeichen, die in Schlüsseln der kognitiven Azure-Suche ungültig sind. Deshalb sind die `_rid`-Werte Base64-codiert.
> 
> Für MongoDB-Sammlungen wird die `_id`-Eigenschaft in der kognitiven Azure-Suche automatisch in `doc_id` umbenannt.  

### <a name="mapping-between-json-data-types-and-azure-cognitive-search-data-types"></a>Zuordnung zwischen JSON-Datentypen und Datentypen der kognitiven Azure-Suche
| JSON-Datentyp | Kompatible Feldtypen im Zielindex |
| --- | --- |
| Bool |Edm.Boolean, Edm.String |
| Zahlen, die wie Ganzzahlen aussehen |Edm.Int32, Edm.Int64, Edm.String |
| Zahlen, die wie Gleitkommas aussehen |Edm.Double, Edm.String |
| Zeichenfolge |Edm.String |
| Arrays primitiver Typen, z.B. ["a", "b", "c"] |Collection(Edm.String) |
| Zeichenfolgen, die wie Datumsangaben aussehen |Edm.DateTimeOffset, Edm.String |
| GeoJSON-Objekte, z.B. { "type": "Point", "coordinates": [long, lat] } |Edm.GeographyPoint |
| Andere JSON-Objekte |– |

### <a name="4---configure-and-run-the-indexer"></a>4 - Konfigurieren und Ausführen des Indexers

Nach der Erstellung von Index und Datenquelle können Sie den Indexer erstellen:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mycosmosdbindexer",
      "dataSourceName" : "mycosmosdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Dieser Indexer wird alle zwei Stunden ausgeführt (das Planungsintervall ist auf „PT2H“ festgelegt). Um einen Indexer alle 30 Minuten auszuführen, legen Sie das Intervall auf „PT30M“ fest. Das kürzeste unterstützte Intervall beträgt fünf Minuten. Der Zeitplan ist optional. Ohne Zeitplan wird ein Indexer nur einmal bei seiner Erstellung ausgeführt. Allerdings können Sie ein Indexer bei Bedarf jederzeit ausführen.   

Weitere Informationen zur API zum Erstellen eines Indexers finden Sie unter [Erstellen eines Indexers](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Weitere Informationen zum Definieren von Indexerzeitplänen finden Sie unter [Festlegen eines Zeitplans für Indexer in der kognitiven Azure-Suche](search-howto-schedule-indexers.md).

## <a name="use-net"></a>Verwenden von .NET

Das allgemein verfügbare .NET SDK ist vollständig gleichwertig mit der allgemein verfügbaren REST-API. Es wird empfohlen, dass Sie den vorherige Abschnitt zur REST-API genau lesen, um die Konzepte, den Workflow und die Anforderungen zu verstehen. Sie können sich anschließend auf die folgende .NET-API-Referenzdokumentation beziehen, um einen JSON-Indexer in verwalteten Code zu implementieren.

+ [microsoft.azure.search.models.datasource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [microsoft.azure.search.models.datasourcetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [microsoft.azure.search.models.index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [microsoft.azure.search.models.indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

<a name="DataChangeDetectionPolicy"></a>

## <a name="indexing-changed-documents"></a>Indizieren von geänderten Dokumenten

Die Richtlinie zum Erkennen von Datenänderungen dient einer effizienten Identifizierung geänderter Datenelemente. Derzeit ist die einzige unterstützte Richtlinie die `High Water Mark`-Richtlinie, die die `_ts`-Eigenschaft (Zeitstempel) verwendet, die von Azure Cosmos DB bereitgestellt wird. Diese wird wie folgt angegeben:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Wenn Sie diese Richtlinie verwenden, wird dringend empfohlen sicherzustellen, dass der Indexer eine hohe Leistung erzielt. 

Wenn Sie eine benutzerdefinierte Abfrage verwenden, stellen sicher, dass die `_ts`Eigenschaft von der Abfrage projiziert wird.

<a name="IncrementalProgress"></a>

### <a name="incremental-progress-and-custom-queries"></a>Inkrementeller Status und benutzerdefinierte Abfragen

Der inkrementelle Status während der Indizierung stellt sicher, dass der Indexer bei der nächsten Ausführung an der gleichen Stelle fortgesetzt werden kann und nicht die gesamte Sammlung von vorn indizieren muss, wenn er durch vorübergehende Fehler oder das Erreichen des Ausführungszeitlimits unterbrochen wird. Dies ist besonders beim Indizieren umfangreicher Sammlungen wichtig. 

Stellen Sie zum Aktivieren des inkrementellen Status bei Verwendung einer benutzerdefinierten Abfrage sicher, dass die Abfrage die Ergebnisse nach der Spalte `_ts` sortiert. Dadurch können regelmäßige Prüfpunkte erstellt werden, die in der kognitiven Azure-Suche bei Fehlern zur Bereitstellung des inkrementellen Status verwendet werden.   

In bestimmten Fällen kann in der kognitiven Azure-Suche unter Umständen nicht erkannt werden, dass die Abfrage nach `_ts` sortiert ist. (Dies kann sogar vorkommen, wenn die Abfrage eine `ORDER BY [collection alias]._ts`-Klausel enthält.) Mithilfe der Konfigurationseigenschaft `assumeOrderByHighWaterMarkColumn` können Sie in der kognitiven Azure-Suche angeben, dass die Ergebnisse sortiert sind. Erstellen oder aktualisieren Sie Ihren Indexer wie folgt, um diesen Hinweis anzugeben: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>

## <a name="indexing-deleted-documents"></a>Indizieren von gelöschten Dokumenten

Wenn Zeilen aus der Quelltabelle gelöscht werden, möchten Sie diese Zeilen in der Regel auch aus dem Suchindex löschen. Die Richtlinie zum Erkennen von Datenlöschungen dient einer effizienten Identifizierung gelöschter Datenelemente. Zurzeit ist `Soft Delete` die einzige unterstützte Richtlinie (die Löschung wird durch ein bestimmtes Kennzeichen markiert). Diese wird folgendermaßen festgelegt:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

Wenn Sie eine benutzerdefinierte Abfrage verwenden, stellen Sie sicher, dass die Eigenschaft, auf die `softDeleteColumnName` verweist, von der Abfrage projiziert wird.

Im folgenden Beispiel wird eine Datenquelle mit einer Richtlinie zum vorläufigen Löschen erstellt:

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mycosmosdbdatasource",
        "type": "cosmosdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myCosmosDbEndpoint.documents.azure.com;AccountKey=myCosmosDbAuthKey;Database=myCosmosDbDatabaseId"
        },
        "container": { "name": "myCosmosDbCollectionId" },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

## <a name="NextSteps"></a>Nächste Schritte

Glückwunsch! Sie wissen nun, wie Azure Cosmos DB unter Verwendung eines Indexers in die kognitive Azure-Suche integriert wird.

* Weitere Informationen zu Azure Cosmos DB finden Sie auf der [Seite über den Azure Cosmos DB-Dienst](https://azure.microsoft.com/services/cosmos-db/).
* Weitere Informationen zur kognitiven Azure-Suche finden Sie auf der [Seite des Search-Diensts](https://azure.microsoft.com/services/search/).
