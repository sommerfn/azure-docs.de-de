---
title: 'REST-Tutorial: Indizieren von teilweise strukturierten Daten in JSON-Blobs'
titleSuffix: Azure Cognitive Search
description: Hier erfahren Sie, wie Sie teilweise strukturierte Azure-JSON-Blobs mithilfe von Azure Cognitive Search-REST-APIs und Postman indizieren und durchsuchen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 569289a2d750f96423bd03ac82cb9e33f893ee15
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72794290"
---
# <a name="rest-tutorial-index-and-search-semi-structured-data-json-blobs-in-azure-cognitive-search"></a>REST-Tutorial: Indizieren und Durchsuchen von teilweise strukturierten Daten (JSON-Blobs) in Azure Cognitive Search

Azure Cognitive Search kann JSON-Dokumente und -Arrays in Azure Blob Storage mithilfe eines [Indexers](search-indexer-overview.md) indizieren, der teilweise strukturierte Daten lesen kann. Teilweise strukturierte Daten enthalten Tags oder Markierungen, mit denen Inhalte in den Daten voneinander getrennt werden. Sie bilden den Unterschied zwischen unstrukturierten Daten, die vollständig indiziert werden müssen, und formal strukturierten Daten, die einem Datenmodell entsprechen (wie z. B. einem Schema einer relationalen Datenbank), das pro Feld indiziert werden kann.

In diesem Tutorial verwenden Sie die [Azure Cognitive Search-REST-APIs](https://docs.microsoft.com/rest/api/searchservice/) und einen REST-Client, um die folgenden Aufgaben auszuführen:

> [!div class="checklist"]
> * Konfigurieren einer Azure Cognitive Search-Datenquelle für einen Azure-Blobcontainer
> * Erstellen eines Azure Cognitive Search-Index mit durchsuchbarem Inhalt
> * Konfigurieren und Ausführen eines Indexers zum Lesen des Containers und zum Extrahieren von durchsuchbaren Inhalten aus Azure Blob Storage
> * Durchsuchen des soeben erstellten Index

## <a name="prerequisites"></a>Voraussetzungen

In diesem Schnellstart werden die folgenden Dienste, Tools und Daten verwendet. 

[Erstellen Sie einen Azure Cognitive Search-Dienst](search-create-service-portal.md), oder suchen Sie in Ihrem aktuellen Abonnement [nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). In diesem Tutorial können Sie einen kostenlosen Dienst verwenden. 

[Erstellen Sie ein Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) zum Speichern der Beispieldaten.

Die [Postman-Desktop-App](https://www.getpostman.com/) zum Senden von Anforderungen an Azure Cognitive Search

[Clinical-trials-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip) enthält die Daten, die in diesem Tutorial verwendet werden. Laden Sie die Datei herunter, und entzippen Sie sie in einem eigenen Ordner. Die Daten stammen aus [ClinicalTrials.gov](https://clinicaltrials.gov/ct2/results) und werden für dieses Tutorial in JSON konvertiert.

## <a name="get-a-key-and-url"></a>Abrufen eines Schlüssels und einer URL

Für REST-Aufrufe sind die Dienst-URL und ein Zugriffsschlüssel für jede Anforderung erforderlich. Ein Suchdienst wird mit beidem erstellt. Gehen Sie daher wie folgt vor, um die erforderlichen Informationen zu erhalten, falls Sie Azure Cognitive Search Ihrem Abonnement hinzugefügt haben:

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com/), und rufen Sie auf der Seite **Übersicht** Ihres Suchdiensts die URL ab. Ein Beispiel für einen Endpunkt ist `https://mydemo.search.windows.net`.

1. Rufen Sie unter **Einstellungen** > **Schlüssel** einen Administratorschlüssel ab, um Vollzugriff auf den Dienst zu erhalten. Es gibt zwei austauschbare Administratorschlüssel – diese wurden zum Zweck der Geschäftskontinuität bereitgestellt, falls Sie einen Rollover für einen Schlüssel durchführen müssen. Für Anforderungen zum Hinzufügen, Ändern und Löschen von Objekten können Sie den primären oder den sekundären Schlüssel verwenden.

![Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels](media/search-get-started-postman/get-url-key.png "Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels")

Für alle an Ihren Dienst gesendeten Anforderungen ist ein API-Schlüssel erforderlich. Ein gültiger Schlüssel stellt anforderungsbasiert eine Vertrauensstellung her zwischen der Anwendung, die die Anforderung versendet, und dem Dienst, der sie verarbeitet.

## <a name="prepare-sample-data"></a>Vorbereiten der Beispieldaten

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com), navigieren Sie zu Ihrem Azure-Speicherkonto, klicken Sie auf **BLOBs** und dann auf **+ Container**.

1. [Erstellen Sie einen Blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) für die Beispieldaten. Sie können die öffentliche Zugriffsebene auf beliebige gültige Werte festlegen.

1. Nachdem der Container erstellt wurde, öffnen Sie ihn, und wählen Sie auf der Befehlsleiste die Option **Hochladen** aus.

   ![„Hochladen“ auf der Befehlsleiste](media/search-semi-structured-data/upload-command-bar.png "„Hochladen“ auf der Befehlsleiste")

1. Navigieren Sie zu dem Ordner, der die Beispieldateien enthält. Wählen Sie alle Dateien aus, und klicken Sie dann auf **Hochladen**.

   ![Hochladen von Dateien](media/search-semi-structured-data/clinicalupload.png "Hochladen von Dateien")

Nach Abschluss des Uploads sollten die Dateien im Datencontainer in einem eigenen Unterordner angezeigt werden.

## <a name="set-up-postman"></a>Einrichten von Postman

Starten Sie Postman, und richten Sie eine HTTP-Anforderung ein. Wenn Sie mit diesem Tool nicht vertraut sind, lesen Sie [Untersuchen von Azure Cognitive Search-REST-APIs mit Postman](search-get-started-postman.md).

Für alle Aufrufe in diesem Tutorial wird die Anforderungsmethode **POST** verwendet. Die Headerschlüssel sind „Content-type“ und „api-key“. Die Werte der Headerschlüssel sind „application/json“ bzw. Ihr „Administratorschlüssel“ (der Administratorschlüssel ist ein Platzhalter für Ihren Primärschlüssel für die Suche). Im Text ordnen Sie den eigentlichen Inhalt Ihres Aufrufs an. Je nach verwendetem Client kann der Aufbau Ihrer Abfrage leicht abweichen, aber hier erhalten Sie die grundlegenden Informationen.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/postmanoverview.png)

