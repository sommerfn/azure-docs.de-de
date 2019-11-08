---
title: Aktivität „Metadaten abrufen“ in Azure Data Factory
description: Erfahren Sie, wie Sie die Aktivität „Metadaten abrufen“ in einer Data Factory-Pipeline verwenden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: ''
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: e891f6675920e7bb90d2a6d007676cdd65f19917
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73679888"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Aktivität „Metadaten abrufen“ in Azure Data Factory

Die Aktivität „Metadaten abrufen“ kann zum Abrufen von Metadaten beliebiger Daten in Azure Data Factory verwendet werden. Die Aktivität kann in folgenden Szenarien verwendet werden:

- Überprüfen der Metadaten von Daten
- Auslösen einer Pipeline, wenn Daten bereit/verfügbar sind

Die folgende Funktionalität ist in der Ablaufsteuerung verfügbar:

- Sie können die Ausgabe der Aktivität „Metadaten abrufen“ in bedingten Ausdrücken für Überprüfungen verwenden.
- Sie können eine Pipeline über eine „Wiederholen bis“-Schleife auslösen, wenn eine Bedingung erfüllt ist.

## <a name="capabilities"></a>Funktionen

Die Aktivität „Metadaten abrufen“ nutzt ein Dataset als Eingabe und gibt Metadateninformationen als Ausgabe zurück. Derzeit werden die folgenden Connectors und entsprechenden abrufbaren Metadaten unterstützt. Die maximale Größe der zurückgegebenen Metadaten beträgt 1 MB.

>[!NOTE]
>Bei der Ausführung der Aktivität „Metadaten abrufen“ in einer selbstgehosteten Integration Runtime werden die aktuellen Funktionen unter Version 3.6 oder höher unterstützt.

### <a name="supported-connectors"></a>Unterstützte Connectors

**File Storage**

