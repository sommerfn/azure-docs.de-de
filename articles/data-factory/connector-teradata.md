---
title: Kopieren von Daten aus Teradata mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie mehr über den Teradata-Connector von Data Factory, mit dem Sie Daten aus einer Teradata-Datenbank als Senken in von Data Factory unterstützte Datenspeicher kopieren können.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: 63f28c8b6eaceed12e1f76e9c0c5984e3b63b500
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561429"
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>Kopieren von Daten aus Teradata mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Wählen Sie die von Ihren verwendete Version des Data Factory-Diensts aus:"]
>
> * [Version 1](v1/data-factory-onprem-teradata-connector.md)
> * [Aktuelle Version](connector-teradata.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus einer Teradata-Datenbank zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einer Teradata-Datenbank in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der Teradata-Connector unterstützt insbesondere Folgendes:

- Teradata, **Version 14.10, 15.0, 15.10, 16.0, 16.10 und 16.20**.
- Kopieren von Dateien unter Verwendung der **Standard**- oder **Windows**-Authentifizierung
- Parallele Kopie von Teradata-Quelle. Weitere Informationen finden Sie unter [„Parallele Kopie aus Teradata“](#parallel-copy-from-teradata).

> [!NOTE]
>
> Azure Data Factory hat für den Teradata-Connector seit der selbstgehosteten Integration Runtime v3.18 ein Upgrade durchgeführt, das von einem integrierten ODBC-Treiber unterstützt wird und flexible Verbindungsoptionen sowie eine standardmäßige parallele Kopie bereitstellt, um die Leistung zu verbessern. Vorhandene Workloads mit dem vorherigen Teradata-Connector mit .NET-Datenabieter für Teradata werden immer noch unterstützt. Allerdings sehen Sie eine Aufforderung, in Zukunft den Neuen zu verwenden. Bitte beachten Sie, dass für den neuen Pfad eine andere Gruppe verknüpfter Dienste/Datasets/Kopiequelle erforderlich ist. Weitere Informationen finden Sie im entsprechenden Abschnitt über die Konfiguration.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Ihre Teradata nicht öffentlich zugänglich ist, müssen Sie eine selbstgehostete Integration Runtime einrichten. Weitere Informationen zur selbstgehosteten Integration Runtime finden Sie unter [Selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md). Die Integration Runtime bietet ab Version 3.18 einen integrierten Teradata-Treiber. Daher müssen keine Treiber manuell installiert werden. Der Treiber erfordert das „Visual C++ Redistributable 2012 Update 4“ auf dem selbstgehosteten IR-Computer. Falls Sie es noch nicht installiert haben, können Sie es [hier](https://www.microsoft.com/en-sg/download/details.aspx?id=30679) herunterladen.

Bei einer selbstgehosteten IR-Version vor 3.18 müssen Sie den [.NET-Datenanbieter für Teradata](https://go.microsoft.com/fwlink/?LinkId=278886) (Version 14 oder neuer) auf dem Computer mit der Integration Runtime installieren. 

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Teradata-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Teradata verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf Folgendes festgelegt werden: **Teradata** | Ja |
| connectionString | Gibt die Informationen an, die zum Herstellen einer Verbindung mit der Teradata-Datenbankinstanz erforderlich sind. Sehen Sie sich die folgenden Beispiele an.<br/>Sie können auch das Kennwort in Azure Key Vault speichern und die `password`-Konfiguration aus der Verbindungszeichenfolge pullen. Ausführlichere Informationen finden Sie im Artikel [„Speichern von Anmeldeinformationen in Azure Key Vault“](store-credentials-in-key-vault.md). | Ja |
| userName | Geben Sie einen Benutzernamen für das Herstellen der Verbindung mit der Teradata-Datenbank an. Gilt für die Windows-Authentifizierung. | Nein |
| password | Geben Sie das Kennwort für das Benutzerkonto an, das Sie für den Benutzernamen angegeben haben. Sie können auch [auf ein Geheimnis verweisen, das in Azure Key Vault](store-credentials-in-key-vault.md) gespeichert ist. <br>Dies gilt, wenn Sie die Windows-Authentifizierung verwenden oder auf ein Kennwort in Key Vault zur Standardauthentifizierung verweisen. | Nein |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Eine selbstgehostete Integrationslaufzeit ist erforderlich, wie unter [Voraussetzungen](#prerequisites) erwähnt wird. |Ja |

**Beispiel: Verwenden der Standardauthentifizierung**

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

**Beispiel: Verwenden der Windows-Authentifizierung**

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
> Wenn Sie den verknüpften Teradata-Dienst von .NET-Datenanbieter für Teradata mit der folgenden Nutzlast verwendet haben, wird er weiterhin unverändert unterstützt. Es wird jedoch empfohlen, zukünftig die neue Version zu verwenden.

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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Dataset „Teradata“ unterstützt werden.

Zum Kopieren von Daten aus Teradata werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf folgenden Wert festgelegt werden: **TeradataTable** | Ja |
| database | Name der Teradata-Datenbank. | Nein (wenn „query“ in der Aktivitätsquelle angegeben ist) |
| table | Name der Tabelle in der Teradata-Datenbank. | Nein (wenn „query“ in der Aktivitätsquelle angegeben ist) |

**Beispiel:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "TeradataTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

> [!NOTE]
>
> Wenn Sie das Datenset vom Typ „RelationalTable“ wie folgt verwenden, wird es weiterhin unverändert unterstützt. Es wird jedoch empfohlen, zukünftig die neue Version zu verwenden.

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

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Quelle „Teradata“ unterstützt werden.

### <a name="teradata-as-source"></a>Teradata als Quelle

> [!TIP]
>
> Erfahren Sie mehr über das effiziente Laden von Daten aus Teradata mit Datenpartitionierung unter [„Parallele Kopie aus Teradata“](#parallel-copy-from-teradata).

Wenn Sie Daten aus Teradata kopieren wollen, werden die folgenden Eigenschaften im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf Folgendes festgelegt werden: **TeradataSource** | Ja |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Beispiel: `"SELECT * FROM MyTable"`.<br>Wenn Sie die partitionierte Last aktivieren, müssen Sie die entsprechenden integrierten Partitionsparameter in Ihre Abfrage einbinden. Beispiele finden Sie im Abschnitt [„Parallele Kopie aus Teradata“](#parallel-copy-from-teradata). | Nein (wenn „table“ im Dataset angegeben ist) |
| partitionOptions | Spezifiziert die Datenpartitionierungsoptionen mit denen Daten aus Teradata geladen werden. <br>Zugelassene Werte sind: **„Keine“** (Standard), **„Hash“** und **„DynamicRange“** .<br>Wenn die Partitionsoption aktiviert ist (nicht „Keine“), dann konfigurieren Sie auch die **[`parallelCopies`](copy-activity-performance.md#parallel-copy)** -Einstellung der Kopieraktivität. Dies kann z. B. 4 sein. Damit wird der Parallelitätsgrad bestimmt, der gleichzeitig die Daten aus der Teradata-Datenbank laden soll. | Nein |
| partitionSettings | Geben Sie die Gruppe der Einstellung für die Datenpartitionierung an. <br>Wenden Sie dies an, wenn die Partitionsoption nicht `None` ist. | Nein |
| partitionColumnName | Geben Sie den Namen der Quellspalte **als Integer** an, der von der Bereichspartitionierung für die parallele Kopie verwendet wird. Wird er nicht angegeben, wird der Primärschlüssel der Tabelle automatisch erkannt und als Partitionsspalte verwendet. <br>Wenden Sie dies an, wenn die Partitionsoption `Hash` oder `DynamicRange` ist. Wenn Sie die Abfrage zum Aufrufen von Quelldaten verwenden, setzen Sie einen Hook `?AdfHashPartitionCondition` oder `?AdfRangePartitionColumnName` in der WHERE-Klausel. Ein Beispiel finden Sie im Abschnitt [„Parallele Kopie aus Teradata“](#parallel-copy-from-teradata). | Nein |
| partitionUpperBound | Höchstwert der Partitionsspalte zum Herauskopieren von Daten. <br>Wenden Sie dies an, wenn die Partitionsoption `DynamicRange` ist. Wenn Sie die Abfrage zum Aufrufen von Quelldaten verwenden, setzen Sie einen Hook `?AdfRangePartitionUpbound` in der WHERE-Klausel. Ein Beispiel finden Sie im Abschnitt [„Parallele Kopie aus Teradata“](#parallel-copy-from-teradata). | Nein |
| PartitionLowerBound | Mindestwert der Partitionsspalte zum Herauskopieren von Daten. <br>Wenden Sie dies an, wenn die Partitionsoption `DynamicRange` ist. Wenn Sie die Abfrage zum Aufrufen von Quelldaten verwenden, setzen Sie einen Hook `?AdfRangePartitionLowbound` in der WHERE-Klausel. Ein Beispiel finden Sie im Abschnitt [„Parallele Kopie aus Teradata“](#parallel-copy-from-teradata). | Nein |

> [!NOTE]
>
> Wenn Sie den Typ „RelationalSource“ als Kopierquelle verwendet haben, dann wird dieser weiterhin in seiner gegenwärtigen Form unterstützt. Allerdings wird die neue integrierte Parallellast von Teradata (Partitionsoptionen) nicht unterstütz. Es wird jedoch empfohlen, in Zukunft das neue Modell zu verwenden.

**Beispiel: Kopieren von Daten mit einer grundlegenden Abfrage ohne Partition**

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

Der Data Factory Teradata-Konnektor verfügt über eine integrierte Datenpartitionierung zum leistungsstarken, parallelen Kopieren von Daten aus Teradata. Sie finden Optionen zur Datenpartitionierung unter Kopieraktivität -> Teradata-Quelle:

![Partitionsoptionen](./media/connector-teradata/connector-teradata-partition-options.png)

Wenn Sie die partitionierte Kopie aktivieren, dann führt Data Factory parallel Abfragen Ihrer Teradata-Quelle durch, um Daten in Partitionen zu laden. Der Parallelitätsgrad wird über die **[`parallelCopies`](copy-activity-performance.md#parallel-copy)** -Einstellung der Kopieraktivität konfiguriert und gesteuert. Wenn Sie zum Beispiel `parallelCopies` als vier bestimmen, dann generiert Data Factory gleichzeitig vier Abfragen und führt sie aus. Diese Abfragen basieren auf der von Ihnen angegebenen Partitionsoption und den Einstellungen und ruft einen Teil Ihrer Daten aus Ihrer Teradata-Datenbank auf.

Es ist ratsam, dass Sie die parallele Kopie mit der Datenpartitionierung aktivieren, vor allem, wenn Sie große Mengen Daten aus der Teradata-Datenbank laden wollen. Es folgen einige empfohlene Konfigurationen für verschiedene Szenarien:

| Szenario                                                     | Empfohlene Einstellungen                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Vollständiges laden aus großer Tabelle                                   | **Partitionsoption**: Hash. <br><br/>Während der Ausführung erkennt Data Factory automatisch die PK-Spalte, wendet den Hash darauf an und kopiert Daten nach Partitionen. |
| Laden von großen Datenmengen mit benutzerdefinierten Abfragen                 | **Partitionsoption**: Hash.<br>**Abfrage**: `SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>`.<br>**Partitionsspalte**: Geben Sie die Spalte für die Hashpartitionierung an. Wenn sie nicht angegeben wird, wählt ADF automatisch die PK-Spalte der Tabelle, die Sie im Teradata-Dataset angegeben haben.<br><br>Während der Ausführung ersetzt Data Factory `?AdfHashPartitionCondition` mit der Hashpartitionierungslogik und schickt es an Teradata. |
| Laden Sie große Mengen Daten mit einer benutzerdefinierten Abfrage herunter. Eine Integerspalte mit gleichmäßig verteilten Werten für die Bereichspartitionierung ist vorhanden. | **Partitionoptionen**: Dynamische Bereichspartitionierung.<br>**Abfrage**: `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Partitionsspalte**: Geben Sie die Spalte für die Datenpartitionierung an. Sie können mit der Spalte mit dem Integer-Datentyp partitionieren.<br>**Obergrenze der Partition** und **Untergrenze der Partition**: Geben Sie an, ob Sie mit der Partitionsspalte filtern möchten, um nur Daten zwischen der Ober- und der Untergrenze zu erhalten.<br><br>Data Factory ersetzt während der Ausführung `?AdfRangePartitionColumnName`, `?AdfRangePartitionUpbound` und `?AdfRangePartitionLowbound` mit dem tatsächlichen Spaltennamen und Wertebereichen für jede Partition, um sie dann an Teradata zu senden. <br>Wenn Ihre Partitionsspalte zum Beispiel „ID“ mit einer Untergrenze von 1 und einer Obergrenze von 80 ist, und die parallele Kopie 4 ist, dann ruft ADF die Daten aus 4 Partitionen mit IDs zwischen [1,20], [21, 40], [41, 60] und [61, 80] ab. |

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

Beim Kopieren von Daten aus Teradata werden die folgenden Zuordnungen von Teradata-Datentypen zu Azure Data Factory-Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

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
| Timestamp With Time Zone |DateTime |
| VarByte |Byte[] |
| VarChar |String |
| VarGraphic |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |
| Xml |Nicht unterstützt. Wenden Sie explizite Umwandlungen in der Quellabfrage durch. |


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