Wir nutzen Postman zum Senden von drei API-Aufrufen an Ihren Suchdienst, um eine Datenquelle, einen Index und einen Indexer zu erstellen. Die Datenquelle enthält einen Zeiger auf Ihr Speicherkonto und Ihre JSON-Daten. Ihr Suchdienst stellt die Verbindung beim Laden der Daten her.

In Abfragezeichenfolgen muss eine API-Version angegeben werden, und für jeden Aufruf sollte **201 Created** zurückgegeben werden. Die allgemein verfügbare API-Version zur Verwendung von JSON-Arrays lautet `2019-05-06`.

Führen Sie die folgenden drei API-Aufrufe über Ihren REST-Client aus.

## <a name="create-a-data-source"></a>Erstellen einer Datenquelle

Die [API zum Erstellen einer Datenquelle](https://docs.microsoft.com/rest/api/searchservice/create-data-source) erstellt ein Azure Cognitive Search-Objekt, das angibt, welche Daten indiziert werden sollen.

Der Endpunkt dieses Aufrufs lautet `https://[service name].search.windows.net/datasources?api-version=2019-05-06`. Ersetzen Sie `[service name]` durch den Namen Ihres Suchdiensts. 

Für diesen Aufruf muss der Anforderungstext den Namen Ihres Speicherkontos, den Speicherkontoschlüssel und den Namen des Blobcontainers enthalten. Sie finden den Speicherkontoschlüssel im Azure-Portal unter den **Zugriffsschlüsseln** Ihres Speicherkontos. Der Speicherort ist in der folgenden Abbildung angegeben:

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/storagekeys.png)

