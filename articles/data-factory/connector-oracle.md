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
ms.date: 02/01/2019
ms.author: jingwang
ms.openlocfilehash: 3fa7612b9e4cd8a714e60879229bd0d39349494f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60405935"
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Kopieren von Daten aus und nach Oracle mit Azure Data Factory
> [!div class="op_single_selector" title1="Wählen Sie die von Ihren verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-onprem-oracle-connector.md)
> * [Aktuelle Version](connector-oracle.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus einer und in eine Oracle-Datenbank zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einer Oracle-Datenbank in jeden unterstützten Senkendatenspeicher kopieren. Zudem können Sie Daten aus jedem unterstützten Quelldatenspeicher in eine Oracle-Datenbank kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Dieser Oracle-Connector unterstützt insbesondere die folgenden Oracle-Datenbankversionen. Standard- oder OID-Authentifizierungen werden ebenfalls unterstützt:

- Oracle 12c R1 (12.1)
- Oracle 11g R1, R2 (11.1, 11.2)
- Oracle 10g R1, R2 (10.1, 10.2)
- Oracle 9i R1, R2 (9.0.1, 9.2)
- Oracle 8i R3 (8.1.7)

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

Legen Sie zum Kopieren von Daten aus Oracle den Quelltyp in der Kopieraktivität auf **OracleSource** fest. Die folgenden Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **OracleSource** festgelegt werden. | Ja |
| oracleReaderQuery | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Ein Beispiel ist `"SELECT * FROM MyTable"`. | Nein |

Ohne Angabe von „oracleReaderQuery“ werden die im Abschnitt „structure“ des Datasets definierten Spalten zum Erstellen einer Abfrage (`select column1, column2 from mytable`) verwendet, die in der Oracle-Datenbank ausgeführt wird. Falls die Datasetdefinition keinen Abschnitt „structure“ enthält, werden alle Spalten der Tabelle ausgewählt.

**Beispiel:**

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
