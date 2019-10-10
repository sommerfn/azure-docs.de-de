---
title: Erstellen eines Wissensspeichers mithilfe von REST – Azure Search
description: Hier erfahren Sie, wie Sie mithilfe der REST-API und von Postman einen Azure Search-Wissensspeicher zum Speichern von Anreicherungen aus der Pipeline für die kognitive Suche erstellen.
author: lobrien
services: search
ms.service: search
ms.topic: tutorial
ms.date: 10/01/2019
ms.author: laobri
ms.openlocfilehash: 26dc66474eecffd7f5a34bcfcaf93fd49f59606c
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936503"
---
# <a name="create-an-azure-search-knowledge-store-using-rest"></a>Erstellen eines Azure Search-Wissensspeichers mithilfe von REST

Der Wissensspeicher ist ein Azure Search-Feature, das Ausgaben aus einer KI-Anreicherungspipeline zur späteren Analyse oder anderweitigen Downstreamverarbeitung speichert. Eine Pipeline mit KI-Anreicherung akzeptiert Bilddateien oder unstrukturierte Textdateien, indiziert sie mithilfe von Azure Search, wendet KI-Anreicherungen von Cognitive Services (beispielsweise Bildanalyse und Verarbeitung natürlicher Sprache) an, und speichert die Ergebnisse in einem Wissensspeicher in Azure Storage. Der Wissensspeicher kann dann mithilfe von Tools wie Power BI oder Storage-Explorer erkundet werden.

In diesem Artikel wird die REST-API-Schnittstelle verwendet, um eine Gruppe von Hotelrezensionen zu erfassen, zu indizieren und KI-Anreicherungen auf sie anzuwenden. Die Hotelrezensionen werden in Azure Blob Storage importiert und die Ergebnisse als Wissensspeicher in Azure Table Storage gespeichert.

Nachdem Sie den Wissensspeicher erstellt haben, können Sie sich darüber informieren, wie Sie per [Storage-Explorer](knowledge-store-view-storage-explorer.md) oder [Power BI](knowledge-store-connect-power-bi.md) darauf zugreifen.

## <a name="1---create-services"></a>1\. Erstellen der Dienste

+ [Erstellen Sie einen Azure Search-Dienst](search-create-service-portal.md), oder suchen Sie in Ihrem aktuellen Abonnement [nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). In diesem Tutorial können Sie einen kostenlosen Dienst verwenden.

+ [Erstellen Sie ein Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) zum Speichern der Beispieldaten und des Wissensspeichers. Ihr Speicherkonto muss den gleichen Speicherort (z. B. „USA, Westen“) für Ihren Azure Search-Dienst verwenden. Für *Kontoart* muss *StorageV2 (universell V2)* (Standard) oder *Storage (universell V1)* festgelegt sein.

+ Empfohlen: [Postman-Desktop-App](https://www.getpostman.com/) zum Senden von Anforderungen an Azure Search. Sie können die REST-API mit jedem Tool verwenden, das HTTP-Anforderungen und -Antworten verarbeiten kann. Postman empfiehlt sich für die Untersuchung von REST-APIs, daher wird in diesem Artikel darauf Bezug genommen. Außerdem enthält der [Quellcode](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) für diesen Artikel eine Postman-Auflistung von Anforderungen. 

## <a name="2---store-the-data"></a>2\. Speichern der Daten

Laden Sie die CSV-Datei mit den Hotelrezensionen in Azure Blob Storage, damit sie für einen Azure Search-Indexer zur Verfügung steht und die KI-Anreicherungspipeline durchlaufen kann.

### <a name="create-an-azure-blob-container-with-the-data"></a>Erstellen eines Azure-Blobcontainers mit den Daten

1. [Laden Sie die CSV-Datei „HotelReviews_Free.csv“ mit den gespeicherten Hotelrezensionsdaten herunter.](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) Diese Daten stammen von Kaggle.com und enthalten Kundenfeedback zu Hotels.
1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com), und navigieren Sie zu Ihrem Azure-Speicherkonto.
1. [Erstellen Sie einen Blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). Klicken Sie zum Erstellen des Containers auf der linken Navigationsleiste für Ihr Speicherkonto auf **Blobs** und anschließend auf der Befehlsleiste auf **+ Container**.
1. Geben Sie unter **Name** den Namen `hotel-reviews` für den neuen Container ein.
1. Wählen Sie unter **Öffentliche Zugriffsebene** eine beliebige Option aus. Wir haben den Standardwert verwendet.
1. Klicken Sie auf **OK**, um den Azure-Blobcontainer zu erstellen.
1. Öffnen Sie den neuen Container (`hotels-review`), klicken Sie auf **Hochladen**, und wählen Sie die Datei **HotelReviews-Free.csv** aus, die Sie im ersten Schritt heruntergeladen haben.

    ![Hochladen der Daten](media/knowledge-store-create-portal/upload-command-bar.png "Hochladen der Hotelrezensionen")

