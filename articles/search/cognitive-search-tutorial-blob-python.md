---
title: 'Tutorial: Erstellen eines Skillsets in Python mit REST-APIs'
titleSuffix: Azure Cognitive Search
description: Detailliertes Beispiel für KI-basierte Datenextraktion, Verarbeitung natürlicher Sprache und Bildverarbeitung in Azure Cognitive Search unter Verwendung eines Jupyter Python-Notebooks. Extrahierte Daten werden indiziert und lassen sich einfach abfragen.
manager: nitinme
author: LisaLeib
ms.author: v-lilei
ms.service: cognitive-search
ms.devlang: python
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 1e404998c8f49852248a754e7134f439dcdf5b04
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74113681"
---
# <a name="tutorial-create-an-ai-enrichment-pipeline-using-rest-and-python"></a>Tutorial: Erstellen einer Pipeline für die KI-Anreicherung mit REST und Python

In diesem Tutorial werden die Fähigkeiten für die Programmierung von Datenanreicherungen in Azure Cognitive Search mithilfe von *kognitiven Qualifikationen* vermittelt. Qualifikationen werden durch die Verarbeitung von natürlicher Sprache und Bildanalysefunktionen in Cognitive Services unterstützt. Durch die Zusammenstellung und Konfiguration von Qualifikationsgruppen können Sie Text und Textdarstellungen eines Bilds oder einer gescannten Dokumentdatei extrahieren. Sie können außerdem Sprachen, Entitäten, Schlüsselbegriffe und mehr erkennen. Das Ergebnis sind umfangreiche zusätzliche Inhalte in einem Suchindex, die mit KI-Anreicherungen in einer Indizierungspipeline erstellt wurden. 

In diesem Tutorial verwenden Sie Python für die folgenden Aufgaben:

> [!div class="checklist"]
> * Erstellen einer Indizierungspipeline, die Beispieldaten auf dem Weg in einen Index anreichert
> * Anwenden von integrierten Qualifikationen: Entitätserkennung, Spracherkennung, Textbearbeitung und Schlüsselbegriffserkennung
> * Erfahren Sie, wie Qualifikationen miteinander verkettet werden, indem die Eingänge den Ausgängen in einer Qualifikationsgruppe zugeordnet werden
> * Ausführen von Anforderungen und Überprüfen von Ergebnissen
> * Zurücksetzen des Index und der Indexer für die weitere Entwicklung

