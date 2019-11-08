---
title: Fehlertoleranz der Kopieraktivität in Azure Data Factory
description: Erfahren Sie, wie Sie der Kopieraktivität von Azure Data Factory Fehlertoleranz hinzufügen, indem inkompatible Zeilen übersprungen werden.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: yexu
ms.openlocfilehash: a60cafd529db1c6726a15db2c442af8d097411cc
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73678159"
---
#  <a name="fault-tolerance-of-copy-activity-in-azure-data-factory"></a>Fehlertoleranz der Kopieraktivität in Azure Data Factory
> [!div class="op_single_selector" title1="Wählen Sie die von Ihren verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-copy-activity-fault-tolerance.md)
> * [Aktuelle Version](copy-activity-fault-tolerance.md)

Die Kopieraktivität in Azure Data Factory bietet beim Kopieren von Daten zwischen Quell- und Senkendatenspeichern zwei Optionen für den Umgang mit nicht kompatiblen Zeilen:

- Die Kopieraktivität kann mit einem Fehler abgebrochen werden, wenn sie auf inkompatible Daten stößt (Standardverhalten).
- Der Kopiervorgang kann für alle Daten fortgesetzt werden, indem Fehlertoleranz hinzugefügt wird und die inkompatiblen Datenzeilen übersprungen werden. Darüber hinaus können die inkompatiblen Zeilen in Azure Blob Storage oder Azure Data Lake Store protokolliert werden. Sie können dann das Protokoll überprüfen, um die Ursache des Fehlers zu ermitteln, die Daten in der Datenquelle korrigieren und die Kopieraktivität wiederholen.

## <a name="supported-scenarios"></a>Unterstützte Szenarien
Die Kopieraktivität unterstützt drei Szenarien zum Erkennen, Überspringen und Protokollieren inkompatibler Daten:

- **Inkompatibilität zwischen dem Quelldatentyp und dem nativen Senkentyp**. 

    Beispiel:  Kopieren von Daten aus einer CSV-Datei in Blob Storage in eine SQL-Datenbank mit einer Schemadefinition, die drei Spalten vom Typ „INT“ enthält. Die Zeilen der CSV-Datei, die numerische Daten wie z.B. 123,456,789 enthalten, werden erfolgreich in den Senkenspeicher kopiert. Die Zeilen mit nicht numerischen Werten, z.B. 123,456, abc, werden dagegen als nicht kompatibel erkannt und übersprungen.

- **Fehlende Übereinstimmung bei der Anzahl der Spalten zwischen der Quelle und der Senke**.

    Beispiel:  Kopieren von Daten aus einer CSV-Datei in Blob Storage in eine SQL-Datenbank mit einer Schemadefinition, die sechs Spalten enthält. Die Zeilen der CSV-Datei, die sechs Spalten enthalten, werden erfolgreich in den Senkenspeicher kopiert. Die Zeilen der CSV-Datei mit weniger oder mehr als sechs Spalten werden als inkompatibel erkannt und übersprungen.

- **Primärschlüsselverletzung beim Schreiben in SQL Server/Azure SQL-Datenbank/Azure Cosmos DB**

    Beispiel:  Kopieren von Daten von einer SQL Server-Instanz in eine SQL-Datenbank. In der SQL-Datenbank der Senke ist ein Primärschlüssel definiert, in der SQL Server-Instanz der Quelle ist dagegen kein Primärschlüssel definiert. Die doppelten Zeilen, die in der Quelle vorhanden sind, können nicht in die Senke kopiert werden. Die Kopieraktivität kopiert nur die erste Zeile der Quelldaten in die Senke. Die nachfolgenden Quellzeilen, die den doppelten Primärschlüsselwert enthalten, werden als inkompatibel erkannt und übersprungen.

>[!NOTE]
>- Um Daten mit PolyBase in SQL Data Warehouse zu laden, verwenden Sie die native Fehlertoleranzunterstützung von PolyBase, indem Sie in der Kopieraktivität über [polyBaseSettings](connector-azure-sql-data-warehouse.md#azure-sql-data-warehouse-as-sink) Ablehnungsrichtlinien angeben. Sie können für nicht mit PolyBase kompatible Zeilen weiterhin die Umleitung an ein Blob oder ADLS aktivieren, wie unten gezeigt.
>- Dieses Feature ist nicht anwendbar, wenn die Kopieraktivität zum Aufruf von [Amazon Redshift Unload](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift) konfiguriert ist.
>- Dieses Feature ist nicht anwendbar, wenn die Kopieraktivität dafür konfiguriert ist, eine [gespeicherte Prozedur aus einer SQL-Senke](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database#invoke-a-stored-procedure-from-a-sql-sink) aufzurufen.

## <a name="configuration"></a>Konfiguration
Das folgende Beispiel umfasst eine JSON-Definition zum Konfigurieren des Überspringens inkompatibler Datenzeilen in der Kopieraktivität:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },
    "enableSkipIncompatibleRow": true,
    "redirectIncompatibleRowSettings": {
         "linkedServiceName": {
              "referenceName": "<Azure Storage or Data Lake Store linked service>",
              "type": "LinkedServiceReference"
            },
            "path": "redirectcontainer/erroroutput"
     }
}
```

Eigenschaft | BESCHREIBUNG | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | -------- 
enableSkipIncompatibleRow | Gibt an, ob nicht kompatible Zeilen beim Kopieren übersprungen werden sollen. | True<br/>False (Standardwert) | Nein
redirectIncompatibleRowSettings | Eine Gruppe von Eigenschaften, die angegeben werden können, wenn Sie die inkompatiblen Zeilen protokollieren möchten. | &nbsp; | Nein
linkedServiceName | Der verknüpfte Dienst von [Azure Storage](connector-azure-blob-storage.md#linked-service-properties) oder [Azure Data Lake Store](connector-azure-data-lake-store.md#linked-service-properties) zum Speichern des Protokolls mit den übersprungenen Zeilen | Der Name eines verknüpften Diensts vom Typ `AzureStorage` oder `AzureDataLakeStore`, der auf die Instanz verweist, in der die Protokolldatei gespeichert werden soll. | Nein
path | Der Pfad der Protokolldatei, die die übersprungenen Zeilen enthält. | Geben Sie den gewünschten Pfad für die Protokollierung der inkompatiblen Daten an. Wenn Sie keinen Pfad angeben, erstellt der Dienst automatisch einen Container. | Nein

## <a name="monitor-skipped-rows"></a>Überwachen der übersprungenen Zeilen
Nach Abschluss der Ausführung der Kopieraktivität wird die Anzahl übersprungener Zeilen in der Ausgabe der Kopieraktivität angezeigt:

```json
"output": {
            "dataRead": 95,
            "dataWritten": 186,
            "rowsCopied": 9,
            "rowsSkipped": 2,
            "copyDuration": 16,
            "throughput": 0.01,
            "redirectRowPath": "https://myblobstorage.blob.core.windows.net//myfolder/a84bf8d4-233f-4216-8cb5-45962831cd1b/",
            "errors": []
        },

```
Wenn Sie das Protokollieren der nicht kompatiblen Zeilen konfigurieren, finden Sie die Protokolldatei in diesem Pfad: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv`. 

Die Protokolldateien können nur CSV-Dateien sein. Die ursprünglichen übersprungenen Daten werden bei Bedarf mit Komma als Spaltentrennzeichen protokolliert. Wir fügen den ursprünglichen Quelldaten in der Protokolldatei zwei weitere Spalten „ErrorCode“ und „ErrorMessage“ hinzu, die die Grundursache der Inkompatibilität angeben. „ErrorCode“ und „ErrorMessage“ werden in doppelte Anführungszeichen gesetzt. 

Der Inhalt der Protokolldatei kann beispielsweise wie folgt aussehen:

```
data1, data2, data3, "UserErrorInvalidDataValue", "Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'."
data4, data5, data6, "2627", "Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4)."
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den anderen Artikeln zur Kopieraktivität:

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Leistung der Kopieraktivität](copy-activity-performance.md)