| Connector/Metadaten | itemName<br>(Datei/Ordner) | itemType<br>(Datei/Ordner) | size<br>(Datei) | created<br>(Datei/Ordner) | lastModified<br>(Datei/Ordner) |childItems<br>(Ordner) |contentMD5<br>(Datei) | structure<br/>(Datei) | columnCount<br>(Datei) | exists<br>(Datei/Ordner) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| [Amazon S3](connector-amazon-simple-storage-service.md) | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| [Google Cloud Storage](connector-google-cloud-storage.md) | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| [Azure Blob Storage](connector-azure-blob-storage.md) | √/√ | √/√ | √ | x/x | √/√* | √ | √ | √ | √ | √/√ |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure Files](connector-azure-file-storage.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [Dateisystem](connector-file-system.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [SFTP](connector-sftp.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [FTP](connector-ftp.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |

- Für Amazon S3 und Google Cloud Storage gilt `lastModified` für den Bucket und den Schlüssel, aber nicht für den virtuellen Ordner. `exists` gilt für den Bucket und den Schlüssel, aber nicht für das Präfix oder den virtuellen Ordner.
- Für Azure Blob Storage gilt `lastModified` für den Container und das Blob, aber nicht für den virtuellen Ordner.

**Relationale Datenbank**

| Connector/Metadaten | structure | columnCount | exists |
|:--- |:--- |:--- |:--- |
| [Azure SQL-Datenbank](connector-azure-sql-database.md) | √ | √ | √ |
| [Azure SQL-Datenbank – Verwaltete Instanz](connector-azure-sql-database-managed-instance.md) | √ | √ | √ |
| [Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md) | √ | √ | √ |
| [SQL Server](connector-sql-server.md) | √ | √ | √ |

### <a name="metadata-options"></a>Metadatenoptionen

Sie können die folgenden Metadatentypen in der Feldliste der Aktivität „Metadaten abrufen“ angeben, um die entsprechenden Informationen abzurufen:

| Metadatentyp | BESCHREIBUNG |
|:--- |:--- |
| itemName | Name der Datei oder des Ordners. |
| itemType | Typ der Datei oder des Ordners. Der zurückgegebene Wert lautet `File` oder `Folder`. |
| size | Größe der Datei in Bytes. Gilt nur für Dateien. |
| created | Datum/Uhrzeit der Erstellung der Datei oder des Ordners. |
| lastModified | Datum/Uhrzeit der letzten Änderung der Datei oder des Ordners. |
| childItems | Liste der Unterordner und Dateien im angegebenen Ordner. Gilt nur für Ordner. Der Rückgabewert ist eine Liste mit den Namen und Typen der einzelnen untergeordneten Elemente. |
| contentMD5 | MD5 der Datei. Gilt nur für Dateien. |
| structure | Datenstruktur der Datei oder Tabelle in der relationalen Datenbank. Der Rückgabewert ist eine Liste von Spaltennamen und Spaltentypen. |
| columnCount | Anzahl der Spalten in der Datei oder relationalen Tabelle. |
| exists| Gibt an, ob eine Datei, ein Ordner oder eine Tabelle vorhanden ist. Beachten Sie Folgendes: Wenn `exists` in der Feldliste der Aktivität „Metadaten abrufen“ angegeben ist, schlägt die Aktivität selbst dann nicht fehl, wenn die Datei, der Ordner oder die Tabelle nicht vorhanden ist. Stattdessen wird `exists: false` in der Ausgabe zurückgegeben. |

>[!TIP]
>Wenn Sie überprüfen möchten, ob eine Datei, ein Ordner oder eine Tabelle vorhanden ist, geben Sie `exists` in der Feldliste der Aktivität „Metadaten abrufen“ an. Anschließend können Sie das Ergebnis (`exists: true/false`) in der Ausgabe der Aktivität überprüfen. Falls `exists` nicht in der Feldliste angegeben ist, schlägt die Aktivität „Metadaten abrufen“ fehl, wenn das Objekt nicht gefunden wird.

>[!NOTE]
>Wenn Sie Metadaten aus Dateispeichern abrufen und `modifiedDatetimeStart` oder `modifiedDatetimeEnd` konfigurieren, enthält `childItems` in der Ausgabe nur Dateien im festgelegten Pfad, deren letzte Änderung im angegebenen Zeitraum liegt. Elemente in Unterordnern werden ausgeschlossen.

## <a name="syntax"></a>Syntax

**Aktivität „Metadaten abrufen“**

```json
{
    "name": "MyActivity",
    "type": "GetMetadata",
    "typeProperties": {
        "fieldList" : ["size", "lastModified", "structure"],
        "dataset": {
            "referenceName": "MyDataset",
            "type": "DatasetReference"
        }
    }
}
```

**Dataset**

```json
{
    "name": "MyDataset",
    "properties": {
    "type": "AzureBlob",
        "linkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath":"container/folder",
            "filename": "file.json",
            "format":{
                "type":"JsonFormat"
            }
        }
    }
}
```

## <a name="type-properties"></a>Typeigenschaften

Die Aktivität „Metadaten abrufen“ kann zurzeit die folgenden Arten von Metadateninformationen zurückgeben:

Eigenschaft | BESCHREIBUNG | Erforderlich
-------- | ----------- | --------
fieldList | Die erforderlichen Arten von Metadateninformationen. Ausführliche Informationen zu unterstützten Metadaten finden Sie im Abschnitt [Metadatenoptionen](#metadata-options) dieses Artikels. | Ja 
dataset | Das Referenzdataset, dessen Metadaten von der Aktivität „Metadaten abrufen“ abgerufen werden sollen. Weitere Informationen zu unterstützten Connectors finden Sie im Abschnitt [Funktionen](#capabilities). Ausführliche Informationen zur Syntax von Datasets finden Sie in den jeweiligen Connectorthemen. | Ja
formatSettings | Wird bei Verwendung eines formatierten Datasets angewendet. | Nein
storeSettings | Wird bei Verwendung eines formatierten Datasets angewendet. | Nein

## <a name="sample-output"></a>Beispielausgabe

Die Ergebnisse von „Metadaten abrufen“ werden in der Ausgabe der Aktivität angezeigt. Im Folgenden zwei Beispiele für umfassende Metadatenoptionen. Um die Ergebnisse in nachfolgenden Aktivitäten zu nutzen, verwenden Sie das Muster `@{activity('MyGetMetadataActivity').output.itemName}`.

### <a name="get-a-files-metadata"></a>Abrufen der Metadaten einer Datei

```json
{
  "exists": true,
  "itemName": "test.csv",
  "itemType": "File",
  "size": 104857600,
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "contentMD5": "cMauY+Kz5zDm3eWa9VpoyQ==",
  "structure": [
    {
        "name": "id",
        "type": "Int64"
    },
    {
        "name": "name",
        "type": "String"
    }
  ],
  "columnCount": 2
}
```

### <a name="get-a-folders-metadata"></a>Abrufen der Metadaten eines Ordners

```json
{
  "exists": true,
  "itemName": "testFolder",
  "itemType": "Folder",
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "childItems": [
    {
      "name": "test.avro",
      "type": "File"
    },
    {
      "name": "folder hello",
      "type": "Folder"
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich über weitere Ablaufsteuerungsaktivitäten, die von Data Factory unterstützt werden:

- [Aktivität „Pipeline ausführen“](control-flow-execute-pipeline-activity.md)
- [ForEach-Aktivität](control-flow-for-each-activity.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [Webaktivität](control-flow-web-activity.md)