1. Klicken Sie auf **Hochladen**, um die CSV-Datei in Azure Blob Storage zu importieren. Daraufhin wird der neue Container angezeigt.

    ![Erstellen des Azure-Blobcontainers](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "Erstellen des Azure-Blobcontainers")

## <a name="3---configure-postman"></a>3\. Konfigurieren von Postman

Laden Sie den [Quellcode der Postman-Auflistung](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) herunter, und importieren Sie ihn mit **Datei > Importieren...** in Postman. Wechseln Sie zur Registerkarte **Sammlungen**, klicken Sie auf die Schaltfläche **...** , und wählen Sie **Bearbeiten** aus. 

![Postman-App mit Navigation](media/knowledge-store-create-rest/postman-edit-menu.png "Navigieren zum Menü „Bearbeiten“ in Postman")

Navigieren Sie im geöffneten Dialogfeld „Bearbeiten“ zur Registerkarte **Variablen**. 

Auf der Registerkarte **Variablen** können Sie Werte hinzufügen, die Postman bei jedem ihrer Vorkommen in doppelten geschweiften Klammern einfügt. Postman ersetzt z. B. das Symbol `{{admin-key}}` mit dem Wert für `admin-key` unter „Aktueller Wert“. Diese Ersetzung wird von Postman in URLs, Headern, dem Anforderungstext usw. ausgeführt. 

