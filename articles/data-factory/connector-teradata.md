---
title: Kopieren von Daten aus Teradata mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Mit dem Teradata-Connector des Data Factory-Diensts können Sie Daten aus einer Teradata-Datenbank in Datenspeicher kopieren, die von Data Factory als Senken unterstützt werden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: jingwang
ms.openlocfilehash: ce326d7284e22a8734f6be671a277795ba659522
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68720522"
---
# <a name="copy-data-from-teradata-by-using-azure-data-factory"></a>Kopieren von Daten aus Teradata mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version des Data Factory-Diensts aus:"]
>
> * [Version 1](v1/data-factory-onprem-teradata-connector.md)
> * [Aktuelle Version](connector-teradata.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus einer Teradata-Datenbank zu kopieren. Der Artikel baut auf der [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einer Teradata-Datenbank in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der Teradata-Connector unterstützt insbesondere Folgendes:

- Teradata, **Version 14.10, 15.0, 15.10, 16.0, 16.10 und 16.20**.
- Kopieren von Dateien unter Verwendung der **Standard**- oder **Windows**-Authentifizierung
- Paralleles Kopieren aus einer Teradata-Quelle. Weitere Informationen finden Sie im Abschnitt [Paralleles Kopieren aus Teradata](#parallel-copy-from-teradata).

> [!NOTE]
>
> Nach der Veröffentlichung der Version 3.18 der selbstgehosteten Integration Runtime wurde von Azure Data Factory ein Upgrade des Teradata-Connectors durchgeführt. Bereits vorhandene Workloads, die den älteren Teradata-Connector verwenden, werden weiterhin unterstützt. Bei neuen Workloads empfiehlt es sich jedoch, die neue Version zu verwenden. Für den neuen Pfad wird eine andere Kombination aus verknüpftem Dienst, Dataset und Kopierquelle benötigt. Konfigurationsdetails finden Sie in den entsprechenden Folgeabschnitten.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Ihre Teradata-Instanz nicht öffentlich zugänglich ist, müssen Sie eine [selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md) einrichten. Die Integration Runtime verfügt ab Version 3.18 über einen integrierten Teradata-Treiber. Es ist keine manuelle Treiberinstallation erforderlich. Für den Treiber muss „Visual C++ Redistributable 2012 Update 4“ auf dem Computer mit der selbstgehosteten Integration Runtime vorhanden sein. Sollte diese Komponente noch nicht installiert sein, können Sie sie [hier](https://www.microsoft.com/en-sg/download/details.aspx?id=30679) herunterladen.

Wenn Sie nicht mindestens die Version 3.18 der selbstgehosteten Integration Runtime verwenden, installieren Sie auf dem Computer mit der Integration Runtime den [.NET-Datenanbieter für Teradata](https://go.microsoft.com/fwlink/?LinkId=278886) (ab Version 14). 

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren spezifischer Data Factory-Entitäten für den Teradata-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Der verknüpfte Teradata-Dienst unterstützt folgende Eigenschaften:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die Eigenschaft „type“ muss auf **Teradata** festgelegt sein. | Ja |
| connectionString | Gibt die Informationen an, die zum Herstellen einer Verbindung mit der Teradata-Datenbankinstanz erforderlich sind. Sehen Sie sich die folgenden Beispiele an.<br/>Sie können auch ein Kennwort in Azure Key Vault speichern und die `password`-Konfiguration aus der Verbindungszeichenfolge pullen. Ausführlichere Informationen finden Sie unter [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| username | Geben Sie einen Benutzernamen für die Verbindungsherstellung mit der Teradata-Datenbank an. Gilt bei Verwendung der Windows-Authentifizierung. | Nein |
| password | Geben Sie das Kennwort für das Benutzerkonto an, das Sie für den Benutzernamen angegeben haben. Sie können auch [auf ein Geheimnis verweisen, das in Azure Key Vault](store-credentials-in-key-vault.md) gespeichert ist. <br>Gilt, wenn Sie die Windows-Authentifizierung verwenden oder für die Standardauthentifizierung auf ein Kennwort in Key Vault verweisen. | Nein |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Eine selbstgehostete Integrationslaufzeit ist erforderlich (siehe [Voraussetzungen](#prerequisites)). |Ja |

**Beispiel mit Standardauthentifizierung**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>;Uid=<username>;Pwd=<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel mit Windows-Authentifizierung**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>",
            "username": "<username>",
            "password": "<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

> [!NOTE]
>
> Die folgende Nutzlast wird weiterhin unterstützt. In Zukunft sollte jedoch die neue verwendet werden.

**Vorherige Nutzlast:**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<Basic/Windows>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Teradata-Dataset unterstützt werden. Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie unter [„Datasets“](concepts-datasets-linked-services.md).

Zum Kopieren von Daten aus Teradata werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die Eigenschaft „type“ des Datasets muss auf `TeradataTable` festgelegt werden. | Ja |
| database | Der Name der Teradata-Datenbank. | Nein (wenn „query“ in der Aktivitätsquelle angegeben ist) |
| table | Der Name der Tabelle in der Teradata-Datenbank. | Nein (wenn „query“ in der Aktivitätsquelle angegeben ist) |

**Beispiel:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "TeradataTable",
        "typeProperties": {},
        "schema": [],        
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

> [!NOTE]
>
> Datasets vom Typ `RelationalTable` werden weiterhin unterstützt. Es wird jedoch empfohlen, das neue Dataset zu verwenden.

**Vorherige Nutzlast:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Quelle „Teradata“ unterstützt werden. Eine vollständige Liste der verfügbaren Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie unter [Pipelines](concepts-pipelines-activities.md). 

### <a name="teradata-as-a-source-type"></a>Teradata als Quelltyp

> [!TIP]
>
> Informationen zum effizienten Laden von Daten aus Teradata mittels Datenpartitionierung finden Sie im Abschnitt [Paralleles Kopieren aus Teradata](#parallel-copy-from-teradata).

Wenn Sie Daten aus Teradata kopieren wollen, werden die folgenden Eigenschaften im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die Eigenschaft „type“ der Quelle der Kopieraktivität muss auf `TeradataSource` festgelegt werden. | Ja |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Ein Beispiel ist `"SELECT * FROM MyTable"`.<br>Wenn Sie partitioniertes Laden aktivieren, müssen Sie die entsprechenden integrierten Partitionsparameter in Ihre Abfrage integrieren. Beispiele finden Sie im Abschnitt [Paralleles Kopieren aus Teradata](#parallel-copy-from-teradata). | Nein (wenn „table“ im Dataset angegeben ist) |
| partitionOptions | Spezifiziert die Datenpartitionierungsoptionen mit denen Daten aus Teradata geladen werden. <br>Zugelassene Werte sind: **„Keine“** (Standard), **„Hash“** und **„DynamicRange“** .<br>Wenn eine Partitionsoption aktiviert (also nicht `None` festgelegt) ist, konfigurieren Sie auch die Einstellung [`parallelCopies`](copy-activity-performance.md#parallel-copy) für die Kopieraktivität. Dadurch wird der Parallelitätsgrad für das parallele Laden von Daten aus einer Teradata-Datenbank bestimmt. Dieser Wert kann beispielsweise auf „4“ festgelegt werden. | Nein |
| partitionSettings | Geben Sie die Gruppe der Einstellung für die Datenpartitionierung an. <br>Verwenden Sie diese Option, wenn die Partitionsoption nicht `None` lautet. | Nein |
| partitionColumnName | Geben Sie den Namen der Quellspalte **als Integer** an, der von der Bereichspartitionierung für die parallele Kopie verwendet wird. Ohne Angabe wird der Primärschlüssel der Tabelle automatisch erkannt und als Partitionsspalte verwendet. <br>Verwenden Sie diese Option, wenn die Partitionsoption `Hash` oder `DynamicRange` lautet. Wenn Sie die Quelldaten mithilfe einer Abfrage abrufen, integrieren Sie `?AdfHashPartitionCondition` oder `?AdfRangePartitionColumnName` in die WHERE-Klausel. Ein Beispiel finden Sie im Abschnitt [„Parallele Kopie aus Teradata“](#parallel-copy-from-teradata). | Nein |
| partitionUpperBound | Der Höchstwert der Partitionsspalte zum Herauskopieren von Daten. <br>Wenden Sie dies an, wenn die Partitionsoption `DynamicRange` ist. Wenn Sie Quelldaten per Abfrage abrufen, integrieren Sie `?AdfRangePartitionUpbound` in die WHERE-Klausel. Ein Beispiel finden Sie im Abschnitt [Paralleles Kopieren aus Teradata](#parallel-copy-from-teradata). | Nein |
| PartitionLowerBound | Der Mindestwert der Partitionsspalte zum Herauskopieren von Daten. <br>Verwenden Sie diese Option, wenn die Partitionsoption `DynamicRange` lautet. Wenn Sie die Quelldaten mithilfe einer Abfrage abrufen, integrieren Sie `?AdfRangePartitionLowbound` in die WHERE-Klausel. Ein Beispiel finden Sie im Abschnitt [Paralleles Kopieren aus Teradata](#parallel-copy-from-teradata). | Nein |

> [!NOTE]
>
> Kopierquellen vom Typ `RelationalSource` werden zwar weiterhin unterstützt, das neue integrierte parallele Laden aus Teradata (Partitionsoptionen) wird allerdings nicht unterstützt. Es wird jedoch empfohlen, das neue Dataset zu verwenden.

**Beispiel: Kopieren von Daten mit einer einfachen Abfrage ohne Partition**

```json
"activities":[
    {
        "name": "CopyFromTeradata",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Teradata input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "TeradataSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="parallel-copy-from-teradata"></a>Parallele Kopie von Teradata

Der Data Factory-Teradata-Connector verfügt über eine integrierte Datenpartitionierung zum parallelen Kopieren von Daten aus Teradata. Die Datenpartitionierungsoptionen befinden sich auf der Registerkarte **Quelle** der Kopieraktivität.

![Screenshot der Partitionierungsoptionen](./media/connector-teradata/connector-teradata-partition-options.png)

Wenn Sie die partitioniertes Kopieren aktivieren, führt Data Factory parallele Abfragen für Ihre Teradata-Quelle aus, um Daten anhand von Partitionen zu laden. Der Parallelitätsgrad wird über die Einstellung [`parallelCopies`](copy-activity-performance.md#parallel-copy) der Kopieraktivität gesteuert. Wenn Sie `parallelCopies` also beispielsweise auf „4“ festlegen, generiert Data Factory parallel vier Abfragen und führt sie parallel aus (basierend auf der von Ihnen angegebenen Partitionsoption und den entsprechenden Einstellungen). Jede Abfrage ruft einen Teil der Daten aus Ihrer Teradata-Datenbank ab.

Es empfiehlt sich, das parallele Kopieren mit Datenpartitionierung zu aktivieren – insbesondere, wenn Sie große Datenmengen aus Ihrer Teradata-Datenbank laden. Im Anschluss finden Sie empfohlene Konfigurationen für verschiedene Szenarien:

| Szenario                                                     | Empfohlene Einstellungen                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Vollständiges Laden aus großer Tabelle                                   | **Partitionsoption**: Hash. <br><br/>Während der Ausführung erkennt Data Factory automatisch die PK-Spalte, wendet einen Hash darauf an und kopiert Daten anhand von Partitionen. |
| Laden einer großen Datenmenge unter Verwendung einer benutzerdefinierten Abfrage                 | **Partitionsoption**: Hash.<br>**Abfrage**: `SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>`.<br>**Partitionsspalte**: Geben Sie die Spalte für die Hashpartitionierung an. Ohne Angabe erkennt Data Factory automatisch die PK-Spalte der Tabelle, die Sie im Teradata-Dataset angegeben haben.<br><br>Während der Ausführung ersetzt Data Factory `?AdfHashPartitionCondition` durch die Hashpartitionierungslogik und sendet die Daten an Teradata. |
| Laden einer großen Datenmenge unter Verwendung einer benutzerdefinierten Abfrage, wenn eine Integerspalte mit gleichmäßig verteilten Werten für die Bereichspartitionierung vorhanden ist | **Partitionoptionen**: Dynamische Bereichspartitionierung.<br>**Abfrage**: `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Partitionsspalte**: Geben Sie die Spalte für die Datenpartitionierung an. Die Partitionierung kann auf der Grundlage der Spalte mit dem Datentyp „Integer“ erfolgen.<br>**Obergrenze der Partition** und **Untergrenze der Partition**: Geben Sie an, ob Sie anhand der Partitionsspalte filtern möchten, um nur Daten zwischen der Ober- und der Untergrenze zu erhalten.<br><br>Data Factory ersetzt während der Ausführung `?AdfRangePartitionColumnName`, `?AdfRangePartitionUpbound` und `?AdfRangePartitionLowbound` durch den tatsächlichen Spaltennamen und die Wertebereiche für die jeweilige Partition und sendet die Daten dann an Teradata. <br>Wenn Ihre Partitionsspalte „ID“ also beispielsweise mit der Untergrenze „1“ und der Obergrenze „80“ konfiguriert und paralleles Kopieren auf „4“ festgelegt ist, ruft Data Factory Daten anhand von vier Partitionen ab. Die ID-Bereiche sehen dann wie folgt aus: [1–20], [21–40], [41–60] und [61–80]. |

**Beispiel: Abfrage mit Hashpartition**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "Hash",
    "partitionSettings": {
        "partitionColumnName": "<hash_partition_column_name>"
    }
}
```

**Beispiel: Abfrage mit dynamischer Bereichspartition**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<dynamic_range_partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column>",
        "partitionLowerBound": "<lower_value_of_partition_column>"
    }
}
```

## <a name="data-type-mapping-for-teradata"></a>Datentypzuordnung für Teradata

Beim Kopieren von Daten aus Teradata gelten folgende Zuordnungen. Weitere Informationen dazu, wie die Kopieraktivität das Quellschema und den Datentyp zur Senke zuordnet, finden Sie unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md).

| Teradata-Datentyp | Data Factory-Zwischendatentyp |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Byte[] |
| Byte |Byte[] |
| ByteInt |Int16 |
| Char |String |
| Clob |String |
| Date |DateTime |
| Decimal |Decimal |
| Double |Double |
| Graphic |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Integer |Int32 |
| Interval Day |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Day To Hour |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Day To Minute |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Day To Second |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Hour |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Hour To Minute |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Hour To Second |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Minute |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Minute To Second |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Month |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Second |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Year |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Interval Year To Month |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Number |Double |
| Zeitraum (Datum) |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Zeitraum (Zeit) |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Zeitraum (Zeit mit Zeitzone) |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Zeitraum (Zeitstempel) |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Zeitraum (Zeitstempel mit Zeitzone) |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| SmallInt |Int16 |
| Time |TimeSpan |
| Time With Time Zone |TimeSpan |
| Timestamp |DateTime |
| Timestamp With Time Zone |Datetime |
| VarByte |Byte[] |
| VarChar |String |
| VarGraphic |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Xml |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
