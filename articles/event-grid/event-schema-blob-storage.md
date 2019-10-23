---
title: Azure Event Grid-Ereignisschema für Blob Storage
description: Beschreibt die Eigenschaften, die mit Azure Event Grid für Blob Storage-Ereignisse bereitgestellt werden.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: reference
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 0ab81d3c1d4c68827cf1569bf4a22c3311fe355d
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72555824"
---
# <a name="azure-event-grid-event-schema-for-blob-storage"></a>Azure Event Grid-Ereignisschema für Blob Storage

In diesem Artikel werden die Eigenschaften und das Schema für Blob Storage-Ereignisse beschrieben. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md).

Eine Liste von Beispielskripts und Tutorials finden Sie unter [Ereignisquellen in Azure Event Grid](event-sources.md#storage) im Abschnitt „Speicher“.

## <a name="list-of-events-for-blob-rest-apis"></a>Ereignisliste für Blob-REST-APIs

Die folgenden Ereignisse werden ausgelöst, wenn ein Client durch Aufrufen von Blob-REST-APIs ein Blob erstellt, ersetzt oder löscht:

 |Ereignisname |BESCHREIBUNG|
 |----------|-----------|
 |**Microsoft.Storage.BlobCreated** |Wird ausgelöst, wenn ein Blob erstellt oder ersetzt wird. <br>Dieses Ereignis wird insbesondere ausgelöst, wenn Clients den Vorgang `PutBlob`, `PutBlockList` oder `CopyBlob` verwenden, die in der Blob-REST-API zur Verfügung stehen.   |
 |**Microsoft.Storage.BlobDeleted** |Wird ausgelöst, wenn ein Blob gelöscht wird. <br>Dieses Ereignis wird insbesondere ausgelöst, wenn Clients den Vorgang `DeleteBlob` aufrufen, der in der Blob-REST-API zur Verfügung steht. |

> [!NOTE]
> Wenn Sie sicherstellen möchten, dass das Ereignis **Microsoft.Storage.BlobCreated** erst ausgelöst wird, nachdem ein Blockblob vollständig committet wurde, filtern Sie das Ereignis nach den REST-API-Aufrufen `CopyBlob`, `PutBlob` und `PutBlockList`. Bei diesen API-Aufrufen wird das Ereignis **Microsoft.Storage.BlobCreated** erst ausgelöst, nachdem Daten vollständig in einem Blockblob committet wurden. Informationen zum Erstellen eines Filters finden Sie unter [Filtern von Ereignissen für Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

## <a name="list-of-the-events-for-azure-data-lake-storage-gen-2-rest-apis"></a>Ereignisliste für REST-APIs von Azure Data Lake Storage Gen2

Die folgenden Ereignisse werden ausgelöst, wenn Sie einen hierarchischen Namespace für das Speicherkonto aktivieren und Clients REST-APIs von Azure Data Lake Storage Gen2 aufrufen.

|Ereignisname|BESCHREIBUNG|
|----------|-----------|
|**Microsoft.Storage.BlobCreated** | Wird ausgelöst, wenn ein Blob erstellt oder ersetzt wird. <br>Dieses Ereignis wird insbesondere ausgelöst, wenn Clients die Vorgänge `CreateFile` und `FlushWithClose` verwenden, die in der REST-API von Azure Data Lake Storage Gen2 zur Verfügung stehen. |
|**Microsoft.Storage.BlobDeleted** |Wird ausgelöst, wenn ein Blob gelöscht wird. <br>Dieses Ereignis wird insbesondere auch ausgelöst, wenn Clients den Vorgang `DeleteFile` aufrufen, der in der REST-API von Azure Data Lake Storage Gen2 zur Verfügung steht. |
|**Microsoft.Storage.BlobRenamed**|Wird ausgelöst, wenn ein Blob umbenannt wird. <br>Dieses Ereignis wird insbesondere ausgelöst, wenn Clients den Vorgang `RenameFile` verwenden, der in der REST-API von Azure Data Lake Storage Gen2 zur Verfügung steht.|
|**Microsoft.Storage.DirectoryCreated**|Wird ausgelöst, wenn ein Verzeichnis erstellt wird. <br>Dieses Ereignis wird insbesondere ausgelöst, wenn Clients den Vorgang `CreateDirectory` verwenden, der in der REST-API von Azure Data Lake Storage Gen2 zur Verfügung steht.|
|**Microsoft.Storage.DirectoryRenamed**|Wird ausgelöst, wenn ein Verzeichnis umbenannt wird. <br>Dieses Ereignis wird insbesondere ausgelöst, wenn Clients den Vorgang `RenameDirectory` verwenden, der in der REST-API von Azure Data Lake Storage Gen2 zur Verfügung steht.|
|**Microsoft.Storage.DirectoryDeleted**|Wird ausgelöst, wenn ein Verzeichnis gelöscht wird. <br>Dieses Ereignis wird insbesondere ausgelöst, wenn Clients den Vorgang `DeleteDirectory` verwenden, der in der REST-API von Azure Data Lake Storage Gen2 zur Verfügung steht.|

> [!NOTE]
> Wenn Sie sicherstellen möchten, dass das Ereignis **Microsoft.Storage.BlobCreated** erst ausgelöst wird, nachdem ein Blockblob vollständig committet wurde, filtern Sie das Ereignis nach dem REST-API-Aufruf `FlushWithClose`. Bei diesem API-Aufruf wird das Ereignis **Microsoft.Storage.BlobCreated** erst ausgelöst, nachdem Daten vollständig in einem Blockblob committet wurden. Informationen zum Erstellen eines Filters finden Sie unter [Filtern von Ereignissen für Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

<a id="example-event" />

## <a name="the-contents-of-an-event-response"></a>Der Inhalt einer Ereignisantwort

Wenn ein Ereignis ausgelöst wird, sendet der Event Grid-Dienst Daten zum Ereignis an den Endpunkt, der über ein entsprechendes Abonnement verfügt.

In diesem Abschnitt wird anhand eines Beispiels gezeigt, wie diese Daten für ein Blob Storage-Ereignis aussehen.

### <a name="microsoftstorageblobcreated-event"></a>Ereignis „Microsoft.Storage.BlobCreated“

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/test-container/blobs/new-file.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/new-file.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobcreated-event-data-lake-storage-gen2"></a>Ereignis „Microsoft.Storage.BlobCreated“ (Data Lake Storage Gen2)

Wenn das Blob Storage-Konto über einen hierarchischen Namespace verfügt, sehen die Daten in etwa wie im vorherigen Beispiel aus – mit Ausnahme folgender Änderungen:

* Der Schlüssel `dataVersion` wird auf den Wert `2` festgelegt.

* Der Schlüssel `data.api` wird auf die Zeichenfolge `CreateFile` oder `FlushWithClose` festgelegt.

* Der Schlüssel `contentOffset` wird in das Dataset eingeschlossen.

> [!NOTE]
> Wenn Anwendungen den Vorgang `PutBlockList` verwenden, um ein neues Blob in das Konto hochzuladen, sind diese Änderungen nicht in den Daten enthalten.

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/new-file.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 0,
    "contentOffset": 0,
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/new-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "2",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobdeleted-event"></a>Ereignis „Microsoft.Storage.BlobDeleted“

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/testcontainer/blobs/file-to-delete.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/file-to-delete.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobdeleted-event-data-lake-storage-gen2"></a>Ereignis „Microsoft.Storage.BlobDeleted“ (Data Lake Storage Gen2)

Wenn das Blob Storage-Konto über einen hierarchischen Namespace verfügt, sehen die Daten in etwa wie im vorherigen Beispiel aus – mit Ausnahme folgender Änderungen:

* Der Schlüssel `dataVersion` wird auf den Wert `2` festgelegt.

* Der Schlüssel `data.api` wird auf die Zeichenfolge `DeleteFile` festgelegt.

* Die Schlüssel `url` enthält den Pfad `dfs.core.windows.net`.

> [!NOTE]
> Wenn Anwendungen den Vorgang `DeleteBlob` verwenden, um ein Blob aus dem Konto zu löschen, sind diese Änderungen nicht in den Daten enthalten.

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/file-to-delete.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
    "api": "DeleteFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/file-to-delete.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "2",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobrenamed-event"></a>Ereignis „Microsoft.Storage.BlobRenamed“

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-file.txt",
  "eventType": "Microsoft.Storage.BlobRenamed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-file.txt",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectorycreated-event"></a>Ereignis „Microsoft.Storage.DirectoryCreated“

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-new-directory",
  "eventType": "Microsoft.Storage.DirectoryCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-new-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectoryrenamed-event"></a>Ereignis „Microsoft.Storage.DirectoryRenamed“

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-directory",
  "eventType": "Microsoft.Storage.DirectoryRenamed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-directory",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectorydeleted-event"></a>Ereignis „Microsoft.Storage.DirectoryDeleted“

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/directory-to-delete",
  "eventType": "Microsoft.Storage.DirectoryDeleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "DeleteDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/directory-to-delete",
    "recursive": "true", 
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Ereigniseigenschaften

Ein Ereignis weist die folgenden Daten auf oberster Ebene aus:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| topic | string | Vollständiger Ressourcenpfaf zur Ereignisquelle. Dieses Feld ist nicht beschreibbar. Dieser Wert wird von Event Grid bereitgestellt. |
| subject | string | Vom Herausgeber definierter Pfad zum Ereignisbetreff |
| eventType | string | Einer der registrierten Ereignistypen für die Ereignisquelle. |
| eventTime | string | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | string | Eindeutiger Bezeichner für das Ereignis. |
| data | object | Ereignisdaten für Blob Storage. |
| dataVersion | string | Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion. |
| metadataVersion | string | Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt. |

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| api | string | Der Vorgang, durch den das Ereignis ausgelöst wurde. |
| clientRequestId | string | Vom Client bereitgestellte Anforderungs-ID für den Speicher-API-Vorgang. Diese ID kann zur Korrelation mit Azure Storage-Diagnoseprotokollen anhand des Felds „client-request-id“ in den Protokollen verwendet und in Clientanforderungen mit dem Header „x-ms-client-request-id“ bereitgestellt werden. Informationen finden Sie unter [Storage Analytics Log Format](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format) (Storage Analytics-Protokollformat). |
| requestId | string | Dienstgenerierte Anforderungs-ID für den Speicher-API-Vorgang. Kann zum Korrelieren mit Azure Storage-Diagnoseprotokolle mithilfe des Felds „request-id-header“ in den Protokollen verwendet werden, und wird vom einleitenden API-Aufruf im „x-ms-request-id“-Header zurückgegeben. Informationen finden Sie unter [Storage Analytics Log Format](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format) (Storage Analytics-Protokollformat). |
| eTag | string | Der Wert, den Sie verwenden können, um Vorgänge bedingt auszuführen. |
| contentType | string | Der für das Blob angegebene Inhaltstyp. |
| contentLength | integer | Die Größe des Blobs in Byte. |
| blobType | string | Der Blobtyp. Gültige Werte sind „BlockBlob“ oder „PageBlob“. |
| contentOffset | number | Das Offset (in Bytes) eines Schreibvorgangs zu dem Zeitpunkt, zu dem die ereignisauslösende Anwendung das Schreiben in die Datei abgeschlossen hat. <br>Nur relevant für Ereignisse, die für Blob Storage-Konten mit einem hierarchischen Namespace ausgelöst wurden.|
| destinationUrl |string | Die URL der Datei, die nach Abschluss des Vorgangs vorhanden ist. Wenn also beispielsweise eine Datei umbenannt wird, enthält die Eigenschaft `destinationUrl` die URL des neuen Dateinamens. <br>Nur relevant für Ereignisse, die für Blob Storage-Konten mit einem hierarchischen Namespace ausgelöst wurden.|
| sourceUrl |string | Die URL der Datei, die vor dem Vorgang vorhanden ist. Wenn also beispielsweise eine Datei umbenannt wird, enthält `sourceUrl` die URL des ursprünglichen Dateinamens vor dem Umbenennungsvorgang. <br>Nur relevant für Ereignisse, die für Blob Storage-Konten mit einem hierarchischen Namespace ausgelöst wurden. |
| url | string | Der Pfad des Blobs. <br>Wenn der Client eine Blob-REST-API verwendet, hat die URL die folgende Struktur: *\<Speicherkontoname\>.blob.core.windows.net/\<Containername\>/\<Dateiname\>* . <br>Wenn der Client eine Data Lake Storage-REST-API verwendet, hat die URL die folgende Struktur: *\<Speicherkontoname\>.dfs.core.windows.net/\<Dateisystemname\>/\<Dateiname\>* . |
| recursive | string | `True`, um den Vorgang für alle untergeordneten Verzeichnisse auszuführen; andernfalls `False`. <br>Nur relevant für Ereignisse, die für Blob Storage-Konten mit einem hierarchischen Namespace ausgelöst wurden. |
| sequencer | string | Ein nicht transparenter Zeichenfolgenwert, der die logische Reihenfolge von Ereignissen für einen bestimmten Blobnamen darstellt.  Benutzer können anhand des standardmäßigen Zeichenfolgenvergleichs die relative Reihenfolge von zwei Ereignissen unter dem gleichen Blobnamen verstehen. |
| storageDiagnostics | object | Diagnosedaten, die gelegentlich vom Azure Storage-Dienst einbezogen werden. Falls vorhanden, sollten sie vom Ereignisconsumer ignoriert werden. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).
* Eine Einführung zum Arbeiten mit Blob Storage-Ereignissen finden Sie unter [Routen von Blob Storage-Ereignissen -– Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json). 