Denken Sie daran, `[storage account name]`, `[storage account key]` und `[blob container name]` im Text Ihres Aufrufs durch die entsprechenden Werte zu ersetzen, bevor Sie den Aufruf ausführen.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "[blob container name]"}
}
```

Die Antwort sollte in etwa wie folgt aussehen:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#datasources/$entity",
    "@odata.etag": "\"0x8D505FBC3856C9E\"",
    "name": "clinical-trials-json",
    "description": null,
    "type": "azureblob",
    "subtype": null,
    "credentials": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=[mystorageaccounthere];AccountKey=[[myaccountkeyhere]]];"
    },
    "container": {
        "name": "[mycontainernamehere]",
        "query": null
    },
    "dataChangeDetectionPolicy": null,
    "dataDeletionDetectionPolicy": null
}
```

## <a name="create-an-index"></a>Erstellen eines Index
    
Beim zweiten Aufruf wird die [API zum Erstellen eines Index](https://docs.microsoft.com/rest/api/searchservice/create-indexer) aufgerufen, die einen Azure Cognitive Search-Index mit allen durchsuchbaren Daten erstellt. Ein Index gibt alle Parameter und die dazugehörigen Attribute an.

Die URL für diesen Aufruf lautet `https://[service name].search.windows.net/indexes?api-version=2019-05-06`. Ersetzen Sie `[service name]` durch den Namen Ihres Suchdiensts.

Ersetzen Sie zunächst die URL. Fügen Sie den folgenden Code in den Text ein, und führen Sie die Abfrage aus.

```json
{
  "name": "clinical-trials-json-index",  
  "fields": [
  {"name": "FileName", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "Description", "type": "Edm.String", "searchable": true, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MinimumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Title", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "URL", "type": "Edm.String", "searchable": false, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MyURL", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "Gender", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "MaximumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Summary", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "NCTID", "type": "Edm.String", "key": true, "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "Phase", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Date", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "OverallStatus", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "OrgStudyId", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": false},
  {"name": "HealthyVolunteers", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Keywords", "type": "Collection(Edm.String)", "searchable": true, "retrievable": true, "facetable": true, "filterable": false, "sortable": false},
  {"name": "metadata_storage_last_modified", "type":"Edm.DateTimeOffset", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_storage_size", "type":"Edm.String", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_content_type", "type":"Edm.String", "searchable": true, "retrievable": true, "filterable": true, "sortable": false}
  ],
  "suggesters": [
  {
    "name": "sg",
    "searchMode": "analyzingInfixMatching",
    "sourceFields": ["Title"]
  }
  ]
}
```

Die Antwort sollte in etwa wie folgt aussehen:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexes/$entity",
    "@odata.etag": "\"0x8D505FC00EDD5FA\"",
    "name": "clinical-trials-json-index",
    "fields": [
        {
            "name": "FileName",
            "type": "Edm.String",
            "searchable": false,
            "filterable": false,
            "retrievable": true,
            "sortable": true,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "Description",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "retrievable": false,
            "sortable": false,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        ...
          "scoringProfiles": [],
    "defaultScoringProfile": null,
    "corsOptions": null,
    "suggesters": [],
    "analyzers": [],
    "tokenizers": [],
    "tokenFilters": [],
    "charFilters": []
}
```

## <a name="create-and-run-an-indexer"></a>Erstellen und Ausführen eines Indexers

Ein Indexer verbindet die Datenquelle, importiert Daten in den Zielsuchindex und stellt optional einen Zeitplan für die Automatisierung der Datenaktualisierung bereit. Die REST-API für diese Aufgabe ist [Indexer erstellen](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Die URL für diesen Aufruf lautet `https://[service name].search.windows.net/indexers?api-version=2019-05-06`. Ersetzen Sie `[service name]` durch den Namen Ihres Suchdiensts.

Ersetzen Sie zunächst die URL. Kopieren Sie dann den folgenden Code, fügen Sie ihn in den Text ein, und senden Sie die Anforderung. Die Anforderung wird sofort verarbeitet. Wenn die Antwort eintrifft, verfügen Sie über einen Index, der für die Volltextsuche verwendet werden kann.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

Die Antwort sollte in etwa wie folgt aussehen:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexers/$entity",
    "@odata.etag": "\"0x8D505FDE143D164\"",
    "name": "clinical-trials-json-indexer",
    "description": null,
    "dataSourceName": "clinical-trials-json",
    "targetIndexName": "clinical-trials-json-index",
    "schedule": null,
    "parameters": {
        "batchSize": null,
        "maxFailedItems": null,
        "maxFailedItemsPerBatch": null,
        "base64EncodeKeys": null,
        "configuration": {
            "parsingMode": "jsonArray"
        }
    },
    "fieldMappings": [],
    "enrichers": [],
    "disabled": null
}
```

## <a name="search-your-json-files"></a>Durchsuchen von JSON-Dateien

Sie können mit der Suche beginnen, sobald das erste Dokument geladen wurde. Verwenden Sie für diese Aufgabe den [**Suchexplorer**](search-explorer.md) im Portal.

Öffnen Sie im Azure-Portal die Seite **Übersicht** des Suchdiensts, und suchen Sie in der Liste **Indizes** nach dem Index, den Sie erstellt haben.

Wählen Sie den soeben erstellten Index aus. 

  ![Unstrukturierte Suche](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Suche nach benutzerdefinierten Metadaten

Die Daten können auch hier wieder auf verschiedene Weise abgefragt werden: Volltextsuche, Systemeigenschaften oder benutzerdefinierte Metadaten. Sowohl Systemeigenschaften als auch benutzerdefinierte Metadaten können nur dann mit dem Parameter `$select` durchsucht werden, wenn sie beim Erstellen des Zielindex als **retrievable** (abrufbar) gekennzeichnet wurden. Parameter im Index können nach ihrer Erstellung nicht mehr geändert werden. Es können aber zusätzliche Parameter hinzugefügt werden.

Ein Beispiel für eine grundlegende Abfrage ist `$select=Gender,metadata_storage_size`, mit der die Rückgabe auf diese beiden Parameter beschränkt wird.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/lastquery.png)

Ein Beispiel für eine komplexere Abfrage ist `$filter=MinimumAge ge 30 and MaximumAge lt 75`. Hiermit werden nur Ergebnisse zurückgegeben, bei denen der Parameter MinimumAge größer oder gleich 30 und der Parameter MaximumAge kleiner als 75 ist.

  ![Teilweise strukturierte Suche](media/search-semi-structured-data/metadatashort.png)

Wenn Sie experimentieren und einige weitere Abfragen selbst ausprobieren möchten, können Sie dies tun. Denken Sie daran, dass Sie logische Operatoren (and, or, not) und Vergleichsoperatoren (eq, ne, gt, lt, ge, le) nutzen können. Bei Zeichenfolgenvergleichen wird die Groß-/Kleinschreibung beachtet.

Der Parameter `$filter` funktioniert nur mit Metadaten, die beim Erstellen des Index als filterbar gekennzeichnet wurden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Die schnellste Möglichkeit, das System nach einem Tutorial zu bereinigen, besteht im Löschen der Ressourcengruppe, die den Azure Cognitive Search-Dienst enthält. Sie können dann die Ressourcengruppe löschen, um alle darin enthaltenen Daten endgültig zu löschen. Der Name der Ressourcengruppe befindet sich im Portal auf der Seite „Übersicht“ des Azure Cognitive Search-Diensts.

## <a name="next-steps"></a>Nächste Schritte

Es gibt verschiedene Ansätze und mehrere Optionen für die Indizierung von JSON-Blobs. Überprüfen und testen Sie im nächsten Schritt die unterschiedlichen Optionen, um zu ermitteln, was für Ihr Szenario am besten funktioniert.

> [!div class="nextstepaction"]
> [Indizieren von JSON-Blobs mit dem Azure Cognitive Search-Blobindexer](search-howto-index-json-blobs.md)
