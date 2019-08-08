---
title: Kopieren von Daten nach bzw. aus Oracle mit Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie mithilfe von Data Factory Daten aus unterstützten Quellspeichern in eine Oracle-Datenbank oder aus Oracle in unterstützte Senkenspeicher kopiert werden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: jingwang
ms.openlocfilehash: 04f623a889a87c325b1f53e3b39656ca4b703961
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509230"
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Kopieren von Daten aus und nach Oracle mit Azure Data Factory
> [!div class="op_single_selector" title1="Wählen Sie die von Ihren verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-onprem-oracle-connector.md)
> * [Aktuelle Version](connector-oracle.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus einer und in eine Oracle-Datenbank zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einer Oracle-Datenbank in jeden unterstützten Senkendatenspeicher kopieren. Zudem können Sie Daten aus jedem unterstützten Quelldatenspeicher in eine Oracle-Datenbank kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Dieser Oracle-Connector unterstützt insbesondere Folgendes:

- Die folgenden Versionen einer Oracle-Datenbank:
  - Oracle 12c R1 (12.1)
  - Oracle 11g R1, R2 (11.1, 11.2)
  - Oracle 10g R1, R2 (10.1, 10.2)
  - Oracle 9i R1, R2 (9.0.1, 9.2)
  - Oracle 8i R3 (8.1.7)
- Kopieren von Dateien unter Verwendung der **Standard**- oder **OID**-Authentifizierung.
- Parallele Kopie aus einer Oracle-Quelle. Weitere Informationen finden Sie unter [„Parallele Kopie aus Oracle“](#parallel-copy-from-oracle).

> [!Note]
> Der Oracle-Proxyserver wird nicht unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

Zum Kopieren von Daten aus einer bzw. in eine Oracle-Datenbank, die nicht öffentlich zugänglich ist, müssen Sie eine selbstgehostete Integration Runtime einrichten. Weitere Informationen zur selbstgehosteten Integration Runtime finden Sie unter [Selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md). Die Integration Runtime stellt einen integrierten Oracle-Treiber bereit. Daher müssen Sie zum Kopieren von Daten aus und nach Oracle nicht manuell einen Treiber erstellen.

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Oracle-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Die folgenden Eigenschaften werden für den mit Oracle verknüpften Dienst unterstützt.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **Oracle** festgelegt werden. | Ja |
| connectionString | Gibt die Informationen an, die zum Herstellen einer Verbindung mit der Oracle-Datenbankinstanz erforderlich sind. <br/>Markieren Sie dieses Feld als „SecureString“, um es sicher in Data Factory zu speichern. Sie können auch das Kennwort in Azure Key Vault speichern und die `password`-Konfiguration aus der Verbindungszeichenfolge pullen. Ausführlichere Informationen finden Sie in den folgenden Beispielen und im Artikel [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md). <br><br>**Unterstützter Verbindungstyp:** Sie können die **Oracle-SID** oder den **Oracle-Dienstnamen** zur Identifizierung Ihrer Datenbank verwenden:<br>– Wenn Sie die SID verwenden: `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>– Wenn Sie den Dienstnamen verwenden: `Host=<host>;Port=<port>;ServiceName=<servicename>;User Id=<username>;Password=<password>;` | Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die selbstgehostete Integration Runtime oder Azure Integration Runtime verwenden (sofern Ihr Datenspeicher öffentlich zugänglich ist). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

>[!TIP]
>Wenn der Fehler „ORA-01025: UPI parameter out of range“ angezeigt wird und Sie die Oracle-Version 8i nutzen, können Sie Ihrer Verbindungszeichenfolge `WireProtocolMode=1` hinzufügen und den Vorgang wiederholen.

**Zum Aktivieren der Verschlüsselung bei einer Oracle-Verbindung** haben Sie zwei Möglichkeiten:

1.  Um die **Triple-DES-Verschlüsselung (3DES) und Advanced Encryption Standard (AES)** zu verwenden, wechseln Sie auf Oracle-Serverseite zu Oracle Advanced Security (OAS), und konfigurieren Sie die Verschlüsselungseinstellungen. Ausführliche Informationen dazu finden Sie [hier](https://docs.oracle.com/cd/E11882_01/network.112/e40393/asointro.htm#i1008759). Der ADF-Oracle-Connector handelt automatisch die zu verwendende Verschlüsselungsmethode als diejenige aus, die Sie in OAS beim Herstellen der Verbindung mit Oracle konfigurieren.

2.  Zur Verwendung von **SSL** führen Sie die folgenden Schritte aus:

    1.  Rufen Sie die SSL-Zertifikatsinformationen ab. Rufen Sie die DER-codierten Zertifikatsinformationen von Ihrem SSL-Zertifikat ab, und speichern Sie die Ausgabe (----- Begin Certificate ... End Certificate -----) als Textdatei.

        ```
        openssl x509 -inform DER -in [Full Path to the DER Certificate including the name of the DER Certificate] -text
        ```

        **Beispiel:** Extrahieren Sie die Zertifikatsinformationen aus DERcert.cer; speichern Sie anschließend die Ausgabe in cert.txt

        ```
        openssl x509 -inform DER -in DERcert.cer -text
        Output:
        -----BEGIN CERTIFICATE-----
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXX
        -----END CERTIFICATE-----
        ```
    
    2.  Erstellen Sie den KeyStore oder TrustStore. Mit dem folgenden Befehl wird die TrustStore-Datei mit oder ohne Kennwort im PKCS-12-Format erstellt.

        ```
        openssl pkcs12 -in [Path to the file created in the previous step] -out [Path and name of TrustStore] -passout pass:[Keystore PWD] -nokeys -export
        ```

        **Beispiel:** erstellt eine PKCS12-TrustStore-Datei namens „MyTrustStoreFile“ mit einem Kennwort.

        ```
        openssl pkcs12 -in cert.txt -out MyTrustStoreFile -passout pass:ThePWD -nokeys -export  
        ```

    3.  Speichern Sie die TrustStore-Datei auf dem Computer mit der selbstgehosteten IR, z. B. auf C:\MyTrustStoreFile.
    4.  Konfigurieren Sie in ADF die Oracle-Verbindungszeichenfolge mit `EncryptionMethod=1` und entsprechendem `TrustStore`/`TrustStorePassword`-Wert, z. B. `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;EncryptionMethod=1;TrustStore=C:\\MyTrustStoreFile;TrustStorePassword=<trust_store_password>`.

**Beispiel:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel: Speichern des Kennworts in Azure Key Vault**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Oracle-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus bzw. nach Oracle die type-Eigenschaft des Datasets auf **OracleTable** fest. Die folgenden Eigenschaften werden unterstützt.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf **OracleTable** festgelegt werden. | Ja |
| tableName |Der Name der Tabelle in der Oracle-Datenbank, auf die der verknüpfte Dienst verweist. | Ja |

**Beispiel:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Oracle-Quelle und -Senke unterstützt werden.

### <a name="oracle-as-a-source-type"></a>Oracle als Quelltyp

> [!TIP]
>
> Erfahren Sie mehr über das effiziente Laden von Daten aus Oracle mit Datenpartitionierung unter [„Parallele Kopie aus Oracle“](#parallel-copy-from-oracle).

Legen Sie zum Kopieren von Daten aus Oracle den Quelltyp in der Kopieraktivität auf **OracleSource** fest. Die folgenden Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **OracleSource** festgelegt werden. | Ja |
| oracleReaderQuery | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Ein Beispiel ist `"SELECT * FROM MyTable"`.<br>Wenn Sie die partitionierte Last aktivieren, müssen Sie die entsprechenden integrierten Partitionsparameter in Ihre Abfrage einbinden. Beispiele finden Sie im Abschnitt [„Parallele Kopie aus Oracle“](#parallel-copy-from-oracle). | Nein |
| partitionOptions | Spezifiziert die Datenpartitionierungsoptionen, anhand derer Daten aus Oracle geladen werden. <br>Zugelassene Werte sind: **„Keine“** (Standard), **„PhysicalPartitionsOfTable“** und **„DynamicRange“** .<br>Wenn die Partitionsoption aktiviert ist (nicht „Keine“), dann konfigurieren Sie auch die **[`parallelCopies`](copy-activity-performance.md#parallel-copy)** -Einstellung der Kopieraktivität. Dies kann z. B. 4 sein. Damit wird der Parallelitätsgrad bestimmt, der gleichzeitig die Daten aus der Oracle-Datenbank laden soll. | Nein |
| partitionSettings | Geben Sie die Gruppe der Einstellung für die Datenpartitionierung an. <br>Wenden Sie dies an, wenn die Partitionsoption nicht `None` ist. | Nein |
| partitionNames | Die Liste der physischen Partitionen, die kopiert werden sollen. <br>Wenden Sie dies an, wenn die Partitionsoption `PhysicalPartitionsOfTable` ist. Wenn Sie die Abfrage zum Aufrufen von Quelldaten verwenden, setzen Sie einen Hook `?AdfTabularPartitionName` in der WHERE-Klausel. Beispiele finden Sie im Abschnitt [„Parallele Kopie aus Oracle“](#parallel-copy-from-oracle). | Nein |
| partitionColumnName | Geben Sie den Namen der Quellspalte **als Integer** an, der von der Bereichspartitionierung für die parallele Kopie verwendet wird. Wird er nicht angegeben, wird der Primärschlüssel der Tabelle automatisch erkannt und als Partitionsspalte verwendet. <br>Wenden Sie dies an, wenn die Partitionsoption `DynamicRange` ist. Wenn Sie die Abfrage zum Aufrufen von Quelldaten verwenden, setzen Sie einen Hook `?AdfRangePartitionColumnName` in der WHERE-Klausel. Beispiele finden Sie im Abschnitt [„Parallele Kopie aus Oracle“](#parallel-copy-from-oracle). | Nein |
| partitionUpperBound | Höchstwert der Partitionsspalte zum Herauskopieren von Daten. <br>Wenden Sie dies an, wenn die Partitionsoption `DynamicRange` ist. Wenn Sie die Abfrage zum Aufrufen von Quelldaten verwenden, setzen Sie einen Hook `?AdfRangePartitionUpbound` in der WHERE-Klausel. Beispiele finden Sie im Abschnitt [„Parallele Kopie aus Oracle“](#parallel-copy-from-oracle). | Nein |
| partitionLowerBound | Mindestwert der Partitionsspalte zum Herauskopieren von Daten. <br>Wenden Sie dies an, wenn die Partitionsoption `DynamicRange` ist. Wenn Sie die Abfrage zum Aufrufen von Quelldaten verwenden, setzen Sie einen Hook `?AdfRangePartitionLowbound` in der WHERE-Klausel. Beispiele finden Sie im Abschnitt [„Parallele Kopie aus Oracle“](#parallel-copy-from-oracle). | Nein |

**Beispiel: Kopieren von Daten mit einer grundlegenden Abfrage ohne Partition**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
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
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Weitere Beispiele finden Sie im Abschnitt [„Parallele Kopie aus Oracle“](#parallel-copy-from-oracle).

### <a name="oracle-as-a-sink-type"></a>Oracle als Senkentyp

Legen Sie zum Kopieren von Daten in Oracle den Senkentyp in der Kopieraktivität auf **OracleSink** fest. Die folgenden Eigenschaften werden im Abschnitt **sink** der Kopieraktivität unterstützt.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf **OracleSink** festgelegt werden. | Ja |
| writeBatchSize | Fügt Daten in die SQL-Tabelle ein, wenn die Puffergröße "writeBatchSize" erreicht.<br/>Zulässige Werte sind Integer-Werte (Anzahl der Zeilen). |Nein (Standardwert ist 10.000) |
| writeBatchTimeout | Die Wartezeit für den Abschluss der Batcheinfügung, bis das Timeout wirksam wird.<br/>Zulässige Werte sind Timespan-Werte. Beispiel: 00:30:00 (30 Minuten). | Nein |
| preCopyScript | Geben Sie eine SQL-Abfrage an, die bei jeder Ausführung von der Kopieraktivität ausgeführt werden soll, bevor Daten in Oracle geschrieben werden. Sie können diese Eigenschaft nutzen, um die vorab geladenen Daten zu bereinigen. | Nein |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="parallel-copy-from-oracle"></a>Parallele Kopie von Oracle

Der Data Factory Oracle-Konnektor verfügt über eine integrierte Datenpartitionierung zum leistungsstarken, parallelen Kopieren von Daten aus Oracle. Sie finden Optionen zur Datenpartitionierung unter Kopieraktivität -> Oracle-Quelle:

![Partitionsoptionen](./media/connector-oracle/connector-oracle-partition-options.png)

Wenn Sie die partitionierte Kopie aktivieren, dann führt Data Factory parallel Abfragen Ihrer Oracle-Quelle durch, um Daten in Partitionen zu laden. Der Parallelitätsgrad wird über die **[`parallelCopies`](copy-activity-performance.md#parallel-copy)** -Einstellung der Kopieraktivität konfiguriert und gesteuert. Wenn Sie zum Beispiel `parallelCopies` als vier bestimmen, dann generiert Data Factory gleichzeitig vier Abfragen und führt sie aus. Diese Abfragen basieren auf der von Ihnen angegebenen Partitionsoption und den Einstellungen und ruft einen Teil Ihrer Daten aus Ihrer Oracle-Datenbank auf.

Es ist ratsam, dass Sie die parallele Kopie mit der Datenpartitionierung aktivieren, vor allem, wenn Sie große Mengen Daten aus der Oracle-Datenbank laden wollen. Es folgen einige empfohlene Konfigurationen für verschiedene Szenarien:

| Szenario                                                     | Empfohlene Einstellungen                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Vollständiges Laden aus einer großen Tabelle mit physischen Partitionen          | **Partitionsoption**: Physische Partitionen der Tabelle. <br><br/>Während der Ausführung erkennt Data Factory automatisch die physischen Partitionen und kopiert Daten nach Partitionen. |
| Vollständiges Laden aus einer großen Tabelle ohne physische Partitionen mit einer Integerspalte für die Datenpartitionierung | **Partitionoptionen**: Dynamische Bereichspartitionierung.<br>**Partitionsspalte**: Geben Sie die Spalte für die Datenpartitionierung an. Wenn Sie sie nicht angeben, wird die Primärschlüsselspalte verwendet. |
| Laden von großen Mengen Daten mit einer benutzerdefinierten Abfrage unterhalb der physischen Partitionen | **Partitionsoption**: Physische Partitionen der Tabelle.<br>**Abfrage**: `SELECT * FROM <TABLENAME> PARTITION("?AdfTabularPartitionName") WHERE <your_additional_where_clause>`.<br>**Partitionsname**: Geben Sie den/die Partitionsnamen an, aus dem/denen Daten kopiert werden sollen. Wenn sie nicht angegeben wird, wählt ADF automatisch die physischen Partitionen der Tabelle, die Sie im Oracle-Dataset angegeben haben.<br><br>Während der Ausführung ersetzt Data Factory `?AdfTabularPartitionName` mit dem tatsächlichen Partitionsnamen und schickt es an Oracle. |
| Laden von großen Mengen Daten mit benutzerdefinierter Abfrage ohne physische Partitionen, während eine Integerspalte für die Datenpartitionierung verwendet wird | **Partitionoptionen**: Dynamische Bereichspartitionierung.<br>**Abfrage**: `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Partitionsspalte**: Geben Sie die Spalte für die Datenpartitionierung an. Sie können mit der Spalte mit dem Integer-Datentyp partitionieren.<br>**Obergrenze der Partition** und **Untergrenze der Partition**: Geben Sie an, ob Sie mit der Partitionsspalte filtern möchten, um nur Daten zwischen der Ober- und der Untergrenze zu erhalten.<br><br>Data Factory ersetzt während der Ausführung `?AdfRangePartitionColumnName`, `?AdfRangePartitionUpbound` und `?AdfRangePartitionLowbound` mit dem tatsächlichen Spaltennamen und Wertebereichen für jede Partition, um sie dann an Oracle zu senden. <br>Wenn Ihre Partitionsspalte zum Beispiel „ID“ mit einer Untergrenze von 1 und einer Obergrenze von 80 ist, und die parallele Kopie 4 ist, dann ruft ADF die Daten aus 4 Partitionen mit IDs zwischen [1,20], [21, 40], [41, 60] und [61, 80] ab. |

**Beispiel: Abfrage mit physischer Partition**

```json
"source": {
    "type": "OracleSource",
    "query": "SELECT * FROM <TABLENAME> PARTITION(\"?AdfTabularPartitionName\") WHERE <your_additional_where_clause>",
    "partitionOption": "PhysicalPartitionsOfTable",
    "partitionSettings": {
        "partitionNames": [
            "<partitionA_name>",
            "<partitionB_name>"
        ]
    }
}
```

**Beispiel: Abfrage mit dynamischer Bereichspartition**

```json
"source": {
    "type": "OracleSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column>",
        "partitionLowerBound": "<lower_value_of_partition_column>"
    }
}
```

## <a name="data-type-mapping-for-oracle"></a>Datentypzuordnung für Oracle

Beim Kopieren von Daten aus bzw. nach Oracle werden die folgenden Zuordnungen von Oracle-Datentypen zu Data Factory-Zwischendatentypen verwendet. Weitere Informationen dazu, wie die Kopieraktivität das Quellschema und den Datentyp zur Senke zuordnet, finden Sie unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md).

| Datentyp "Oracle" | Data Factory-Zwischendatentyp |
|:--- |:--- |
| BFILE |Byte[] |
| BLOB |Byte[]<br/>(nur für Oracle 10g und höher unterstützt) |
| CHAR |String |
| CLOB |String |
| DATE |DateTime |
| FLOAT |Dezimal, Zeichenfolge (wenn Genauigkeit > 28) |
| INTEGER |Dezimal, Zeichenfolge (wenn Genauigkeit > 28) |
| LONG |String |
| LONG RAW |Byte[] |
| NCHAR |String |
| NCLOB |String |
| NUMBER |Dezimal, Zeichenfolge (wenn Genauigkeit > 28) |
| NVARCHAR2 |String |
| RAW |Byte[] |
| ROWID |String |
| TIMESTAMP |DateTime |
| TIMESTAMP WITH LOCAL TIME ZONE |String |
| TIMESTAMP WITH TIME ZONE |String |
| UNSIGNED INTEGER |Number |
| VARCHAR2 |String |
| XML |String |

> [!NOTE]
> Die Datentypen INTERVAL YEAR TO MONTH und INTERVAL DAY TO SECOND werden nicht unterstützt.


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).