Den Wert für `admin-key` finden Sie auf der Registerkarte **Schlüssel** des Suchdiensts. Sie müssen `search-service-name` und `storage-account-name` in die Werte ändern, die Sie in [Schritt 1](#1---create-services) ausgewählt haben. Legen Sie `storage-connection-string` anhand des Werts auf der Registerkarte **Zugriffsschlüssel** des Speicherkontos fest. Die anderen Werte können unverändert übernommen werden.

![Postman-App: Registerkarte „Variablen“](media/knowledge-store-create-rest/postman-variables-window.png "Fenster „Variablen“ in Postman")


| Variable    | Ursprung |
|-------------|-----------------|
| `admin-key` | Suchdienst, Registerkarte **Schlüssel**              |
| `api-version` | Als „2019-05-06-Preview“ beibehalten |
| `datasource-name` | Als „hotel-reviews-ds“ beibehalten | 
| `indexer-name` | Als „hotel-reviews-ixr“ beibehalten | 
| `index-name` | Als „hotel-reviews-ix“ beibehalten | 
| `search-service-name` | Suchdienst, Hauptname. Die URL ist `https://{{search-service-name}}.search.windows.net`. | 
| `skillset-name` | Als „hotel-reviews-ss“ beibehalten | 
| `storage-account-name` | Speicherkonto, Hauptname | 
| `storage-connection-string` | Speicherkonto, Registerkarte **Access Keys** (Zugriffsschlüssel), **key1** **Verbindungszeichenfolge** | 
| `storage-container-name` | Als „hotel-reviews“ beibehalten | 

### <a name="review-the-request-collection-in-postman"></a>Überprüfen der Auflistung von Anforderungen in Postman

Zum Erstellen eines Wissensspeichers müssen Sie vier HTTP-Anforderungen ausgeben: 

1. Eine PUT-Anforderung zum Erstellen des Indexes. Dieser Index enthält die Daten, die von Azure Search verwendet und zurückgegeben werden.
1. Eine POST-Anforderung zum Erstellen der Datenquelle. Diese Datenquelle verknüpft das Azure Search-Verhalten mit den Daten und dem Speicherkonto des Wissensspeichers. 
1. Eine PUT-Anforderung zum Erstellen des Skillsets. Das Skillset gibt die auf die Daten angewendeten Anreicherungen und die Struktur des Wissensspeichers an.
1. Eine PUT-Anforderung zum Erstellen des Indexers. Beim Ausführen des Indexers werden die Daten gelesen, das Skillset wird angewendet, und die Ergebnisse werden gespeichert. Diese Anforderung muss zuletzt ausgeführt werden.

Der [Quellcode](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) enthält eine Postman-Auflistung mit diesen vier Anforderungen. Wechseln Sie zum Ausgeben der Anforderungen zur Registerkarte der Anforderung in Postman, und fügen Sie die Anforderungsheader `api-key` und `Content-Type` hinzu. Legen Sie den Wert von `api-key` auf `{{admin-key}}` fest. Legen Sie den Wert `Content-type` auf `application/json` fest. 

> [!div class="mx-imgBorder"]
> ![Screenshot der Postman-Schnittstelle für Header](media/knowledge-store-create-rest/postman-headers-ui.png)

> [!Note]
> Sie müssen die Header `api-key` und `Content-type` in allen Anforderungen festlegen. Eine von Postman erkannte Variable wird in orangefarbenem Text dargestellt; siehe `{{admin-key}}` im Screenshot. Wenn die Variable falsch geschrieben ist, wird Sie in rotem Text dargestellt.
>

## <a name="4---create-an-azure-search-index"></a>4\. Erstellen eines Azure Search-Index

Sie müssen einen Azure Search-Index erstellen, um die Daten darzustellen, die Sie durchsuchen, filtern und erweitern möchten. Sie erstellen den Index, indem Sie eine PUT-Anforderung an `https://{{search-service-name}}.search.windows.net/indexes/{{index-name}}?api-version={{api-version}}` ausgeben. Postman ersetzt in doppelte geschweifte Klammern eingeschlossene Symbole (z. B. `{{search-service-name}}`, `{{index-name}}` und `{{api-version}}`) durch die in [Schritt 3](#3---configure-postman) angegebenen Werte. Wenn Sie Ihre REST-Befehle mit einem anderen Tool ausgeben, müssen Sie diese Variablen selbst ersetzen.

Geben Sie die Struktur des Azure Search-Indexes im Text der Anforderung an. Wechseln Sie in Postman nach dem Festlegen der Header `api-key` und `Content-type` zum Bereich **Text** der Anforderung. Der folgende JSON-Code sollte angezeigt werden; wählen Sie andernfalls **Raw** und **JSON (application/json)** aus, und fügen Sie den folgenden Code als Text ein:

```JSON
{
    "name": "{{index-name}}",
    "fields": [
        { "name": "address", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "categories", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "city", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false },
        { "name": "country", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "latitude", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "longitude", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "name", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false },
        { "name": "postalCode", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "province", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_date", "type": "Edm.DateTimeOffset", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_dateAdded", "type": "Edm.DateTimeOffset", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_rating", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_text", "type": "Edm.String", "filterable": false,  "sortable": false, "facetable": false },
        { "name": "reviews_title", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_username", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "AzureSearch_DocumentKey", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false, "key": true },
        { "name": "metadata_storage_content_type", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "metadata_storage_size", "type": "Edm.Int64", "searchable": false, "filterable": false, "sortable": false, "facetable": false},
        { "name": "metadata_storage_last_modified", "type": "Edm.DateTimeOffset", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "metadata_storage_name", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "metadata_storage_path", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "Sentiment", "type": "Collection(Edm.Double)", "searchable": false, "filterable": true, "retrievable": true, "sortable": false, "facetable": true },
        { "name": "Language", "type": "Edm.String", "filterable": true, "sortable": false, "facetable": true },
        { "name": "Keyphrases", "type": "Collection(Edm.String)", "filterable": true, "sortable": false, "facetable": true }
    ]
}

```

Sie stellen fest, diese Indexdefinition ist eine Kombination von Daten, die dem Benutzer präsentiert werden sollen (Name des Hotels, Inhalt der Rezension, Datum usw.), Suchmetadaten und KI-Erweiterungsdaten (Stimmung, Schlüsselausdrücke und Sprache).

Drücken Sie die Schaltfläche **Senden**, um die PUT-Anforderung auszugeben. Daraufhin sollte die Statusmeldung `201 - Created` angezeigt werden. Wenn Sie eine andere Statusmeldung erhalten, wird im Bereich **Text** eine JSON-Antwort mit einer Fehlermeldung angezeigt. 

## <a name="5---create-the-datasource"></a>5\. Erstellen der Datenquelle

Nun müssen Sie Azure Search mit den in [Schritt 2](#2---store-the-data) gespeicherten Hoteldaten verknüpfen. Das Erstellen der Datenquelle erfolgt mit einem POST an `https://{{search-service-name}}.search.windows.net/datasources?api-version={{api-version}}`. Sie müssen wieder die Header `api-key` und `Content-Type` festlegen, die Sie bereits früher angegeben haben. 

Öffnen Sie in Postman die Anforderung „Create Datasource“ (Datenquelle erstellen). Wechseln Sie zum Bereich **Text**; hier sollte der folgende Code angezeigt werden:

```json
{
  "name" : "{{datasource-name}}",
  "description" : "Demo files to demonstrate knowledge store capabilities.",
  "type" : "azureblob",
  "credentials" : { "connectionString" : "{{storage-connection-string}}" },
  "container" : { "name" : "{{storage-container-name}}" }
}
```

Drücken Sie die Schaltfläche **Senden**, um die POST-Anforderung auszugeben. 

## <a name="6---create-the-skillset"></a>6\. Erstellen des Skillsets 

Im nächsten Schritt wird das Skillset angegeben. Dieses gibt sowohl die anzuwendenden Erweiterungen als auch den Wissensspeicher an, in dem die Ergebnisse gespeichert werden. Öffnen Sie in Postman die Registerkarte „Create the Skillset“ (Skillset erstellen). Diese Anforderung sendet einen PUT an `https://{{search-service-name}}.search.windows.net/skillsets/{{skillset-name}}?api-version={{api-version}}`.
Legen Sie die Header `api-key` und `Content-type` wie zuvor fest. 

Es gibt zwei große Objekte der obersten Ebene: `"skills"` und `"knowledgeStore"`. Jedes Objekt innerhalb des `"skills"`-Objekts ist ein Anreicherungsdienst. Jeder Anreicherungsdienst verfügt über `"inputs"` und `"outputs"`. Beachten Sie, dass `LanguageDetectionSkill` eine Ausgabe-`targetName` von `"Language"` aufweist. Der Wert dieses Knotens wird von den meisten anderen Qualifikationen als Eingabe verwendet; die Quelle ist auf `document/Language` festgelegt. Diese Fähigkeit, die Ausgabe eines Knotens als Eingabe für einen anderen zu nutzen, wird noch besser ersichtlich im `ShaperSkill`. Dort wird angegeben, wie die Daten in die Tabellen im Wissensspeicher fließen.

Das `"knowledge_store"`-Objekt stellt über die `{{storage-connection-string}}`-Postman-Variable eine Verbindung mit dem Speicherkonto her. Dann enthält sie eine Gruppe von Zuordnungen zwischen dem erweiterten Dokument und Tabellen und Spalten, die im Wissensspeicher selbst verfügbar sind. 

Um das Skillset zu generieren, übermitteln Sie die Anforderung per PUT, indem Sie die Schaltfläche **Senden** in Postman drücken.

```json
{
    "name": "{{skillset-name}}",
    "description": "Skillset to detect language, extract key phrases, and detect sentiment",
    "skills": [ 
        {
            "@odata.type": "#Microsoft.Skills.Text.SplitSkill", 
            "context": "/document/reviews_text", "textSplitMode": "pages", "maximumPageLength": 5000,
            "inputs": [ 
                { "name": "text", "source": "/document/reviews_text" },
                { "name": "languageCode", "source": "/document/Language" }
            ],
            "outputs": [
                { "name": "textItems", "targetName": "pages" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
            "context": "/document/reviews_text/pages/*",
            "inputs": [
                { "name": "text", "source": "/document/reviews_text/pages/*" },
                { "name": "languageCode", "source": "/document/Language" }
            ],
            "outputs": [
                { "name": "score", "targetName": "Sentiment" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
            "context": "/document",
            "inputs": [
                { "name": "text", "source": "/document/reviews_text" }
            ],
            "outputs": [
                { "name": "languageCode", "targetName": "Language" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
            "context": "/document/reviews_text/pages/*",
            "inputs": [
                { "name": "text",  "source": "/document/reviews_text/pages/*" },
                { "name": "languageCode",  "source": "/document/Language" }
            ],
            "outputs": [
                { "name": "keyPhrases" , "targetName": "Keyphrases" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
            "context": "/document",
            "inputs": [
                { "name": "name",  "source": "/document/name" },
                { "name": "reviews_date",  "source": "/document/reviews_date" },
                { "name": "reviews_rating",  "source": "/document/reviews_rating" },
                { "name": "reviews_text",  "source": "/document/reviews_text" },
                { "name": "reviews_title",  "source": "/document/reviews_title" },
                { "name": "AzureSearch_DocumentKey",  "source": "/document/AzureSearch_DocumentKey" },
                { 
                    "name": "pages",
                    "sourceContext": "/document/reviews_text/pages/*",
                    "inputs": [
                        { "name": "SentimentScore", "source": "/document/reviews_text/pages/*/Sentiment" },
                        { "name": "LanguageCode", "source": "/document/Language" },
                        { "name": "Page", "source": "/document/reviews_text/pages/*" },
                        { 
                            "name": "keyphrase", "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                            "inputs": [
                                { "name": "Keyphrases", "source": "/document/reviews_text/pages/*/Keyphrases/*" }
                            ]
                        }
                    ]
                }
            ],
            "outputs": [
                { "name": "output" , "targetName": "tableprojection" }
            ]
        }
    ],
    "knowledgeStore": {
        "storageConnectionString": "{{storage-connection-string}}",
        "projections": [
            {
                "tables": [
                    { "tableName": "hotelReviewsDocument", "generatedKeyName": "Documentid", "source": "/document/tableprojection" },
                    { "tableName": "hotelReviewsPages", "generatedKeyName": "Pagesid", "source": "/document/tableprojection/pages/*" },
                    { "tableName": "hotelReviewsKeyPhrases", "generatedKeyName": "KeyPhrasesid", "source": "/document/tableprojection/pages/*/keyphrase/*" },
                    { "tableName": "hotelReviewsSentiment", "generatedKeyName": "Sentimentid", "source": "/document/tableprojection/pages/*/sentiment/*" }
                ],
                "objects": []
            },
            {
                "tables": [
                    { 
                        "tableName": "hotelReviewsInlineDocument", "generatedKeyName": "Documentid", "sourceContext": "/document",
                        "inputs": [
                            { "name": "name", "source": "/document/name"},
                            { "name": "reviews_date", "source": "/document/reviews_date"},
                            { "name": "reviews_rating", "source": "/document/reviews_rating"},
                            { "name": "reviews_text", "source": "/document/reviews_text"},
                            { "name": "reviews_title", "source": "/document/reviews_title"},
                            { "name": "AzureSearch_DocumentKey", "source": "/document/AzureSearch_DocumentKey" }
                        ]
                    },
                    { 
                        "tableName": "hotelReviewsInlinePages", "generatedKeyName": "Pagesid", "sourceContext": "/document/reviews_text/pages/*",
                        "inputs": [
                            { "name": "SentimentScore", "source": "/document/reviews_text/pages/*/Sentiment"},
                            { "name": "LanguageCode", "source": "/document/Language"},
                            { "name": "Page", "source": "/document/reviews_text/pages/*" }
                        ]
                    },
                    { 
                        "tableName": "hotelReviewsInlineKeyPhrases", "generatedKeyName": "kpidv2", "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                        "inputs": [
                            { "name": "Keyphrases", "source": "/document/reviews_text/pages/*/Keyphrases/*" }
                        ]
                    }
                ],
                "objects": []
            }
        ]
    }
}
```

## <a name="7---create-the-indexer"></a>7\. Erstellen des Indexers

Im letzten Schritt wird der Indexer erstellt, der die Daten liest und das Skillset aktiviert. Wechseln Sie in Postman zur Anforderung „Indexer erstellen“, und überprüfen Sie den Text. Sie stellen fest, dass die Definition des Indexers auf verschiedene andere bereits erstellte Ressourcen verweist: die Datenquelle, den Index und das Skillset. 

Das `"parameters/configuration"`-Objekt steuert die Erfassung der Daten durch den Indexer. In diesem Fall befinden sich die Eingabedaten in einem einzigen Dokument mit einer Kopfzeile und durch Trennzeichen getrennten Werten. Der Dokumentschlüssel ist ein eindeutiger Bezeichner für das Dokument. Vor der Codierung entspricht er der URL des Quelldokuments. Schließlich werden die Ausgabewerte des Skillsets wie Sprachcode, Stimmung und Schlüsselbegriffe ihren entsprechenden Positionen im Dokument zugeordnet. Beachten Sie, dass es einen einzelnen Wert für `Language` gibt, `Sentiment` jedoch auf jedes Element im Array von `pages` angewendet wird. `Keyphrases` stellt selbst ein Array dar und wird ebenso auf jedes Element im Array `pages` angewendet.

Nachdem Sie die Header `api-key` und `Content-type` festgelegt und sich vergewissert haben, dass der Text der Anforderung dem folgenden Code ähnelt, drücken Sie **Senden** in Postman. Postman sendet die Anforderung per PUT an `https://{{search-service-name}}.search.windows.net/indexers/{{indexer-name}}?api-version={{api-version}}`. Der Indexer wird von Azure Search erstellt und ausgeführt. 

```json
{
    "name": "{{indexer-name}}",
    "dataSourceName": "{{datasource-name}}",
    "skillsetName": "{{skillset-name}}",
    "targetIndexName": "{{index-name}}",
    "parameters": {
        "configuration": {
            "dataToExtract": "contentAndMetadata",
            "parsingMode": "delimitedText",
            "firstLineContainsHeaders": true,
            "delimitedTextDelimiter": ","
        }
    },
    "fieldMappings": [
        {
            "sourceFieldName": "AzureSearch_DocumentKey",
            "targetFieldName": "AzureSearch_DocumentKey",
            "mappingFunction": { "name": "base64Encode" }
        }
    ],
    "outputFieldMappings": [
        { "sourceFieldName": "/document/reviews_text/pages/*/Keyphrases/*", "targetFieldName": "Keyphrases" },
        { "sourceFieldName": "/document/Language", "targetFieldName": "Language" },
        { "sourceFieldName": "/document/reviews_text/pages/*/Sentiment", "targetFieldName": "Sentiment" }
    ]
}
```

## <a name="8---run-the-indexer"></a>8\. Ausführen des Indexers 

Navigieren Sie im Azure-Portal zur **Übersicht** des Suchdiensts, und wählen Sie die Registerkarte **Indexer** aus. Klicken Sie auf den Indexer **hotels-reviews-ixr**, den Sie im vorherigen Schritt erstellt haben. Wenn der Indexer noch nicht ausgeführt wurde, drücken Sie die Schaltfläche **Ausführen**. Die Indizierungsaufgabe gibt möglicherweise einige Warnung zur Spracherkennung aus, da die Daten auch Rezensionen enthalten, die in Sprachen verfasst wurden, welche von den kognitiven Qualifikationen noch nicht unterstützt werden. 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Daten mithilfe von Cognitive Services angereichert und die Ergebnisse in einen Wissensspeicher projiziert haben, können Sie Storage-Explorer oder Power BI verwenden, um Ihr angereichertes Dataset zu erkunden.

In der folgenden exemplarischen Vorgehensweise erfahren Sie, wie Sie diesen Wissensspeicher mithilfe von Storage-Explorer erkunden:

> [!div class="nextstepaction"]
> [Anzeigen mit Storage-Explorer](knowledge-store-view-storage-explorer.md)

In der folgenden exemplarischen Vorgehensweise erfahren Sie, wie Sie diesen Wissensspeicher mit Power BI verbinden:

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit Power BI](knowledge-store-connect-power-bi.md)

Wenn Sie diese Übung wiederholen oder eine andere exemplarische Vorgehensweise für die KI-Anreicherung ausprobieren möchten, löschen Sie den Indexer *hotel-reviews-idxr*. Durch Löschen des Indexers wird der Zähler für kostenlose Transaktionen pro Tag auf Null zurückgesetzt.
