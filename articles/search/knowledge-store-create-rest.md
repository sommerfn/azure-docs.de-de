---
title: Erstellen eines Wissensspeichers (Vorschau) mithilfe von REST
titleSuffix: Azure Cognitive Search
description: Verwenden Sie die REST-API und Postman, um einen Azure Cognitive Search-Wissensspeicher zum Speichern von Anreicherungen aus einer KI-Anreicherungspipeline zu erstellen. Dieses Feature ist zurzeit als öffentliche Preview verfügbar.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 107dcfa9ea312774e679c301ea934255c7b836c0
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73720067"
---
# <a name="create-an-azure-cognitive-search-knowledge-store-by-using-rest"></a>Erstellen eines Azure Cognitive Search-Wissensspeichers mit REST

> [!IMPORTANT] 
> „Wissensspeicher“ ist zurzeit als öffentliche Vorschauversion verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Previewfunktionen werden von der [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Die Portalunterstützung ist momentan eingeschränkt, und das .NET SDK wird nicht unterstützt.

Mit dem Wissensspeicher-Feature in Azure Cognitive Search werden Ausgaben aus einer KI-Anreicherungspipeline zur späteren Analyse oder anderweitigen Downstreamverarbeitung gespeichert. Eine Pipeline mit KI-Anreicherung akzeptiert Bilddateien oder unstrukturierte Textdateien, indiziert sie mithilfe von Azure Cognitive Search, wendet KI-Anreicherungen von Azure Cognitive Services (z. B. Bildanalyse und Verarbeitung natürlicher Sprache) an und speichert die Ergebnisse in einem Wissensspeicher in Azure Storage. Sie können den Wissensspeicher dann im Azure-Portal mit Tools wie Power BI oder Storage-Explorer erkunden.

In diesem Artikel wird die REST-API-Schnittstelle verwendet, um eine Gruppe von Hotelrezensionen zu erfassen, zu indizieren und KI-Anreicherungen darauf anzuwenden. Die Hotelrezensionen werden in Azure Blob Storage importiert. Die Ergebnisse werden als Wissensspeicher in Azure Table Storage gespeichert.

Nachdem Sie den Wissensspeicher erstellt haben, können Sie sich darüber informieren, wie Sie per [Storage-Explorer](knowledge-store-view-storage-explorer.md) oder [Power BI](knowledge-store-connect-power-bi.md) darauf zugreifen.

## <a name="create-services"></a>Erstellen von Diensten

Erstellen Sie die folgenden Dienste:

- [Erstellen Sie einen Azure Cognitive Search-Dienst](search-create-service-portal.md), oder suchen Sie in Ihrem aktuellen Abonnement [nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). In diesem Tutorial können Sie einen kostenlosen Dienst verwenden.

- Erstellen Sie ein [Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) zum Speichern der Beispieldaten und des Wissensspeichers. Ihr Speicherkonto muss den gleichen Speicherort (z. B. „USA, Westen“) für Ihren Azure Cognitive Search-Dienst verwenden. Der Wert für **Kontoart** muss auf **StorageV2 (universell V2)** (Standard) oder **Storage (universell V1)** festgelegt sein.

- Empfohlen: Installieren Sie die [Postman-Desktop-App](https://www.getpostman.com/) zum Senden von Anforderungen an Azure Cognitive Search. Sie können die REST-API mit jedem Tool verwenden, das HTTP-Anforderungen und -Antworten verarbeiten kann. Postman ist eine gute Wahl für die Untersuchung von REST-APIs. In diesem Artikel verwenden wir Postman. Außerdem enthält der [Quellcode](https://github.com/Azure-Samples/azure-search-postman-samples/tree/master/knowledge-store) für diesen Artikel eine Postman-Auflistung der Anforderungen. 

## <a name="store-the-data"></a>Speichern der Daten

Laden Sie die CSV-Datei mit den Hotelrezensionen in Azure Blob Storage, damit sie für einen Azure Cognitive Search-Indexer zur Verfügung steht und die KI-Anreicherungspipeline durchlaufen kann.

### <a name="create-a-blob-container-by-using-the-data"></a>Erstellen eines Blobcontainers mit den Daten

1. Laden Sie die CSV-Datei „HotelReviews_Free.csv“ mit den gespeicherten [Hotelrezensionsdaten](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) herunter. Diese Daten stammen von Kaggle.com und enthalten Kundenfeedback zu Hotels.
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem Azure-Speicherkonto.
1. Erstellen Sie einen [Blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). Wählen Sie zum Erstellen des Containers im linken Menü für Ihr Speicherkonto die Option **Blobs** und dann **Container** aus.
1. Geben Sie unter **Name** den Namen **hotel-reviews** für den neuen Container ein.
1. Wählen Sie unter **Öffentliche Zugriffsebene** einen beliebigen Wert aus. Wir haben den Standardwert verwendet.
1. Wählen Sie **OK** aus, um den Blobcontainer zu erstellen.
1. Öffnen Sie den neuen Container **hotels-review**, und wählen Sie **Hochladen** und dann die Datei „HotelReviews-Free.csv“ aus, die Sie im ersten Schritt heruntergeladen haben.

    ![Hochladen der Daten](media/knowledge-store-create-portal/upload-command-bar.png "Hochladen der Hotelrezensionen")

1. Wählen Sie **Hochladen** aus, um die CSV-Datei in Azure Blob Storage zu importieren. Der neue Container wird angezeigt:

    ![Erstellen des Blobcontainers](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "Erstellen des Blobcontainers")

## <a name="configure-postman"></a>Konfigurieren von Postman

Führen Sie die Installation und Einrichtung von Postman durch.

### <a name="download-and-install-postman"></a>Herunterladen und Installieren von Postman

1. Laden Sie den [Quellcode für die Postman-Sammlung](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) herunter.
1. Wählen Sie **Datei** > **Importieren** aus, um den Quellcode in Postman zu importieren.
1. Wählen Sie die Registerkarte **Sammlungen** und dann die Schaltfläche **...** (Auslassungszeichen) aus.
1. Wählen Sie **Bearbeiten** aus. 
   
   ![Postman-App mit Navigation](media/knowledge-store-create-rest/postman-edit-menu.png "Navigieren zum Bearbeitungsmenü in Postman")
1. Wählen Sie im Dialogfeld **Edit** (Bearbeiten) die Registerkarte **Variables** (Variablen) aus. 

Auf der Registerkarte **Variables** (Variablen) können Sie Werte hinzufügen, die von Postman jeweils eingefügt werden, wenn eine bestimmte Variable in doppelten Klammern erkannt wird. Beispielsweise ersetzt Postman das Symbol `{{admin-key}}` durch den aktuellen Wert, den Sie für `admin-key` festlegen. Postman führt die Ersetzung in URLs, Headern, im Anforderungstext usw. durch. 

Navigieren Sie zum Azure Cognitive Search-Dienst, und wählen Sie die Registerkarte **Schlüssel** aus, um den Wert für `admin-key` zu erhalten. Ändern Sie `search-service-name` und `storage-account-name` in die Werte, die Sie unter [Erstellen von Diensten](#create-services) ausgewählt haben. Legen Sie `storage-connection-string` fest, indem Sie den Wert auf der Registerkarte **Zugriffsschlüssel** des Speicherkontos verwenden. Sie können für die anderen Werte die Standardwerte übernehmen.

![Postman-App, Registerkarte mit den Variablen](media/knowledge-store-create-rest/postman-variables-window.png "Fenster mit den Postman-Variablen")


| Variable    | Ursprung |
|-------------|-----------------|
| `admin-key` | Auf der Seite **Schlüssel** des Azure Cognitive Search-Diensts  |
| `api-version` | Übernehmen Sie **2019-05-06-Preview**. |
| `datasource-name` | Übernehmen Sie **hotel-reviews-ds**. | 
| `indexer-name` | Übernehmen Sie **hotel-reviews-ixr**. | 
| `index-name` | Übernehmen Sie **hotel-reviews-ix**. | 
| `search-service-name` | Der Name des Azure Cognitive Search-Diensts. Die URL ist `https://{{search-service-name}}.search.windows.net`. | 
| `skillset-name` | Übernehmen Sie **hotel-reviews-ss**. | 
| `storage-account-name` | Der Name des Speicherkontos. | 
| `storage-connection-string` | Wählen Sie im Speicherkonto auf der Registerkarte **Zugriffsschlüssel** **key1** > **Verbindungszeichenfolge** aus. | 
| `storage-container-name` | Übernehmen Sie **hotel-reviews**. | 

### <a name="review-the-request-collection-in-postman"></a>Überprüfen der Auflistung von Anforderungen in Postman

Beim Erstellen eines Wissensspeichers müssen Sie vier HTTP-Anforderungen durchführen: 

- **PUT-Anforderung zum Erstellen des Index**: Dieser Index enthält die Daten, die von Azure Cognitive Search verwendet und zurückgegeben werden.
- **POST-Anforderung zum Erstellen der Datenquelle**: Diese Datenquelle verknüpft das Azure Cognitive Search-Verhalten mit den Daten und dem Speicherkonto des Wissensspeichers. 
- **PUT-Anforderung zum Erstellen des Skillsets**: Das Skillset gibt die Anreicherungen an, die auf die Daten und die Struktur des Wissensspeichers angewendet werden.
- **PUT-Anforderung zum Erstellen des Indexers**: Beim Ausführen des Indexers werden die Daten gelesen, das Skillset wird angewendet, und die Ergebnisse werden gespeichert. Diese Anforderung muss zuletzt ausgeführt werden.

Der [Quellcode](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) enthält eine Postman-Auflistung, in der diese vier Anforderungen enthalten sind. Wählen Sie zum Erstellen der Anforderungen in Postman die jeweilige Registerkarte für die Anforderung aus. Fügen Sie anschließend die Anforderungsheader `api-key` und `Content-Type` hinzu. Legen Sie den Wert von `api-key` auf `{{admin-key}}` fest. Legen Sie den Wert `Content-type` auf `application/json` fest. 

![Screenshot: Postman-Schnittstelle für Header](media/knowledge-store-create-rest/postman-headers-ui.png)

> [!Note]
> Sie müssen die Header `api-key` und `Content-type` in allen Anforderungen festlegen. Wenn von Postman eine Variable erkannt wird, wird sie als orangefarbener Text angezeigt (wie `{{admin-key}}` im obigen Screenshot). Falls die Variable falsch geschrieben ist, wird sie als roter Text dargestellt.
>

## <a name="create-an-azure-cognitive-search-index"></a>Erstellen eines Azure Cognitive Search-Index

Erstellen Sie einen Azure Cognitive Search-Index, um die Daten darzustellen, die Sie durchsuchen, filtern und erweitern möchten. Erstellen Sie den Index, indem Sie eine PUT-Anforderung an `https://{{search-service-name}}.search.windows.net/indexes/{{index-name}}?api-version={{api-version}}` senden. In Postman werden Symbole, die in doppelte geschweifte Klammern gesetzt sind (z. B. `{{search-service-name}}`, `{{index-name}}` und `{{api-version}}`), durch die Werte ersetzt, die Sie unter [Konfigurieren von Postman](#configure-postman) festlegen. Falls Sie ein anderes Tool zum Ausführen Ihrer REST-Befehle verwenden, müssen Sie diese Variablen selbst ersetzen.

Legen Sie die Struktur des Azure Cognitive Search-Index im Text der Anforderung fest. Navigieren Sie in Postman nach dem Festlegen der Header `api-key` und `Content-type` zum Bereich **Body** (Text) der Anforderung. Der folgende JSON-Code sollte angezeigt werden. Wenn nicht: Wählen Sie **Raw** > **JSON (application/json)** aus, und fügen Sie anschließend den folgenden Code als Text hinzu:

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

Diese Indexdefinition ist eine Kombination von Daten, die dem Benutzer präsentiert werden sollen (Name des Hotels, Inhalt der Rezension, Datum), Suchmetadaten und KI-Erweiterungsdaten (Stimmung, Schlüsselbegriffe und Sprache).

Wählen Sie **Send** (Senden) aus, um die PUT-Anforderung zu senden. Als Status sollte `201 - Created` angezeigt werden. Wenn ein anderer Status angezeigt wird, sollten Sie im Bereich **Body** (Text) nach einer JSON-Antwort suchen, die eine Fehlermeldung enthält. 

## <a name="create-the-datasource"></a>Erstellen der Datenquelle

Verbinden Sie als Nächstes Azure Cognitive Search mit den Hoteldaten, die Sie unter [Speichern der Daten](#store-the-data) gespeichert haben. Senden Sie eine POST-Anforderung an `https://{{search-service-name}}.search.windows.net/datasources?api-version={{api-version}}`, um die Datenquelle zu erstellen. Sie müssen die Header `api-key` und `Content-Type` wie oben beschrieben festlegen. 

Navigieren Sie in Postman zur Anforderung **Create Datasource** (Datenquelle erstellen) und dann zum Bereich **Body** (Text). Der folgende Code sollte angezeigt werden:

```json
{
  "name" : "{{datasource-name}}",
  "description" : "Demo files to demonstrate knowledge store capabilities.",
  "type" : "azureblob",
  "credentials" : { "connectionString" : "{{storage-connection-string}}" },
  "container" : { "name" : "{{storage-container-name}}" }
}
```

Wählen Sie **Send** (Senden) aus, um die POST-Anforderung zu senden. 

## <a name="create-the-skillset"></a>Erstellen des Skillsets 

Im nächsten Schritt wird das Skillset angegeben. Hiermit werden sowohl die anzuwendenden Erweiterungen als auch der Wissensspeicher angegeben, in dem die Ergebnisse gespeichert werden. Wählen Sie in Postman die Registerkarte **Create the Skillset** (Skillset erstellen) aus. Diese Anforderung sendet einen PUT an `https://{{search-service-name}}.search.windows.net/skillsets/{{skillset-name}}?api-version={{api-version}}`. Legen Sie die Header `api-key` und `Content-type` wie zuvor fest. 

Es gibt zwei große Objekte der obersten Ebene: `skills` und `knowledgeStore`. Jedes Objekt innerhalb des `skills`-Objekts ist ein Anreicherungsdienst. Jeder Anreicherungsdienst verfügt über `inputs` und `outputs`. Für `LanguageDetectionSkill` wird für `targetName` das Ergebnis `Language` ausgegeben. Der Wert dieses Knotens wird von den meisten anderen Skills als Eingabe verwendet. Die Quelle ist `document/Language`. Die Fähigkeit, die Ausgabe eines Knotens als Eingabe für einen anderen zu nutzen, wird mit `ShaperSkill` noch besser ersichtlich. Darin wird angegeben, wie die Daten in die Tabellen im Wissensspeicher fließen.

Das `knowledge_store`-Objekt stellt über die `{{storage-connection-string}}`-Postman-Variable eine Verbindung mit dem Speicherkonto her. `knowledge_store` enthält eine Reihe von Zuordnungen zwischen dem erweiterten Dokument und Tabellen und Spalten im Wissensspeicher. 

Wählen Sie zum Generieren des Skillsets in Postman die Schaltfläche **Send** (Senden) aus, um die PUT-Anforderung zu senden:

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

## <a name="create-the-indexer"></a>Erstellen des Indexers

Der letzte Schritt ist die Erstellung des Indexers. Der Indexer liest die Daten und aktiviert das Skillset. Wählen Sie in Postman die Anforderung **Create Indexer** (Indexer erstellen) aus, und sehen Sie sich den Anforderungstext an. Die Definition des Indexers verweist auf verschiedene andere Ressourcen, die Sie bereits erstellt haben: die Datenquelle, den Index und das Skillset. 

Das `parameters/configuration`-Objekt steuert die Erfassung der Daten durch den Indexer. In diesem Fall befinden sich die Eingabedaten in einem einzigen Dokument, das über eine Kopfzeile und durch Trennzeichen getrennte Werte verfügt. Der Dokumentschlüssel ist ein eindeutiger Bezeichner für das Dokument. Vor der Codierung ist der Dokumentschlüssel die URL des Quelldokuments. Schließlich werden die Ausgabewerte des Skillsets, z. B. Sprachcode, Stimmung und Schlüsselbegriffe, den entsprechenden Positionen im Dokument zugeordnet. Es ist zwar ein einzelner Wert für `Language` vorhanden, aber `Sentiment` wird auf jedes Element im Array `pages` angewendet. `Keyphrases` ist ein Array, das ebenfalls auf jedes Element im Array `pages` angewendet wird.

Nachdem Sie die Header `api-key` und `Content-type` festgelegt und sich vergewissert haben, dass der Text der Anforderung dem folgenden Code ähnelt, wählen Sie in Postman die Option **Send** (Senden). Postman sendet eine PUT-Anforderung an `https://{{search-service-name}}.search.windows.net/indexers/{{indexer-name}}?api-version={{api-version}}`. Azure Cognitive Search erstellt den Indexer und führt ihn aus. 

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

## <a name="run-the-indexer"></a>Ausführen des Indexers 

Navigieren Sie im Azure-Portal zur Seite **Übersicht** des Azure Cognitive Search-Diensts. Wählen Sie die Registerkarte **Indexer** und dann **hotels-reviews-ixr** aus. Wählen Sie **Ausführen**, falls der Indexer nicht bereits ausgeführt wurde. Unter Umständen werden für die Indizierungsaufgabe einige Warnungen in Bezug auf die Spracherkennung ausgelöst. Die Daten enthalten einige Überprüfungen in Sprachen, die von den kognitiven Skills noch nicht unterstützt werden. 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Daten mit Cognitive Services angereichert und die Ergebnisse in einen Wissensspeicher projiziert haben, können Sie Storage-Explorer oder Power BI verwenden, um Ihr angereichertes Dataset zu erkunden.

In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie diesen Wissensspeicher mit Storage-Explorer erkunden:

> [!div class="nextstepaction"]
> [Anzeigen mit Storage-Explorer](knowledge-store-view-storage-explorer.md)

In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie diesen Wissensspeicher mit Power BI verbinden:

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit Power BI](knowledge-store-connect-power-bi.md)

Wenn Sie diese Übung wiederholen oder eine andere exemplarische Vorgehensweise für die KI-Anreicherung ausprobieren möchten, löschen Sie den Indexer **hotel-reviews-idxr**. Durch das Löschen des Indexers wird der Zähler für kostenlose Transaktionen pro Tag auf Null zurückgesetzt.