Die Ausgabe ist ein für die Volltextsuche geeigneter Suchindex in Azure Cognitive Search. Sie können den Index mit weiteren Standardfunktionen erweitern, wie etwa [Synonymen](search-synonyms.md), [Bewertungsprofilen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [Analysetools](search-analyzers.md) und [Filtern](search-filters.md). 

In diesem Tutorial wird der kostenlose Dienst verwendet. Die Anzahl kostenloser Transaktionen ist allerdings auf 20 Dokumente pro Tag beschränkt. Falls Sie dieses Tutorial mehrmals am gleichen Tag ausführen möchten, verwenden Sie einen kleineren Dateisatz, um mehr Ausführungen zu ermöglichen.

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie [eine kostenpflichtige Cognitive Services-Ressource anfügen](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumententschlüsselungsphase in Azure Cognitive Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion sind in der [Preisübersicht für Azure Cognitive Search](https://go.microsoft.com/fwlink/?linkid=2042400) angegeben.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial werden die folgenden Dienste, Tools und Daten verwendet. 

+ [Erstellen Sie ein Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) zum Speichern der Beispieldaten. Stellen Sie sicher, dass sich das Speicherkonto in der gleichen Region befindet wie Azure Cognitive Search.

+ [Anaconda 3.x](https://www.anaconda.com/distribution/#download-section) mit Python 3.x und Jupyter Notebooks.

+ Die [Beispieldaten](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) bestehen aus einem kleinen Satz Dateien verschiedenen Typs. 

+ [Erstellen Sie einen Azure Cognitive Search-Dienst](search-create-service-portal.md), oder [suchen Sie nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in Ihrem aktuellen Abonnement. In diesem Tutorial können Sie einen kostenlosen Dienst verwenden.

## <a name="get-a-key-and-url"></a>Abrufen eines Schlüssels und einer URL

Für die Interaktion mit dem Azure Cognitive Search-Dienst benötigen Sie die Dienst-URL und einen Zugriffsschlüssel. Ein Suchdienst wird mit beidem erstellt. Gehen Sie daher wie folgt vor, um die erforderlichen Informationen zu erhalten, falls Sie Azure Cognitive Search Ihrem Abonnement hinzugefügt haben:

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com/), und rufen Sie auf der Seite **Übersicht** Ihres Suchdiensts die URL ab. Ein Beispiel für einen Endpunkt ist `https://mydemo.search.windows.net`.

1. Rufen Sie unter **Einstellungen** > **Schlüssel** einen Administratorschlüssel ab, um Vollzugriff auf den Dienst zu erhalten. Es gibt zwei austauschbare Administratorschlüssel – diese wurden zum Zweck der Geschäftskontinuität bereitgestellt, falls Sie einen Rollover für einen Schlüssel durchführen müssen. Für Anforderungen zum Hinzufügen, Ändern und Löschen von Objekten können Sie den primären oder den sekundären Schlüssel verwenden.

![Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels](media/search-get-started-postman/get-url-key.png "Abrufen eines HTTP-Endpunkts und eines Zugriffsschlüssels")

Für alle an Ihren Dienst gesendeten Anforderungen ist ein API-Schlüssel erforderlich. Ein gültiger Schlüssel stellt anforderungsbasiert eine Vertrauensstellung her zwischen der Anwendung, die die Anforderung sendet, und dem Dienst, der sie verarbeitet.

## <a name="prepare-sample-data"></a>Vorbereiten der Beispieldaten

Die Anreicherungspipeline lädt per Pull aus Azure-Datenquellen herunter. Quelldaten müssen von einem unterstützten Datenquellentyp eines [Azure Cognitive Search-Indexers](search-indexer-overview.md) stammen. Für diese Übung verwenden wir Blobspeicher, um verschiedene Inhaltstypen anschaulich vorzustellen.

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com), navigieren Sie zu Ihrem Azure-Speicherkonto, klicken Sie auf **BLOBs** und dann auf **+ Container**.

1. [Erstellen Sie einen Blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) für die Beispieldaten. Sie können die öffentliche Zugriffsebene auf beliebige gültige Werte festlegen.

1. Öffnen Sie den Container nach der Erstellung, und wählen Sie in der Befehlsleiste die Option **Hochladen**, um die Beispieldateien hochzuladen, die Sie in einem vorherigen Schritt heruntergeladen haben.

   ![Quelldateien in Azure Blob Storage](./media/cognitive-search-quickstart-blob/sample-data.png)

1. Nachdem die Beispieldateien geladen wurden, rufen Sie den Containernamen und eine Verbindungszeichenfolge für Ihren Blobspeicher ab. Dazu können Sie im Azure-Portal zu Ihrem Speicherkonto navigieren. Klicken Sie auf **Zugriffsschlüssel**, und kopieren Sie dann das Feld **Verbindungszeichenfolge**.

Die Verbindungszeichenfolge hat folgendes Format: `DefaultEndpointsProtocol=https;AccountName=<YOUR-STORAGE-ACCOUNT-NAME>;AccountKey=<YOUR-STORAGE-ACCOUNT-KEY>;EndpointSuffix=core.windows.net`

Halten Sie die Verbindungszeichenfolge bereit. Sie benötigen sie in einem späteren Schritt.

Es gibt andere Möglichkeiten zum Angeben der Verbindungszeichenfolge, etwa das Bereitstellen einer Shared Access Signature (SAS). Weitere Informationen über Anmeldeinformationen für Datenquellen finden Sie unter [Indizieren von Azure Blob Storage](search-howto-indexing-azure-blob-storage.md#Credentials).

## <a name="create-a-jupyter-notebook"></a>Erstellen eines Jupyter Notebooks

> [!Note]
> In diesem Artikel wird beschrieben, wie Sie eine Datenquelle, einen Index, einen Indexer und eine Qualifikationsgruppe mithilfe einer Reihe von Python-Skripts erstellen. Das vollständige Notebook-Beispiel können Sie unter [azure-search-python-samples repository](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/Tutorial-AI-Enrichment-Jupyter-Notebook) herunterladen.

Verwenden Sie Anaconda Navigator zum Starten von Jupyter Notebook, und erstellen Sie ein neues Python 3-Notebook.

## <a name="connect-to-azure-cognitive-search"></a>Herstellen einer Verbindung mit Azure Cognitive Search

Führen Sie in Ihrem Notebook das folgende Skript aus, um die Bibliotheken zum Verwenden von JSON und Formulieren von HTTP-Anforderungen zu laden.

```python
import json
import requests
from pprint import pprint
```

Definieren Sie nun die Namen für die Datenquelle, den Index, den Indexer und die Qualifikationsgruppe. Führen Sie das folgende Skript aus, um die Namen für dieses Tutorial einzurichten.

```python
# Define the names for the data source, skillset, index and indexer
datasource_name = "cogsrch-py-datasource"
skillset_name = "cogsrch-py-skillset"
index_name = "cogsrch-py-index"
indexer_name = "cogsrch-py-indexer"
```

> [!Tip]
> Ein kostenloser Dienst ist auf drei Indizes, Indexer und Datenquellen beschränkt. In diesem Tutorial wird davon jeweils eine Instanz erstellt. Stellen Sie sicher, dass Sie Platz zum Erstellen neuer Objekte haben, bevor wir fortfahren.

Ersetzen Sie im folgenden Skript die Platzhalter für Ihren Suchdienst (YOUR-SEARCH-SERVICE-NAME) und Ihren Admin-API-Schlüssel (YOUR-ADMIN-API-KEY). Führen Sie dann das Skript aus, um den Suchdienstendpunkt einzurichten.

```python
# Setup the endpoint
endpoint = 'https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/'
headers = {'Content-Type': 'application/json',
           'api-key': '<YOUR-ADMIN-API-KEY>'}
params = {
    'api-version': '2019-05-06'
}
```

## <a name="create-a-data-source"></a>Erstellen einer Datenquelle

Jetzt, da Ihre Dienste und Datenquellen vorbereitet sind, beginnen Sie damit, die Komponenten Ihrer Indizierungspipeline zusammenzustellen. Beginnen Sie mit einem Datenquellenobjekt, das Azure Cognitive Search darüber informiert, wie die externen Quelldaten abgerufen werden.

Ersetzen Sie im folgenden Skript den Platzhalter YOUR-BLOB-RESOURCE-CONNECTION-STRING durch die Verbindungszeichenfolge für das im vorherigen Schritt erstellte Blob. Führen Sie dann das Skript aus, um eine Datenquelle mit dem Namen `cogsrch-py-datasource` zu erstellen.

```python
# Create a data source
datasourceConnectionString = "<YOUR-BLOB-RESOURCE-CONNECTION-STRING>"
datasource_payload = {
    "name": datasource_name,
    "description": "Demo files to demonstrate cognitive search capabilities.",
    "type": "azureblob",
    "credentials": {
        "connectionString": datasourceConnectionString
    },
    "container": {
        "name": "basic-demo-data-pr"
    }
}
r = requests.put(endpoint + "/datasources/" + datasource_name,
                 data=json.dumps(datasource_payload), headers=headers, params=params)
print(r.status_code)
```

Die Anforderung sollte die erfolgreiche Ausführung durch Rückgabe von Statuscode 201 bestätigen.

Überprüfen Sie im Azure-Portal auf der Dashboardseite des Suchdiensts, ob „cogsrch-py-datasource“ in der Liste **Datenquellen** aufgeführt ist. Klicken Sie auf **Aktualisieren**, um die Seite zu aktualisieren.

![Datenquellenkachel im Portal](./media/cognitive-search-tutorial-blob-python/py-data-source-tile.png "Datenquellenkachel im Portal")

## <a name="create-a-skillset"></a>Erstellen eines Skillsets

In diesem Schritt definieren Sie eine Reihe von Anreicherungsschritten, die auf Ihre Daten angewandt werden sollen. Jeder Anreicherungsschritt wird als *Qualifikation* und der Satz von Anreicherungsschritten als *Qualifikationsgruppe* bezeichnet. In diesem Tutorial werden für die Qualifikationsgruppe [integrierte kognitive Qualifikationen](cognitive-search-predefined-skills.md) verwendet:

+ [Spracherkennung](cognitive-search-skill-language-detection.md), um die Sprache der Inhalte zu bestimmen.

+ [Text unterteilen](cognitive-search-skill-textsplit.md), um große Inhalte vor dem Aufrufen der Schlüsselbegriffserkennungs-Qualifikation in kleinere Stücke aufzuteilen. Die Schlüsselbegriffserkennung akzeptiert Eingaben von 50.000 Zeichen oder weniger. Für einige der Beispieldateien ist eine Aufteilung erforderlich, um diesen Grenzwert zu erfüllen.

+ [Entitätserkennung](cognitive-search-skill-entity-recognition.md), um die Namen von Organisationen aus Inhalten im Blobcontainer zu extrahieren.

+ [Schlüsselbegriffserkennung](cognitive-search-skill-keyphrases.md), um die wichtigsten Schlüsselbegriffe herauszuziehen. 

### <a name="python-script"></a>Python-Skript
Führen Sie das folgende Skript aus, um eine Qualifikationsgruppe mit dem Namen `cogsrch-py-skillset` zu erstellen.

```python
# Create a skillset
skillset_payload = {
    "name": skillset_name,
    "description":
    "Extract entities, detect language and extract key-phrases",
    "skills":
    [
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": ["Organization"],
            "defaultLanguageCode": "en",
            "inputs": [
                {
                    "name": "text", "source": "/document/content"
                }
            ],
            "outputs": [
                {
                    "name": "organizations", "targetName": "organizations"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
            "inputs": [
                {
                    "name": "text", "source": "/document/content"
                }
            ],
            "outputs": [
                {
                    "name": "languageCode",
                    "targetName": "languageCode"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
            "textSplitMode": "pages",
            "maximumPageLength": 4000,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/content"
                },
                {
                    "name": "languageCode",
                    "source": "/document/languageCode"
                }
            ],
            "outputs": [
                {
                    "name": "textItems",
                    "targetName": "pages"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
            "context": "/document/pages/*",
            "inputs": [
                {
                    "name": "text", "source": "/document/pages/*"
                },
                {
                    "name": "languageCode", "source": "/document/languageCode"
                }
            ],
            "outputs": [
                {
                    "name": "keyPhrases",
                    "targetName": "keyPhrases"
                }
            ]
        }
    ]
}

r = requests.put(endpoint + "/skillsets/" + skillset_name,
                 data=json.dumps(skillset_payload), headers=headers, params=params)
print(r.status_code)
```

Die Anforderung sollte die erfolgreiche Ausführung durch Rückgabe von Statuscode 201 bestätigen.

Die Qualifikation zur Schlüsselbegriffserkennung wird für die einzelnen Seiten angewandt. Durch Festlegen des Kontexts auf `"document/pages/*"` führen Sie diesen Anreicherungsschritt für jedes Element des Dokument-/Seitenarrays (für jede Seite im Dokument) aus.

Jede Qualifikation wird auf den Inhalten des Dokuments ausgeführt. Während der Verarbeitung entschlüsselt Azure Cognitive Search jedes Dokument, um die Inhalte aus verschiedenen Dateiformaten zu lesen. In der Quelldatei gefundener Text wird für jedes Dokument jeweils in einem `content`-Feld gespeichert. Legen Sie die Eingabe daher auf `"/document/content"` fest.

Unten finden Sie eine grafische Darstellung der Qualifikationsgruppe.

![Verstehen eines Skillsets](media/cognitive-search-tutorial-blob/skillset.png "Verstehen eines Skillsets")

Ausgaben können einem Index zugeordnet, als Eingabe einer Downstream-Qualifikation verwendet oder in beider Weise zugleich eingesetzt werden, wie etwa bei Sprachcode. Im Index ist ein Sprachcode zu Filterungszwecken nützlich. Als Eingabe wird ein Sprachcode von Qualifikationen zur Textanalyse verwendet, um die Linguistikregeln über Wörtertrennung zu informieren.

Weitere Informationen zu den Grundlagen von Qualifikationsgruppen finden Sie unter [How to define a skillset](cognitive-search-defining-skillset.md) (Definieren von Qualifikationsgruppen).

## <a name="create-an-index"></a>Erstellen eines Index

In diesem Abschnitt definieren Sie das Indexschema, indem Sie die Felder angeben, die in den durchsuchbaren Index aufgenommen werden, und die Suchattribute für die einzelnen Felder festlegen. Felder besitzen einen Typ und können Attribute annehmen, die bestimmen, wie das Feld verwendet wird (durchsuchbar, sortierbar usw.). Feldname in einem Index müssen nicht exakt mit den Feldnamen in der Quelle übereinstimmen. In einem späteren Schritt fügen Sie in einem Indexer Feldzuordnungen hinzu, um die Quell- und Zielfelder zu verbinden. Definieren Sie für diesen Schritt den Index mit Feldbenennungskonventionen, die für Ihre Suchanwendung angemessen sind.

In dieser Übung werden die folgenden Felder und Feldtypen verwendet:

| Feldnamen: | id         | Inhalt   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |

Führen Sie das folgende Skript aus, um den Index `cogsrch-py-index` zu erstellen.

```python
# Create an index
index_payload = {
    "name": index_name,
    "fields": [
        {
            "name": "id",
            "type": "Edm.String",
            "key": "true",
            "searchable": "true",
            "filterable": "false",
            "facetable": "false",
            "sortable": "true"
        },
        {
            "name": "content",
            "type": "Edm.String",
            "sortable": "false",
            "searchable": "true",
            "filterable": "false",
            "facetable": "false"
        },
        {
            "name": "languageCode",
            "type": "Edm.String",
            "searchable": "true",
            "filterable": "false",
            "facetable": "false"
        },
        {
            "name": "keyPhrases",
            "type": "Collection(Edm.String)",
            "searchable": "true",
            "filterable": "false",
            "facetable": "false"
        },
        {
            "name": "organizations",
            "type": "Collection(Edm.String)",
            "searchable": "true",
            "sortable": "false",
            "filterable": "false",
            "facetable": "false"
        }
    ]
}

r = requests.put(endpoint + "/indexes/" + index_name,
                 data=json.dumps(index_payload), headers=headers, params=params)
print(r.status_code)
```

Die Anforderung sollte die erfolgreiche Ausführung durch Rückgabe von Statuscode 201 bestätigen.

Weitere Informationen zum Definieren eines Index finden Sie unter [Index erstellen (Azure Cognitive Search-REST-API)](https://docs.microsoft.com/rest/api/searchservice/create-index).

## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Erstellen eines Indexers, Zuordnen von Feldern und Ausführen von Transformationen

Bisher haben Sie eine Datenquelle, eine Qualifikationsgruppe und einen Index erstellt. Diese drei Komponenten werden Teil eines [Indexers](search-indexer-overview.md), der jedes einzelne Stück per Pull in einen einzelnen mehrstufigen Vorgang herunterlädt. Um diese Objekte in einem Indexer zusammenzuführen, müssen Sie Feldzuordnungen definieren.

+ fieldMappings werden vor der Qualifikationsgruppe verarbeitet, und die Quellfelder der Datenquelle werden Zielfeldern in einem Index zugeordnet. Wenn die Feldnamen und -typen auf beiden Seiten gleich sind, ist keine Zuordnung erforderlich.

+ outputFieldMappings werden nach der Qualifikationsgruppe verarbeitet. Es wird auf nicht vorhandene sourceFieldNames verwiesen, bis diese per Dokumententschlüsselung oder Anreicherung erstellt werden. targetFieldName ist ein Feld in einem Index.

Neben dem Verknüpfen von Ein- und Ausgaben können Sie auch Feldzuordnungen nutzen, um Datenstrukturen zu vereinfachen. Weitere Informationen finden Sie unter [Zuordnen angereicherter Felder zu einem durchsuchbaren Index](cognitive-search-output-field-mapping.md).

Führen Sie das folgende Skript aus, um einen Indexer mit dem Namen `cogsrch-py-indexer` zu erstellen.

```python
# Create an indexer
indexer_payload = {
    "name": indexer_name,
    "dataSourceName": datasource_name,
    "targetIndexName": index_name,
    "skillsetName": skillset_name,
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction":
            {"name": "base64Encode"}
        },
        {
            "sourceFieldName": "content",
            "targetFieldName": "content"
        }
    ],
    "outputFieldMappings":
    [
        {
            "sourceFieldName": "/document/organizations",
            "targetFieldName": "organizations"
        },
        {
            "sourceFieldName": "/document/pages/*/keyPhrases/*",
            "targetFieldName": "keyPhrases"
        },
        {
            "sourceFieldName": "/document/languageCode",
            "targetFieldName": "languageCode"
        }
    ],
    "parameters":
    {
        "maxFailedItems": -1,
        "maxFailedItemsPerBatch": -1,
        "configuration":
        {
            "dataToExtract": "contentAndMetadata",
            "imageAction": "generateNormalizedImages"
        }
    }
}

r = requests.put(endpoint + "/indexers/" + indexer_name,
                 data=json.dumps(indexer_payload), headers=headers, params=params)
print(r.status_code)
```

Die Anforderung sollte schnell Statuscode 201 zurückgeben, die Verarbeitung kann jedoch einige Minuten dauern. Das Dataset ist zwar klein, aber Analysequalifikationen, z. B. Bildanalyse, sind rechenintensiv und nehmen Zeit in Anspruch.

Verwenden Sie das Skript zum [Überprüfen des Indexerstatus](#check-indexer-status) im nächsten Abschnitt, um zu ermitteln, wann der Indexerprozess abgeschlossen ist.

> [!TIP]
> Die Pipeline wird durch Erstellen eines Indexers aufgerufen. Wenn beim Zugriff auf die Daten, dem Zuordnen von Ein- und Ausgaben oder der Reihenfolge der Vorgänge Probleme auftreten, äußern sie sich in dieser Phase. Um die Pipeline mit Code- oder Skriptänderungen auszuführen, müssen Sie möglicherweise zuerst Objekte löschen. Weitere Informationen finden Sie unter [Reset and re-run](#reset) (Zurücksetzen und erneut Ausführen).

#### <a name="explore-the-request-body"></a>Untersuchen des Anforderungstexts

Das Skript legt `"maxFailedItems"` auf -1 fest, was die Indexengine anweist, Fehler beim Datenimport zu ignorieren. Dies ist nützlich, weil die Demodatenquelle nur wenige Dokumente enthält. Für eine größere Datenquelle sollten Sie den Wert größer als 0 festlegen.

Achten Sie außerdem auf die Anweisung `"dataToExtract":"contentAndMetadata"` in den Konfigurationsparametern. Diese Anweisung weist den Indexer an, die Inhalte aus verschiedenen Dateiformaten und die den einzelnen Dateien zugeordneten Metadaten zu extrahieren.

Wenn die Inhalte extrahiert werden, können Sie `imageAction` darauf festlegen, Text aus in der Datenquelle gefundenen Bildern zu extrahieren. Die Konfiguration `"imageAction":"generateNormalizedImages"` in Kombination mit der OCR- und der Textzusammenführungsqualifikation weist den Indexer an, Text aus den Bildern zu extrahieren (beispielsweise das Wort „Stop“ aus einem Stoppschild) und ihn als Teil des Inhaltsfelds einzubetten. Dieses Verhalten betrifft sowohl die in den Dokumenten eingebetteten Bilder (denken Sie etwa an Bilder in PDF-Dateien) als auch die in der Datenquelle gefundenen Bilder, z. B. eine JPG-Datei.

<a name="check-indexer-status"></a>

## <a name="check-indexer-status"></a>Überprüfen des Indexerstatus

Nachdem der Indexer definiert wurde, wird er automatisch ausgeführt, wenn Sie die Anforderung senden. Abhängig von den kognitiven Qualifikationen, die Sie definiert haben, kann die Indizierung länger als erwartet dauern. Führen Sie das folgende Skript aus, um herauszufinden, wann die Indexerverarbeitung abgeschlossen ist.

```python
# Get indexer status
r = requests.get(endpoint + "/indexers/" + indexer_name +
                 "/status", headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

Überprüfen Sie „lastResult“ auf die Werte für „status“ und „endTime“ in der Antwort. Führen Sie das Skript regelmäßig aus, um den Status zu überprüfen. Wenn der Indexer abgeschlossen wurde, wird der Status auf „success“ festlegt, ein Wert für „endTime“ wird angegeben, und die Antwort enthält Fehler und Warnungen, die bei der Anreicherung aufgetreten sind.

![Indexererstellung](./media/cognitive-search-tutorial-blob-python/py-indexer-is-created.png "Indexererstellung")

Warnungen sind bei bestimmten Kombinationen aus Quelldatei und Qualifikation häufig und weisen nicht immer auf ein Problem hin. In diesem Tutorial sind die Warnungen unbedenklich. Beispielsweise wird für eine der JPEG-Dateien, die keinen Text enthält, die Warnung im folgenden Screenshot angegeben.

![Exemplarische Indexerwarnung](./media/cognitive-search-tutorial-blob-python/py-indexer-warning-example.png "Exemplarische Indexerwarnung")

## <a name="query-your-index"></a>Abfragen Ihres Index

Führen Sie nach dem Abschluss der Indizierung Abfragen aus, die die Inhalte einzelner Felder zurückgeben. Standardmäßig gibt Azure Cognitive Search die 50 besten Ergebnisse zurück. Die Beispieldaten sind klein, so dass die Standardeinstellung gut funktioniert. Beim Arbeiten mit größeren Datensets müssen Sie jedoch möglicherweise Parameter in die Abfragezeichenfolge aufnehmen, um mehr Ergebnisse zurückzugeben. Eine entsprechende Anleitung finden Sie unter [Arbeiten mit Suchergebnissen in Azure Search](search-pagination-page-layout.md).

Fragen Sie als Überprüfungsschritt den Index nach allen Feldern ab.

```python
# Query the index for all fields
r = requests.get(endpoint + "/indexes/" + index_name,
                 headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

Die Ergebnisse sollten in etwa dem folgenden Beispiel entsprechen. Im Screenshot wird nur ein Teil der Antwort angezeigt.

![Abfragen des Index: alle Felder](./media/cognitive-search-tutorial-blob-python/py-query-index-for-fields.png "Abfragen des Index: alle Felder")

Die Ausgabe ist das Indexschema mit dem Namen, dem Typ und den Attributen für jedes Feld.

Senden Sie eine zweite Abfrage nach `"*"`, um alle Inhalte eines einzelnen Felds zurückzugeben, z.B. `organizations`.

```python
# Query the index to return the contents of organizations
r = requests.get(endpoint + "/indexes/" + index_name +
                 "/docs?&search=*&$select=organizations", headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

Die Ergebnisse sollten in etwa dem folgenden Beispiel entsprechen. Im Screenshot wird nur ein Teil der Antwort angezeigt.

![Abfragen des Index: Inhalte von Organisationen](./media/cognitive-search-tutorial-blob-python/py-query-index-for-organizations.png "Abfragen des Indexes, um die Inhalte von Organisationen zurückzugeben")

Wiederholen Sie das Verfahren in dieser Übung für weitere Felder: „content“, „languageCode“, „keyPhrases“ und „organizations“. Mithilfe von `$select` können Sie unter Einsatz einer durch Trennzeichen getrennten Liste mehrere Felder zurückgeben.

Sie können GET oder POST verwenden, abhängig von der Komplexität und Länge der Abfragezeichenfolge. Weitere Informationen finden Sie unter [Abfragen mithilfe der REST-API](https://docs.microsoft.com/rest/api/searchservice/search-documents).

<a name="reset"></a>

## <a name="reset-and-rerun"></a>Zurücksetzen und erneut ausführen

In den frühen experimentellen Phasen der Pipelineentwicklung besteht der praktikabelste Ansatz für den Übergang von einer Entwurfsphase zur nächsten darin, die Objekte aus Azure Cognitive Search zu löschen und Ihrem Code zu erlauben, sie neu zu erstellen. Ressourcennamen sind eindeutig. Wenn Sie ein Objekt löschen, können Sie es unter dem gleichen Namen neu erstellen.

So indizieren Sie Ihre Dokumente mit den neuen Definitionen erneut:

1. Löschen Sie den Index, um persistente Daten zu entfernen. Löschen Sie den Indexer, um ihn für Ihren Dienst neu zu erstellen.
2. Ändern Sie die Qualifikationsgruppe und die Indexdefinitionen.
3. Führen Sie eine Neuerstellung eines Index und des Indexers für den Dienst aus, um die Pipeline auszuführen.

Sie können das Portal verwenden, um Indizes, Indexer und Qualifikationsgruppen zu löschen. Wenn Sie den Indexer löschen, können Sie optional auch den Index, die Qualifikationsgruppen und die Datenquelle löschen.

![Löschen von Suchobjekten](./media/cognitive-search-tutorial-blob-python/py-delete-indexer-delete-all.png "Löschen von Suchobjekten im Portal")

Sie können sie auch mithilfe eines Skripts löschen. Mit dem folgenden Skript wird die zuvor erstellte Qualifikationsgruppe gelöscht. Sie können die Anforderung mühelos so ändern, dass der Index, der Indexer oder die Datenquelle gelöscht wird.

```python
# delete the skillset
r = requests.delete(endpoint + "/skillsets/" + skillset_name,
                    headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

In dem Maß, da Ihr Code reift, kann es sinnvoll sein, die Neuerstellungsstrategie neu zu definieren. Weitere Informationen finden  Sie unter [Neuerstellen eines Indexes](search-howto-reindex.md).

## <a name="takeaways"></a>Wesentliche Punkte

Dieses Tutorial veranschaulicht die grundlegenden Schritte beim Erstellen einer erweiterten Indizierungspipeline durch Erstellung von Komponenten: eine Datenquelle, eine Qualifikationsgruppe, ein Index und ein Indexer.

Es wurden [integrierte Qualifikationen](cognitive-search-predefined-skills.md) sowie Skillsetdefinitionen und eine Möglichkeit zur Verkettung von Qualifikationen mithilfe von Ein- und Ausgaben vorgestellt. Sie haben darüber hinaus erfahren, dass `outputFieldMappings` in der Indexerdefinition erforderlich ist, um angereicherte Werte aus der Pipeline an einen durchsuchbaren Index in einem Azure Cognitive Search-Dienst weiterzuleiten.

Ferner haben Sie erfahren, wie die Ergebnisse getestet werden und das System für weitere Entwicklungsschritte zurückgesetzt wird. Sie haben gelernt, dass das Ausgeben von Abfragen auf den Index die von der angereicherten Indizierungspipeline erstellte Ausgabe zurückgibt. In dieser Version ist ein Mechanismus zum Anzeigen von internen Konstrukten (vom System erstellten angereicherten Dokumenten) verfügbar. Darüber hinaus haben Sie erfahren, wie der Indexerstatus überprüft wird und welche Objekte vor der erneuten Ausführung einer Pipeline gelöscht werden müssen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Die schnellste Möglichkeit zur Bereinigung des Systems nach einem Tutorial besteht im Löschen der Ressourcengruppe, die den Azure Cognitive Search-Dienst und den Azure Blob-Dienst enthält. Unter der Annahme, dass Sie beide Dienste in der gleichen Gruppe platziert haben, löschen Sie einfach die Ressourcengruppe, um endgültig ihren gesamten Inhalt zu löschen, einschließlich der Dienste und aller gespeicherten Inhalte, die Sie für dieses Tutorial erstellt haben. Im Portal finden Sie den Namen der Ressourcengruppe auf der Seite „Übersicht“ der einzelnen Dienste.

## <a name="next-steps"></a>Nächste Schritte

Anpassen oder Erweitern der Pipeline mit benutzerdefinierten Qualifikationen. Das Erstellen einer benutzerdefinierten Qualifikation die Sie einer Qualifikationsgruppe hinzufügen, ermöglicht Ihnen, eigene, von Ihnen selbst erstellte Text- oder Bildanalysen einzubeziehen.

> [!div class="nextstepaction"]
> [Beispiel: Erstellen eines benutzerdefinierten Skills für die KI-Anreicherung](cognitive-search-create-custom-skill-example.md)
